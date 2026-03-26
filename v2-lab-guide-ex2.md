# Exercise 2 — Deploy a Windows Virtual Machine
### ⏱️ Estimated Time: 65 Minutes (Deployment + Verification + Submission)

---

## Overview

This is the main challenge — and the heart of the Contoso Hackathon. Your team will use the **Azure CLI** to deploy a **Windows Virtual Machine** into Contoso's Azure subscription, entirely through scripted commands.

You will build each component of the VM step by step, just like a real cloud engineer would on the job.

By the end of this exercise your team will have:
- A running Windows VM deployed via Azure CLI
- A complete submission script documenting your work
- A verified deployment ready for proctor review

---

## 🧠 What You Are Building

```
Resource Group: ODL-contosohack-XXXXXX
│
├── 📡 Public IP Address     : contoso-public-ip
├── 🌐 Virtual Network       : contoso-vnet  (10.0.0.0/16)
│   └── Subnet               : contoso-subnet  (10.0.1.0/24)
├── 🔌 Network Interface     : contoso-nic
│   ├── Connected to         : contoso-subnet
│   └── Associated with      : contoso-public-ip
│
└── 🖥️  Virtual Machine       : contoso-vm
    ├── OS                   : Windows Server 2022 Datacenter
    ├── Size                 : Standard_B2s
    ├── NIC                  : contoso-nic (attached)
    └── OS Disk              : contoso-vm-osdisk
```

---

## Task 1 — Set Your Variables (5 mins)

Make sure your terminal is open and your `$RG` variable is still set from Exercise 1. If you opened a new terminal window, re-run your `az login` command first, then set all variables:

```bash
# Core variables — update $RG with your actual Resource Group name
$RG           = "ODL-contosohack-XXXXXX"
$LOCATION     = "eastus"

# Networking
$VNET_NAME    = "contoso-vnet"
$SUBNET_NAME  = "contoso-subnet"
$PUBLIC_IP    = "contoso-public-ip"
$NIC_NAME     = "contoso-nic"

# Virtual Machine
$VM_NAME      = "contoso-vm"
$VM_SIZE      = "Standard_B2s"
$ADMIN_USER   = "contosoadmin"
$ADMIN_PASS   = "ContosoH@ck2024!"

# Confirm all variables
Write-Host "=== Variables Set ===" -ForegroundColor Green
Write-Host "RG       : $RG"
Write-Host "Location : $LOCATION"
Write-Host "VM Name  : $VM_NAME"
Write-Host "VM Size  : $VM_SIZE"
```

✅ **Expected output:** All 4 lines printed with your correct values.

> ⚠️ **Password Note:** Azure VM passwords must be at least 12 characters and include uppercase, lowercase, a number, and a special character. The password above already meets these requirements. Save it in Notepad.

---

## Task 2 — Create a Virtual Network and Subnet (10 mins)

Every VM needs a network to live in. Create Contoso's Virtual Network first:

```bash
az network vnet create `
    --name $VNET_NAME `
    --resource-group $RG `
    --location $LOCATION `
    --address-prefix "10.0.0.0/16"
```

✅ **Expected output:** JSON with `"provisioningState": "Succeeded"` and name `contoso-vnet`.

Now create a Subnet inside the VNet — this is where the VM's network card will connect:

```bash
az network vnet subnet create `
    --name $SUBNET_NAME `
    --resource-group $RG `
    --vnet-name $VNET_NAME `
    --address-prefix "10.0.1.0/24"
```

✅ **Expected output:** JSON with `"provisioningState": "Succeeded"` and name `contoso-subnet`.

---

## Task 3 — Create a Public IP Address (5 mins)

Your VM needs a public IP so it can be identified and accessed on the internet:

```bash
az network public-ip create `
    --name $PUBLIC_IP `
    --resource-group $RG `
    --location $LOCATION `
    --allocation-method Static `
    --sku Standard
```

✅ **Expected output:** JSON with `"provisioningState": "Succeeded"`.

Note down the assigned Public IP address:

```bash
az network public-ip show `
    --name $PUBLIC_IP `
    --resource-group $RG `
    --query "ipAddress" `
    --output tsv
```

Copy this IP address into your Notepad — you will use it during verification.

---

## Task 4 — Create a Network Interface Card (NIC) (5 mins)

The NIC is the component that connects your VM to both the VNet and the Public IP — think of it as the VM's network plug:

```bash
az network nic create `
    --name $NIC_NAME `
    --resource-group $RG `
    --location $LOCATION `
    --vnet-name $VNET_NAME `
    --subnet $SUBNET_NAME `
    --public-ip-address $PUBLIC_IP
```

✅ **Expected output:** JSON with `"provisioningState": "Succeeded"` and the NIC showing it is connected to `contoso-subnet`.

---

## Task 5 — Deploy the Virtual Machine (15 mins)

This is the main event. Run this command carefully — double-check every value matches your variables:

```bash
az vm create `
    --name $VM_NAME `
    --resource-group $RG `
    --location $LOCATION `
    --nics $NIC_NAME `
    --image "Win2022Datacenter" `
    --size $VM_SIZE `
    --admin-username $ADMIN_USER `
    --admin-password $ADMIN_PASS `
    --os-disk-name "contoso-vm-osdisk" `
    --storage-sku "Standard_LRS"
