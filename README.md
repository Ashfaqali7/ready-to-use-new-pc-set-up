# ready-to-use-new-pc-set-up

1. Installs all **developer tools + common software** (as before).
2. **Configures key developer settings** automatically:

   * **Git**: username, email, default branch, credential manager
   * **VS Code**: installs popular extensions
   * **Node.js**: installs useful global packages
   * **Python**: upgrades pip, installs virtualenv
   * **Android Studio**: sets environment variables
3. Sets **PATH** for important tools if needed.

Here’s the **ultimate “one-click dev setup” script**:

---

```powershell
# ========================================
# Ultimate Dev + Software Setup Script
# Fully Configured Environment
# ========================================

# --- Step 0: Ensure Admin Privileges ---
If (-NOT ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltinRole] "Administrator")) {
    Write-Warning "Please run PowerShell as Administrator."
    Break
}

# --- Step 1: Install Chocolatey ---
Write-Host "`nInstalling Chocolatey..."
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
refreshenv

# --- Step 2: Upgrade Chocolatey ---
Write-Host "`nUpgrading Chocolatey..."
choco upgrade chocolatey -y

# --- Step 3: Define Packages ---
$packages = @(
    # Developer Tools
    "git","sourcetree","github-desktop",
    "vscode","visualstudio2022community","jetbrains-toolbox","androidstudio",
    "nodejs-lts","python","openjdk17","dotnet-sdk",
    # Databases
    "mongodb","mysql","postgresql","redis",
    # Browsers
    "googlechrome","firefox","edge",
    # Productivity
    "7zip","notepadplusplus","slack","zoom","microsoft-teams","spotify","libreoffice",
    # Media / Utilities
    "vlc","gimp","audacity","handbrake","greenshot","powershell-core","microsoft-windows-terminal","obs-studio","paint.net",
    # DevOps / Automation
    "docker-desktop","kubernetes-cli","terraform","n8n"
)

# --- Step 4: Install Packages in Parallel ---
Write-Host "`nInstalling all packages..."
$jobs = @()
foreach ($pkg in $packages) {
    $jobs += Start-Job -ScriptBlock { param($p) choco install $p -y } -ArgumentList $pkg
}

$jobs | ForEach-Object { Wait-Job $_; Receive-Job $_ }

# --- Step 5: Upgrade All Packages ---
Write-Host "`nUpgrading all packages..."
choco upgrade all -y

# --- Step 6: Configure Git ---
Write-Host "`nConfiguring Git..."
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
git config --global credential.helper manager-core

# --- Step 7: Configure Node.js ---
Write-Host "`nInstalling global Node.js packages..."
npm install -g yarn nodemon typescript eslint prettier

# --- Step 8: Configure Python ---
Write-Host "`nUpgrading pip and installing virtualenv..."
python -m pip install --upgrade pip
pip install virtualenv

# --- Step 9: Configure VS Code ---
Write-Host "`nInstalling popular VS Code extensions..."
$extensions = @(
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-vscode.vscode-typescript-next",
    "ms-python.python",
    "ms-azuretools.vscode-docker",
    "msjsdiag.debugger-for-chrome",
    "eamodio.gitlens",
    "pkief.material-icon-theme"
)
foreach ($ext in $extensions) {
    code --install-extension $ext --force
}

# --- Step 10: Configure Android Studio ---
Write-Host "`nSetting Android SDK environment variables..."
$androidSdkPath = "$env:LOCALAPPDATA\Android\Sdk"
[Environment]::SetEnvironmentVariable("ANDROID_HOME", $androidSdkPath, "User")
$env:Path += ";$androidSdkPath\platform-tools;$androidSdkPath\tools;$androidSdkPath\tools\bin"

# --- Step 11: Final Message ---
Write-Host "`n✅ Development environment is fully installed and configured!"
Write-Host "Restart your terminal/PowerShell to apply environment variable changes."
```

---

### ✅ **Features of This Script**

* Installs **all essential developer tools** + **common software**

* **Parallel installation** to save time

* Configures:

  * Git (username, email, branch, credential helper)
  * Node.js global packages (yarn, nodemon, TypeScript, ESLint, Prettier)
  * Python pip + virtualenv
  * VS Code extensions for web development
  * Android SDK environment variables

* Upgrades all packages automatically

---

### **How to Use**

1. Save as `dev-setup.ps1`.
2. Open **PowerShell as Administrator**.
3. Navigate to the script folder.
4. Run:

```powershell
.\dev-setup.ps1
```

5. Once finished, **restart PowerShell** to apply environment variables.


