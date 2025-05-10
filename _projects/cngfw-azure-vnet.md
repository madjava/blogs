---
layout: page
author: Felix Eyetan
title: Deploying CNGFW in Azure - VNet
level: Intermediate
description: "Deploying a Palo Alto CNFGW firewall in Azure, Hub Spoke deployment and Panorama managed"
is_blog: false
---
This project will work us through deploying Palo Alto in Azure. We'll be deploying this using the Hub and Spoke model and also managing policies via Panorama.

We'll look at the VWan deployment in another project as this is another deployment model in Azure.

## Pre-requisite

Certain things needs to be in place to enable you complete this project.

- A Palo Alto Customer Support Account.
- An Azure account
- Some knowledge of Terraform
- Intermediate knowledge of Azure, especially around networking
- A GitHub account and some knowledge of GitHub Actions
- Azure CLI


## Some concepts

### Hub and Spoke Architecture

Hub and Spoke Architecture is a design model often used in network systems, data management, logistics, and enterprise architecture. It’s structured around a central "hub" that connects to multiple "spokes" (peripheral systems, locations, or processes).

| Advantages ✅   | Disadvantages 🛑|
| ------------- | ------------- |
| Centralised control and management | Single point of failure at the hub |
| Simplifies security and monitoring | Potential bottlenecks |
| Easier maintenance and upgrades in the hub | Higher latency for inter-spoke communication (must go through hub) |

**Table 1:** Some pros and cons of Hub and Spoke architecture. See [What is a Hub-and-Spoke Network](https://visiblenetworklabs.com/2023/06/05/what-is-a-hub-and-spoke-network/) and [xx](https://www.cbtnuggets.com/blog/technology/networking/what-is-hub-and-spoke-topology) blog for some more info on the subject.

### What we will build?

![CNGFW Hub & Spoke Architecture](../../assets/images/azure/cngfw-project.drawio.png)

**Img 1:** Final project architecture

#### Summary

At the end of this project we would have deployed the following

- 2 resource groups
- 3 virtual networks
- 2 virtual machines
- 1 Panorama server
- 1 Jumpbox virtual machine
- 3 Public IPs
- 2 Route tables

### Recommended reference documentations

- [Cloud NGFW for Azure](https://docs.paloaltonetworks.com/cloud-ngfw/azure/cloud-ngfw-for-azure/getting-started-with-cngfw-for-azure)
- [Deploy the Cloud NGFW in a vNET](https://docs.paloaltonetworks.com/cloud-ngfw/azure/cloud-ngfw-for-azure/deploy-cloud-ngfw-for-azure/deploy-cloud-ngfw-in-a-vnet)
- [Panorama Policy Management](https://docs.paloaltonetworks.com/cloud-ngfw/azure/cloud-ngfw-for-azure/panorama-policy-management)
- [Palo Alto Networks PAN-OS Provider](https://registry.terraform.io/providers/PaloAltoNetworks/panos/latest/docs)
- [Azurerm - Palo Alto](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/palo_alto_next_generation_firewall_virtual_network_panorama)

_We will refer to information in these docs from time to time_


### Deployment Phases

- [Setup CI/CD pipeline in Git/GitHub](#setup-cicd-pipeline)
- Deploy the networking components
- Deploy the management components
  - Register the management component
- Deploy CNGFW resource
- Update networking components (network path)
- Configure CNGFW for inbound/outbound flow
- Wrap up and clear down resources

### Setup CI/CD pipeline

ℹ️ _Watch out for more updates coming soon_
