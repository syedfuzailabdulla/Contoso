# Exercise 1 — Authenticate to Azure Using the Service Principal
### ⏱️ Estimated Time: 35 Minutes (Getting Started + Authentication)

---

## Overview

Before your team can deploy anything on Azure, two things must happen — you need to access your workspace and then log in to Azure using the **Service Principal** that has been automatically created for your team.

This exercise covers both. By the end, your team will be fully authenticated and ready to deploy.

---

## Task 1 — Access Your Windows 11 VM (15 mins)

### Step 1 — Open the VM
1. On your **lab details page**, click the **Launch** button under your Windows 11 VM
2. The VM will open in a new browser tab — no RDP client or additional software needed
3. Wait for the desktop to fully load

### Step 2 — Open the Terminal
1. Click the **Start Menu** on the Windows 11 VM
2. Search for **Windows Terminal** and open it
3. Verify Azure CLI is installed by running:

    ```bash
    az --version
    ```

    ✅ **Expected output:** Azure CLI version number displayed (e.g. `azure-cli 2.x.x`)

    ❌ **If you get an error:** Raise your hand for a proctor immediately — do not proceed without a working CLI.

### Step 3 — Note Your Service Principal Credentials
1. Go back to your **lab details page** (the CloudLabs browser tab)
2. Locate and copy all four credential values:

    ```
    Application (Client) ID  : <your-app-id>
    Client Secret            : <your-client-secret>
    Tenant ID                : <your-tenant-id>
    Subscription ID          : <your-subscription-id>
    Resource Group Name      : ODL-contosohack-XXXXXX
    ```

3. Open **Notepad** on the Windows 11 VM and paste all credentials there
4. Keep this Notepad window open throughout the hackathon — your whole team will reference it

### Step 4 — Assign Team Roles

Before writing a single command, divide responsibilities:

| Role | Responsibility |
|---|---|
| **Team Lead** | Coordinates tasks, tracks time, handles final submission |
| **CLI Engineer** | Types and runs the Azure CLI commands |
| **Reviewer** | Watches each command output, spots errors |
| **Documenter** | Notes down every command used for the submission script |

> 💡 Rotate the CLI Engineer role between exercises so all members get hands-on experience — this counts toward your collaboration score!

---

## Task 2 — Authenticate to Azure Using the Service Principal (20 mins)

### 🧠 Why a Service Principal?

```
Normal Human Login          Service Principal Login
──────────────────          ────────────────────────────
Username + Password    →    Application ID + Client Secret
Manual sign-in         →    Non-interactive / scripted
For people             →    For automation & scripts ✅
```

In the real world, scripts and automation tools never log in as humans. They use a **Service Principal** — a robot identity with just enough permissions to do its job. That is exactly what your team will use now.

---

### Step 1 — Log In Using the Service Principal

In your Windows Terminal, run the following command. Replace each placeholder with your actual values from Notepad:

```bash
az login --service-principal `
         --username "<Application-Client-ID>" `
         --password "<Client-Secret>" `
         --tenant "<Tenant-ID>"
```

**Example with sample values:**
```bash
az login --service-principal `
         --username "a1b2c3d4-1234-5678-abcd-ef1234567890" `
         --password "xYz~AbCdEfGhIjKlMnOpQrStUvWxYz123456" `
         --tenant "f1e2d3c4-abcd-1234-5678-9abcdef01234"
```

✅ **Expected output:** A JSON block showing your subscription name and `"state": "Enabled"` — you are logged in!

❌ **If you see an authentication error:** Check that you copied the credentials exactly — the Client Secret is case-sensitive and must have no extra spaces.

---

### Step 2 — Set Your Active Subscription

Tell Azure CLI exactly which subscription to work with:

```bash
az account set --subscription "<Subscription-ID>"
```

Confirm it is set correctly:

```bash
az account show --output table
```

✅ **Expected output:** A table showing your Subscription ID, subscription name, and `"State": "Enabled"`.

---

### Step 3 — Verify Access to Your Resource Group

Confirm your Service Principal can see and access the pre-created Resource Group:

```bash
az group list --output table
```

✅ **Expected output:** A table showing `ODL-contosohack-XXXXXX` with `"ProvisioningState": "Succeeded"`.

Now store your Resource Group name as a variable — you will use it in every command from here on:

```bash
# Save to a variable — replace with your actual RG name
$RG       = "ODL-contosohack-XXXXXX"
$LOCATION = "eastus"

# Confirm variables are set
Write-Host "Resource Group : $RG"
Write-Host "Location       : $LOCATION"
```

✅ **Expected output:** Both values printed to the terminal without errors.

---

## ✅ Exercise 1 Checklist

Tick every item before moving to Exercise 2:

- [ ] Windows 11 VM is open and accessible in the browser
- [ ] `az --version` runs successfully — CLI is working
- [ ] SP credentials are saved in Notepad on the VM
- [ ] Team roles have been assigned
- [ ] `az login --service-principal` completed with no errors
- [ ] `az account show` displays your correct Subscription ID
- [ ] `az group list` shows your Resource Group
- [ ] `$RG` and `$LOCATION` variables are set in the terminal

---

*← Back to Introduction &nbsp;|&nbsp; Continue to Exercise 2 →*
