---
layout: page
author: Felix Eyetan
title: Azure Bastion Capabilities
level: Intermediate
description: "Deploying a Bastion host, premium SKU in Azure with file transfer capabilities, RBAC and Native client support"
category: azure
is_blog: false
---

## PoC Plan Overview
This project demonstrates advanced capabilities of **Azure Bastion Premium SKU** in a hub-and-spoke network architecture.<br>

The focus is on secure, role-based access to virtual machines (VMs) in spoke VNets, leveraging Entra ID authentication, Conditional Access policies, session recording, and file transfer with granular permissions.

To learn more about Azure bastion, check out the official documentation [here](https://learn.microsoft.com/en-us/azure/bastion/).
***

### **Architecture Diagram**
[TODO: Insert Architecture Diagram Image Here]

### Our Goals

* Entra ID authentication with Conditional Access
* Role-based access integrated with Access Packages
* Private access only (VPN required)
* Bastion Premium SKU in hub VNet
* Session recording (video + command logs) stored in encrypted Storage Account (7-day retention)
* File transfer with role-based restrictions
* Native client (CLI + SSH/RDP) and browser access via Shareable Link
* Scalable for \~50 users
* Monitoring via Azure Monitor + Log Analytics

### Our limitations
* No public IP on Bastion (Private IP only)
* No direct internet access to VMs (VPN required)
* Azure P1 licenses for Conditional Access

***

### **Step-by-Step Execution Plan**

#### **1. Prerequisites**

*   Confirm hub-and-spoke network setup with VNet peering.
*   Ensure VPN gateway is configured and tested.
*   Prepare Entra ID groups for:
    *   **Bastion-Connect** (basic access)
    *   **Bastion-UploadDownload** (full file transfer)
    *   **Bastion-UploadOnly** (restricted file transfer)
*   Validate Access Package workflow for these groups.

***

#### **2. Deploy Azure Bastion Premium**

*   Deploy **Azure Bastion Premium SKU** in the **hub VNet**.
*   Enable:
    *   **Native Client Support**
    *   **Shareable Link**
    *   **File Transfer**
    *   **Session Recording**
*   Configure **Private IP only** (disable public IP).
*   Integrate Bastion with **Private Link** for extra isolation.

***

#### **3. Configure Authentication & Conditional Access**

*   Assign Bastion access roles via **Entra ID RBAC**:
    *   `Reader` or `Virtual Machine User Login` for basic access.
*   Apply **Conditional Access Policy**:
    *   Require MFA
    *   Require compliant device
    *   Require VPN IP range
*   Integrate with **Access Packages** for JIT access.

***

#### **4. Session Recording & Storage**

*   Enable **Session Recording** in Bastion Premium.
*   Create **Storage Account**:
    *   Enable encryption (Microsoft-managed keys or CMK if needed).
    *   Configure **Lifecycle Management** for 7-day retention.
*   Link Bastion to Storage Account for session logs and video replay.

***

#### **5. File Transfer Policy**

*   Enable **Upload/Download** in Bastion Premium.
*   Use **Custom RBAC or Entra ID groups**:
    *   Group A: Upload + Download
    *   Group B: Upload only
*   Validate Access Package workflow for these roles.

***

#### **6. Client Access**

*   Enable **Native Client Support**:
    *   SSH via `az network bastion ssh`
    *   RDP via `az network bastion rdp`
*   Enable **Shareable Link**:
    *   Generate link for engineers to bookmark.
    *   Ensure link works without portal login (still Entra-authenticated).

***

#### **7. Monitoring & SOC Integration**

*   Enable **Diagnostic Settings** for Bastion:
    *   Send logs to **Log Analytics Workspace**.
*   Configure **Azure Monitor Alerts** for:
    *   Session start/stop
    *   Failed login attempts
*   Optional: Integrate with **Sentinel** for SOC visibility.

***

#### **8. Demo Scenarios**

*   Engineer requests access via Access Package → gets role → connects via Bastion.
*   Show:
    *   Browser access via Shareable Link
    *   Native client SSH/RDP
    *   File upload/download based on role
    *   Session recording replay from Storage Account
    *   Conditional Access enforcement (VPN required)
    *   Monitoring dashboard in Log Analytics

***

### Cost Estimation

**Cost Estimation (Monthly)**
- Azure Bastion Premium SKU: ~$0.45/hour → ~$324/month
- Storage Account (100 GB): ~$1.80/month
- Log Analytics (1 GB/day): ~$69/month
  **Total: ~$394.80/month**
*Note: Actual costs may vary based on usage and region.*
<br>*Refer to [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) for detailed estimates.*

### **Next Steps**

Check out the project code on this [GitHub Repository](https://github.com/madjava/azure-bastion.git) to get started with deploying your own Azure Bastion PoC with these advanced capabilities!