# Contoso Hackathon 2024 — Lab Guide
### Sponsored by [Sponsor Name] | Powered by Microsoft Azure
### Managed by Spectra Systems on CloudLabs

---

> **⏱️ Total Duration:** 120 Minutes &nbsp;|&nbsp; **👥 Team Size:** 3–4 Members &nbsp;|&nbsp; **☁️ Platform:** Microsoft Azure &nbsp;|&nbsp; **🛠️ Tool:** Azure CLI

---

## 🌐 Problem Statement

### Background

**Contoso Ltd.** is a fast-growing technology startup that has recently made the decision to move its operations to the **Microsoft Azure Cloud**. The company's IT team is small and overstretched — they need a group of talented cloud engineers to help them provision their first virtual machine on Azure, entirely through automation.

Contoso's CTO has one strict requirement:

> *"No one is allowed to click around in the Azure Portal to create resources. Everything must be scripted, automated, and repeatable."*

That is where **your team** comes in.

---

### Your Mission

Your team has been contracted by **Contoso Ltd.** as an external cloud engineering squad. Your job is to use the **Azure CLI**, authenticated securely through a **Service Principal**, to deploy Contoso's very first cloud Virtual Machine.

Your team must:

1. Authenticate to Azure **non-interactively** using the Service Principal credentials provided in your lab environment
2. Deploy a **Windows Virtual Machine** into Contoso's Azure subscription using only CLI commands
3. Verify the VM is running and document your deployment

Contoso's leadership will evaluate your team's submission based on:

- ✅ Correct and secure authentication using the Service Principal
- ✅ Successful deployment of a running Windows Virtual Machine
- ✅ Quality and clarity of the CLI commands used
- ✅ Teamwork — evidence that the whole team contributed
- ✅ Ability to verify and explain what was deployed

---

### The Stakes

> *"Contoso's entire cloud journey starts with this single VM. If your team can automate this today, you can automate anything tomorrow. Build it right. Build it together."*

The team that delivers the most complete, well-structured, and clearly documented VM deployment will walk away as **Contoso Hackathon 2024 Champions**.

---

## 📋 Lab Environment Overview

When your lab launches, the following are automatically provisioned for your team — no setup required:

| Resource | Details |
|---|---|
| **Azure Subscription** | Dedicated — shared across all team members |
| **Windows 11 VM** | Your team's workspace — accessible from the browser |
| **Service Principal** | Auto-created — credentials visible on your lab details page |
| **Resource Group** | Pre-created — all your resources go here |

### Your Service Principal Credentials

Once your lab is live, locate these on your **lab details page** and note them down immediately:

```
Application (Client) ID  : ............................................
Client Secret            : ............................................
Tenant ID                : ............................................
Subscription ID          : ............................................
Resource Group Name      : ............................................
```

> ⚠️ **Important:** These credentials are your team's keys to Azure. Keep them inside your lab environment. Do NOT share them outside your group.

---

## 🗺️ Lab Guide Structure

| # | Exercise | Time | What You Will Do |
|---|---|---|---|
| 0 | Getting Started | 15 mins | Access the VM, verify CLI, note SP credentials, assign roles |
| 1 | Authenticate to Azure | 20 mins | Log in to Azure using the Service Principal |
| 2 | Deploy a Virtual Machine | 45 mins | Use Azure CLI to deploy a Windows VM |
| — | Verify & Submit | 20 mins | Confirm deployment and submit to proctor |
| — | Buffer | 20 mins | Bonus tasks and troubleshooting time |

> ⚠️ **VM Uptime Reminder:** Your Windows 11 workspace VM shuts down automatically after **60 minutes** (half of the 120-minute lab duration). Complete all your CLI work within this window. Your deployed Azure resources will remain intact even after the workspace VM shuts down.

---

*Continue to Exercise 0 →*