```

> ⏱️ **This will take approximately 5–8 minutes.** The CLI will show a running progress indicator — do not close the terminal or press Ctrl+C. This is a good time for your team to review the commands used so far and prepare your submission script.

✅ **Expected output:**
```json
{
  "powerState": "VM running",
  "publicIpAddress": "<your-public-ip>",
  "resourceGroup": "ODL-contosohack-XXXXXX",
  "location": "eastus"
}
```

🎉 If you see `"powerState": "VM running"` — your team has successfully deployed a Virtual Machine on Azure using the CLI!

---

## Task 6 — Verify the Deployment (10 mins)

Run the full verification block to confirm everything is in place:

```bash
Write-Host "======================================" -ForegroundColor Cyan
Write-Host "  CONTOSO HACKATHON - DEPLOYMENT CHECK " -ForegroundColor Cyan
Write-Host "======================================" -ForegroundColor Cyan

Write-Host "`n[ Virtual Machine ]" -ForegroundColor Yellow
az vm list --resource-group $RG --output table

Write-Host "`n[ VM Power State ]" -ForegroundColor Yellow
az vm get-instance-view `
    --name $VM_NAME `
    --resource-group $RG `
    --query "instanceView.statuses[1].displayStatus" `
    --output tsv

Write-Host "`n[ Virtual Network ]" -ForegroundColor Yellow
az network vnet list --resource-group $RG --output table

Write-Host "`n[ Public IP Address ]" -ForegroundColor Yellow
az network public-ip list --resource-group $RG --output table

Write-Host "`n[ All Resources in Resource Group ]" -ForegroundColor Yellow
az resource list --resource-group $RG --output table

Write-Host "`n======================================" -ForegroundColor Green
Write-Host "         VERIFICATION COMPLETE!        " -ForegroundColor Green
Write-Host "======================================" -ForegroundColor Green
```

✅ **Everything is correct when you see:**
- `contoso-vm` listed in VM list
- VM power state shows `VM running`
- `contoso-vnet` listed in VNet list
- Public IP listed with an assigned IP address
- All resources shown under your Resource Group

---

## Task 7 — Save Your Submission Script (5 mins)

Create a single script file containing every CLI command your team used:

```bash
# Create submission folder on the VM Desktop
New-Item -ItemType Directory -Path "$env:USERPROFILE\Desktop\contoso-submission" -Force

# Open Notepad to write your submission script
notepad "$env:USERPROFILE\Desktop\contoso-submission\team-script.ps1"
```

In Notepad, paste all commands used from Exercise 1 and Exercise 2 in order. Your script should include:

```
# ================================================
# CONTOSO HACKATHON 2024 — TEAM SUBMISSION SCRIPT
# Team Name    : [Your Team Name]
# Members      : [List all member names]
# Date         : [Today's date]
# ================================================

# --- EXERCISE 1: Authentication ---
az login --service-principal ...
az account set ...

# --- EXERCISE 2: VM Deployment ---
az network vnet create ...
az network vnet subnet create ...
az network public-ip create ...
az network nic create ...
az vm create ...
```

Save the file. This is your team's official submission artefact.

---

## ⭐ Bonus Task — Stop and Restart the VM via CLI

> 💡 Complete this only if your team has time remaining. It earns extra points in judging.

Stop (deallocate) the VM:

```bash
az vm deallocate --name $VM_NAME --resource-group $RG
```

Verify it stopped:

```bash
az vm get-instance-view `
    --name $VM_NAME `
    --resource-group $RG `
    --query "instanceView.statuses[1].displayStatus" `
    --output tsv
```

✅ Expected: `VM deallocated`

Start it again:

```bash
az vm start --name $VM_NAME --resource-group $RG
```

Verify it is running again:

```bash
az vm get-instance-view `
    --name $VM_NAME `
    --resource-group $RG `
    --query "instanceView.statuses[1].displayStatus" `
    --output tsv
```

✅ Expected: `VM running`

---

## 📋 Final Submission Checklist

Work through this as a team before calling the proctor:

### Core Tasks — Required

- [ ] `az login --service-principal` — authenticated successfully
- [ ] `az network vnet create` — `contoso-vnet` deployed
- [ ] `az network vnet subnet create` — `contoso-subnet` created
- [ ] `az network public-ip create` — Public IP assigned
- [ ] `az network nic create` — NIC connected to subnet
- [ ] `az vm create` — `contoso-vm` deployed and running
- [ ] Verification script run — all resources confirmed present
- [ ] Submission script saved to Desktop

### Bonus Tasks — Extra Points

- [ ] VM successfully stopped and restarted via CLI
- [ ] All team members contributed at least one command
- [ ] Submission script is clean, commented, and organized

---

## 🏆 Judging Criteria

| Criteria | Weight |
|---|---|
| Correct SP authentication | 20% |
| VM deployed and running | 40% |
| CLI script quality and completeness | 20% |
| Bonus tasks completed | 10% |
| Team collaboration and explanation | 10% |

---

## 📣 Notify Your Proctor

Once your checklist is complete:

1. Raise your hand or message the event proctor
2. Share your terminal screen showing the verification output
3. Walk the proctor through your submission script — explain what each command does
4. The proctor will confirm your submission and record your team's completion time

---

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🎉 You did it! Contoso's first Azure VM
     is live — and your team built it.

  "The best infrastructure is the kind
    you never had to click to create."

  — Congratulations from Spectra Systems
    and your sponsors at [Sponsor Name] —
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

*← Back to Exercise 1 &nbsp;|&nbsp; 🏁 Lab Complete!*
