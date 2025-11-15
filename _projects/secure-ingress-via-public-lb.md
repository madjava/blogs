---
layout: page
author: Felix Eyetan
title: Secure Ingress via Azure Loadbalancer and Palo Alto Firewall
level: Intermediate
description: "Securing ingress traffic via Public load balancer and Palo Alto firewall using a hub-and-spoke architecture on Azure."
category: azure
is_blog: false
---

## Project Summary

Partedwaves Ltd delivers web-based services to a global client base. To support secure and scalable internet-facing applications, they require an Azure-based architecture that enables reliable access to backend services developed by their engineering team.

They are considering an Azure solution for regional traffic distribution.

Your objective is to design and deploy the necessary infrastructure to securely expose these services to target users, ensuring performance, availability, and protection.

## Pre-requisite and Assumptions

- You have you own Azure account or have and Azure account where you have sufficient permissions to deploy resources.
- You can login to your Azure portal via `az` cli from your terminal.
- To limit complexity of the project, we'll not be deploying via a CI/CD  pipeline.
- You own a domain name that you can use. (optional)
- You have Terraform [installed](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) on your local machine

## What we will build

In this project, we'll design and deploy a secure, scalable hub-and-spoke network architecture in Azure. The setup includes two spoke virtual networks—each hosting a virtual machine—connected to a central hub that provides shared security and connectivity services.

The hub network hosts:

- Palo Alto firewall for traffic inspection
- NAT Gateway for secure outbound internet access
- Azure Bastion for remote administration
- Inbound traffic is routed through Azure Standard loadbalancer.
- East-west traffic between spokes is routed via the hub, enforcing a single control point.

This architecture will demonstrate centralized security, controlled outbound access, and full inspection of north-south and east-west traffic—aligning with Zero Trust principles using Azure-native services and third-party security tooling.

## Architecture

<details>
<summary>Project Architecture Diagram</summary>

<img src="{{ site.url }}/blogs/assets/images/azure/lb-ingress-architecture.png" alt="Secure Ingress via Azure Public loadbalancer architecture diagram" />
</details>

## Project Repository

