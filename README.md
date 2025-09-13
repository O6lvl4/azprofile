# azprofile-cli

Azure CLI Profile Manager - Easy switching between multiple Azure accounts

## Installation

```bash
npm install -g azprofile-cli
```

Or install from source:
```bash
git clone https://github.com/O6lvl4/azprofile.git
cd azprofile
npm install -g .
```

## Quick Setup (Recommended)

**One-time setup** to enable seamless profile switching:

```bash
# Setup shell integration
azprofile setup ~/.zshrc    # for zsh users
azprofile setup ~/.bashrc   # for bash users

# Restart your shell or run:
source ~/.zshrc
```

After setup, you can use `azprofile use <profile>` **without source command**!

## Usage

### 1. Add Azure profiles

```bash
azprofile add work      # Add work profile
azprofile add personal  # Add personal profile
```

Each profile will prompt you to login to Azure and select subscription/tenant.

### 2. Switch profiles (After Setup)

```bash
# Simple profile switching - no source needed!
azprofile use work
az account show      # Shows work account info

azprofile use personal  
az account show      # Shows personal account info
azd up              # Uses personal profile
```

### 3. Other commands

```bash
# List all profiles
azprofile list

# Show current profile status  
azprofile current

# Execute single command with specific profile
azprofile exec work -- az account show
azprofile exec personal -- azd deploy

# Remove profile
azprofile remove work
```

## Manual Usage (Without Setup)

If you haven't run the setup, you need to use `source`:

```bash
source azprofile use work
az account show  # Uses work profile
```

## Example Workflow

```bash
# 1. One-time setup
azprofile setup ~/.zshrc
source ~/.zshrc

# 2. Add profiles for different Azure accounts
azprofile add work
# Login with work account, select work subscription

azprofile add personal
# Login with personal account, select personal subscription

# 3. Switch between profiles easily
azprofile use work
az resource list      # Lists work resources

azprofile use personal
azd up               # Deploy to personal subscription

# 4. Check current status
azprofile list       # See all profiles
azprofile current    # Show active profile info
```

## Features

- ✅ **No dependency on Node.js at runtime** (pure shell script)
- ✅ **Seamless profile switching** with shell integration
- ✅ **Multiple Azure account management** with isolated config directories
- ✅ **Works with both `az` and `azd` commands**
- ✅ **Interactive profile creation** with auto-login prompts
- ✅ **Persistent and temporary profile switching**
- ✅ **Clean, colorful CLI interface**
- ✅ **Cross-shell compatibility** (bash, zsh)

## How it works

azprofile manages separate Azure CLI configuration directories for each profile using the `AZURE_CONFIG_DIR` environment variable. This allows you to have completely isolated Azure authentication contexts.

### Profile Storage

- Profiles are stored in `~/.azure-profiles/<profile-name>/`
- Each profile contains independent Azure CLI configuration
- Settings are saved in `~/.azure-profiles/config`

### Shell Integration

The `azprofile setup` command adds a wrapper function to your shell configuration that:
- Intercepts `azprofile use <profile>` commands
- Automatically sets `AZURE_CONFIG_DIR` environment variable
- Passes through all other commands to the original azprofile binary

## Requirements

- **Bash >= 3.0** or **Zsh**
- **Azure CLI** (`az`) installed
- **Azure Developer CLI** (`azd`) for azd commands (optional)

## Troubleshooting

### Profile switching not working

Make sure you've run the setup:
```bash
azprofile setup ~/.zshrc  # or ~/.bashrc
source ~/.zshrc           # restart shell
```

### Parse errors in shell

Try reinstalling:
```bash
npm uninstall -g azprofile-cli
npm install -g azprofile-cli
azprofile setup ~/.zshrc
```

### Environment variable not set

Check if setup was successful:
```bash
type azprofile              # Should show it's a function
azprofile use <profile>
echo $AZURE_CONFIG_DIR     # Should show profile path
```

## License

MIT