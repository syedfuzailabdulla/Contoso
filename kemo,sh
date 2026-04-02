#!/bin/bash
# ============================================================
# CloudLabs Template VM Setup Script
# OS      : Ubuntu 22.04 LTS
# Software: VS Code, Azure CLI, Google Chrome
# Author  : Spektra Systems - CloudLabs Engineering
# ============================================================

set -e
LOG="/var/log/cloudlabs-setup.log"
exec > >(tee -a "$LOG") 2>&1

echo "============================================"
echo " CloudLabs Template VM Setup Starting..."
echo " $(date)"
echo "============================================"

# ------------------------------------------------------------
# 1. System Update
# ------------------------------------------------------------
echo "[1/6] Updating system packages..."
apt-get update -y
apt-get upgrade -y
apt-get install -y \
    curl \
    wget \
    apt-transport-https \
    ca-certificates \
    gnupg \
    lsb-release \
    software-properties-common \
    unzip \
    git \
    xrdp \
    ubuntu-desktop-minimal \
    xfce4 \
    xfce4-goodies

echo "[1/6] System update complete ✅"

# ------------------------------------------------------------
# 2. Configure XRDP for browser-based RDP access (CloudLabs DWC)
# ------------------------------------------------------------
echo "[2/6] Configuring XRDP for Remote Desktop..."
systemctl enable xrdp
systemctl start xrdp

# Allow RDP through UFW if active
ufw allow 3389/tcp 2>/dev/null || true

# Set xfce4 as default session for xrdp
echo "xfce4-session" > /etc/skel/.xsession
echo "startxfce4" >> /etc/xrdp/startwm.sh

# Fix xrdp color depth issue
sed -i 's/max_bpp=32/max_bpp=24/g' /etc/xrdp/xrdp.ini
sed -i 's/xserverbpp=24/xserverbpp=24/g' /etc/xrdp/xrdp.ini

systemctl restart xrdp
echo "[2/6] XRDP configured ✅"

# ------------------------------------------------------------
# 3. Install Visual Studio Code
# ------------------------------------------------------------
echo "[3/6] Installing Visual Studio Code..."
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /usr/share/keyrings/packages.microsoft.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] \
    https://packages.microsoft.com/repos/code stable main" \
    > /etc/apt/sources.list.d/vscode.list

apt-get update -y
apt-get install -y code

# Install useful VS Code extensions for all users
sudo -u "$SUDO_USER" code --install-extension ms-azuretools.vscode-azureresourcegroups 2>/dev/null || true
sudo -u "$SUDO_USER" code --install-extension ms-vscode.azurecli 2>/dev/null || true
sudo -u "$SUDO_USER" code --install-extension ms-vscode.azure-account 2>/dev/null || true

echo "[3/6] Visual Studio Code installed ✅"

# ------------------------------------------------------------
# 4. Install Azure CLI
# ------------------------------------------------------------
echo "[4/6] Installing Azure CLI..."
curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Verify installation
az --version
echo "[4/6] Azure CLI installed ✅"

# ------------------------------------------------------------
# 5. Install Google Chrome
# ------------------------------------------------------------
echo "[5/6] Installing Google Chrome..."
wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -O /tmp/chrome.deb
apt-get install -y /tmp/chrome.deb
rm -f /tmp/chrome.deb

# Create desktop shortcut for all users
cat > /usr/share/applications/google-chrome.desktop << 'EOF'
[Desktop Entry]
Version=1.0
Name=Google Chrome
Exec=/usr/bin/google-chrome-stable %U --no-sandbox
Terminal=false
Icon=google-chrome
Type=Application
Categories=Network;WebBrowser;
EOF

echo "[5/6] Google Chrome installed ✅"

# ------------------------------------------------------------
# 6. CloudLabs Idle Tracker Prep & Final Cleanup
# ------------------------------------------------------------
echo "[6/6] Final cleanup and prep..."

# Clean apt cache
apt-get autoremove -y
apt-get autoclean -y
rm -rf /tmp/*
rm -rf /var/tmp/*

# Clear bash history
history -c
cat /dev/null > ~/.bash_history

# Clear logs for clean image capture
find /var/log -type f -exec truncate -s 0 {} \;

echo "[6/6] Cleanup complete ✅"

echo "============================================"
echo " CloudLabs Template VM Setup COMPLETE"
echo " $(date)"
echo " Installed:"
echo "   ✅ Ubuntu Desktop (XFCE4)"
echo "   ✅ XRDP (Remote Desktop)"
echo "   ✅ Visual Studio Code"
echo "   ✅ Azure CLI"
echo "   ✅ Google Chrome"
echo "============================================"
echo ""
echo " NEXT STEP: Shut down this VM and export"
echo " the image from the CloudLabs Template tab."
