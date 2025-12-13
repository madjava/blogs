---
layout: post
author: Felix Eyetan
title: Managing TLS Policies in Azure Front Door with Terraform and AzAPI
level: Intermediate
is_blog: true
---

Azure Front Door supports end-to-end TLS encryption. When you add a custom domain to Azure Front Door, HTTPS is mandatory, and you must define a TLS policy that controls the TLS protocol version and cipher suites during the handshake.

This is outlined in the official Azure Front Door TLS policy documentation, which explains how Azure Front Door (AFD) manages TLS and cipher suites.

### The Challenge

The documentation clearly states that the predefined TLS policy is `TLSv1.2_2023` under the [Predefined TLS policy](https://learn.microsoft.com/en-us/azure/frontdoor/standard-premium/tls-policy#predefined-tls-policy) section.

As of **March 1, 2025**, Azure Front Door disallows `TLS 1.0` and `TLS 1.1` as minimum versions. To comply, our organization updated the minimum TLS version to `TLS 1.2`.

#### Why TLS 1.2 and not TLS 1.3?

According to the documentation:

> For a minimum TLS version of 1.2, the negotiation will attempt to establish TLS 1.3 first, then fall back to TLS 1.2. The client must support at least one of the supported ciphers to establish an HTTPS connection with Azure Front Door. Azure Front Door chooses a cipher in the listed order from the client-supported ciphers.

In other words, AFD will always try `TLS 1.3` first. If the client supports any advertised cipher, the connection succeeds; otherwise, it falls back to the minimum version (`TLS 1.2` in our case).

#### After Recent ITHCs

Following recent ITHC assessments, our SecOps team raised concerns about weak ciphers being advertised on endpoints—particularly those handling PCI DSS data. They requested that Platform Engineering address this issue.

### AFD Limitations

Reviewing our Azure Front Door configuration revealed that we could switch from `TLSv1.2_2022` to `TLSv1.2_2023` (which uses stronger ciphers). However, making this change manually in the Azure Portal was ineffective because our resources were deployed via Terraform. Any manual updates were reverted during subsequent pipeline runs.

As of **Nov/Dec 2025**, the Terraform provider [`azurerm_cdn_frontdoor_custom_domain`](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_custom_domain) (v4.56.0) does **not** support configuring the TLS policy cipher suite. Azure defaults to `TLSv1.2_2022`, which was surprising given the documentation suggested otherwise (see [Why TLS 1.2 and not TLS 1.3?](#why-tls-12-and-not-tls-13)).

### The Solution

To meet security requirements and stop advertising weaker ciphers, i implemented a workaround using the **AzAPI provider** alongside Terraform.

#### Code Changes

Here’s a simplified extract of the changes to the custom domain resource:

```terraform
resource "azurerm_cdn_frontdoor_custom_domain" "this" {
  ...
  tls {
    certificate_type        = "CustomerCertificate"
    minimum_tls_version     = "TLS12"
    cdn_frontdoor_secret_id = azurerm_cdn_frontdoor_secret.certificate[each.key].id : null
  }

  lifecycle {
    # Prevent Terraform from overwriting TLS policy changes applied via AzAPI
    ignore_changes = [ tls ]
  }
}
```

The `ignore_changes` directive ensures Terraform does not revert TLS policy changes applied by AzAPI. Without this, pipeline runs would reset the configuration, similar to what happens when changes are made manually in the portal.

#### Why AzAPI?

The [documentation](https://registry.terraform.io/providers/Azure/azapi/latest/docs) states:

> The AzAPI provider is a thin layer on top of Azure ARM REST APIs. It complements the AzureRM provider by enabling management of Azure resources not yet supported in AzureRM—such as preview features or advanced configurations.

#### AzAPI Resource for TLS Policy

In addition to the changes mage to the `azurerm_cdn_frontdoor_custom_domain` above, the below azapi code was added.

```terraform
resource "azapi_update_resource" "this" {
  ...
  type        = "Microsoft.Cdn/profiles/customDomains@2025-04-15"
  resource_id = azurerm_cdn_frontdoor_custom_domain.this.id

  body = jsonencode({
    properties = {
      tlsSettings = {
        minimumTlsVersion  = var.minimum_tls_version
        cipherSuiteSetType = var.cipher_suite_policy
        # certificateType   = "ManagedCertificate" (if needed as well)
      }
    }
  })

  depends_on = [
    azurerm_cdn_frontdoor_custom_domain.this
  ]
}
```

By specifying the `"Microsoft.Cdn/profiles/customDomains@2025-04-15"` API version, we gain access to the full `tlsSettings` properties, including `cipherSuiteSetType`. This allows us to configure cipher suites while keeping Terraform pipelines stable i.e. terraform not trying to `destroy` and `recreate` existing resources. This allow us to keep existing code as is but gave the capability to now change the cipher suite used.

#### Variable Definitions with Validation

To make this configurable and reduce errors, i now added validation to the `cipher_suite_policy` variable:

```terraform
variable "minimum_tls_version" {
  type        = string
  description = "The default TLS policy to apply to Front Door custom domain."
  default     = "TLS12" # Could be TLS13 depending on your use case
}

variable "cipher_suite_policy" {
  description = <<-EOT
  TLS policy preset for Azure Front Door custom domains.
  Options:
    - null: Use Azure's default policy
    - "TLS12_2022": More compatible (includes DHE cipher suites)
    - "TLS12_2023": Higher security (may exclude older cipher suites)
  EOT

  type    = string
  default = null # Let Azure decide the default

  validation {
    condition     = var.cipher_suite_policy == null ? true : contains(["TLS12_2022", "TLS12_2023"], var.cipher_suite_policy)
    error_message = "Must be null, 'TLS12_2022', or 'TLS12_2023'"
  }
}
```

This ensures only valid cipher suite policies are used, reducing misconfigurations.

### Validation

A great tool for validation is [SSL Labs Server Test](https://www.ssllabs.com/ssltest/). Enter your domain name, run a scan, and review the results. It provides detailed TLS information, including advertised cipher suites.

**Key Takeaways:**

* Azure Front Door defaults may not align with your security requirements.
* Terraform currently lacks native support for cipher suite configuration.
* AzAPI provides a reliable workaround until the AzureRM provider adds this capability.
* Solution keeps existing resource, but provides more flexibility.
* More code is introduced to existing code base.
* Always validate changes using SSL testing tools.
* Setting minimum TLS to `TLS1.3` maybe all you need and above code changes not needed.
* Minimum TLS as `TLS1.3` may have implications for some clients, if so then above suggestion may work for you.
* These setting don't change much, once set they usually are that way for a while.

### Reference Docs

* [Configure TLS policy on a Front Door custom domain](https://learn.microsoft.com/en-us/azure/frontdoor/standard-premium/tls-policy-configure)
* [azapi_update_resource (Resource)](https://registry.terraform.io/providers/Azure/azapi/latest/docs/resources/update_resource)
* [How to limit Azure Front Door Cipher Suites Manually?](https://learn.microsoft.com/en-us/answers/questions/1296563/how-to-limit-azure-front-door-cipher-suites-manual?page=1#answers)
* [azurerm_cdn_frontdoor - Support for TLS Policy #29215](https://github.com/hashicorp/terraform-provider-azurerm/issues/29215)
* [azurerm_cdn_frontdoor_custom_domain](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_custom_domain)