You can find the project repository the [Secure Ingress via Azure Loadbalancer and Palo Alto Firewall](https://github.com/madjava/secure-ingress-via-public-lb) git repository.

Clone the repo:

```bash
git clone <repo-url>
```

After cloning, follow the `README.md` file and go through the steps.

### Pros and Cons of the Design

#### Pros (Advantages)

* **Centralized Security Control**

    All inbound, outbound, and east-west traffic is funneled through the Palo Alto firewall, enabling consistent inspection, policy enforcement, and unified logging.

* **Simplified Management**

    Shared services like NAT Gateway, Bastion, and Azure load balancer are hosted centrally in the hub—simplifying operations and updates.

* **Enhanced Outbound Security**

    The NAT Gateway provides predictable outbound traffic with static egress IPs, improving security and compliance tracking.

* **Layer 7 Protection**
    
    Palo Alto firewall adds [application-layer](https://docs.paloaltonetworks.com/pan-os/10-1/pan-os-new-features/url-filtering-features/advanced-url-filtering) (L7) defense capability, protecting against web attacks such as SQL injection, XSS, and DDoS capabilities to ingress and egress flows.

* **Improved East-West Visibility**

    With no direct spoke-to-spoke peering, all lateral traffic passes through the hub, improving visibility and securing lateral movement.

* **Scalable and Modular**

    New spokes or workloads can be added seamlessly—each benefits from the existing centralized controls and security stack.

* **Zero Trust Alignment**

    The design enforces segmentation, centralized inspection, and least-privilege access—core Zero Trust principles.

#### Cons (Trade-offs / Limitations)

* **Increased Complexity**

    The architecture requires careful configuration of UDRs, load balancers, and firewall policies, increasing engineer administration.

* **Added Latency**

    Routing all east-west traffic through the hub introduces an additional hop and potential latency as the firewall has to perform configured traffic inspection.

* **Higher Costs**

    Multiple Azure services e.g. NAT Gateway, Palo Alto firewall, and Bastion—add to overall operational cost.

* **Throughput Bottlenecks**

    Improper sizing of the firewall or load balancer can limit performance under heavy workloads.

* **Reliance on Custom Routing**

    Azure’s non-transitive VNet peering model requires manual routing (via UDRs) for spoke-to-spoke communication.

* **Single Point of Failure**

    Without redundancy, the hub components (firewall, load balancer) can become single points of failure impacting all spoke connectivity.
* **Learning Curve**

    Teams unfamiliar with Palo Alto firewalls or Azure networking may face a steep learning curve during deployment and management.

## Estimated Cost Considerations

In any medium to large-scale cloud project, understanding and managing infrastructure costs is critical. Cloud operational expenses can escalate rapidly if not planned and monitored effectively. A proactive approach to cost estimation ensures that investments are aligned with business value and technical requirements.

### Common Cost Drivers

#### Networking Costs

* **East-West Traffic:**  Internal service-to-service communication within a region or across zones.
* **North-South Traffic:** Ingress and egress to/from the internet, often impacted by firewall, load balancer, and NAT gateway configurations.
* **Cross-Region Traffic:** Data transfer between Azure regions can be significantly more expensive and should be justified by business continuity or latency requirements.

#### Storage Costs

* **Log Retention:** Services like Log Analytics Workspace and Azure Monitor can accumulate large volumes of logs. Long retention periods and high ingestion rates drive up costs.
* **Blob Storage:** Used for backups, telemetry, and diagnostics. Costs vary by redundancy tier (LRS, GRS) and access patterns (hot, cool, archive).
* **Premium Storage SKUs:** Often used for performance-sensitive workloads but must be justified by actual IOPS requirements.

#### Resource SKU Selection

Choosing the right VM sizes, resource SKUs or licence plans is essential.
Over-provisioning leads to waste; under-provisioning affects performance.
Use Azure Advisor, [Azure pricing calculator](https://azure.microsoft.com/en-gb/pricing/calculator/?ef_id=_k_EAIaIQobChMIkODG8670kAMVspNQBh1XBBs1EAAYASAAEgJVdvD_BwE_k_&OCID=AIDcmm3bvqzxp1_SEM__k_EAIaIQobChMIkODG8670kAMVspNQBh1XBBs1EAAYASAAEgJVdvD_BwE_k_&gad_source=1&gad_campaignid=1634940187&gclid=EAIaIQobChMIkODG8670kAMVspNQBh1XBBs1EAAYASAAEgJVdvD_BwE) and [Palo Alto pricing estimator](https://www.paloaltonetworks.co.uk/resources/tools/ngfw-for-aws-azure-estimator) to guide right-sizing decisions for this architecture.

## Room for Improvement

While this is a common architecture pattern, there is room for improvement or adaptation. Happy to hear your ideas.

## Next steps

- **Challenge yourself:** Attempt to deploy the project code via [Azure DevOps](https://azure.microsoft.com/en-us/products/devops/pipelines) pipeline, [GitHub Actions](https://azure.github.io/actions/#home), [Jenkins](https://www.jenkins.io/doc/pipeline/tour/deployment/), Argos CD or whatever CI tool you are comfortable with.
- **Redundancy & High Availability:** How can you improve on this, where do you see single points of failure in this design and how can you mitigate against that
- **Further learning:**
  - Azure has lots of great content regarding [network architectures](https://learn.microsoft.com/en-us/azure/architecture/networking/)
   - [Palo Alto reference architecture](url) is another great source of various patterns and for many use cases


## Conclusion
In this project, we designed and deployed a secure hub-and-spoke architecture on Azure, leveraging a public load balancer and Palo Alto firewall to manage ingress traffic. This setup ensures that all inbound, outbound, and east-west traffic is inspected and controlled, aligning with Zero Trust principles. The architecture provides centralized security, simplified management, and enhanced visibility into network traffic, while also addressing potential challenges such as latency and complexity. By implementing this design, organizations can achieve a robust and scalable infrastructure that meets their security and performance needs.

Feel free to reach out via [GitHub](https://github.com/madjava/) or [LinkedIn](www.linkedin.com/in/eyetanfelix) if you have any questions or need further assistance.
