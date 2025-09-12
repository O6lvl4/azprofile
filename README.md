# azprofile

Azure CLI Profile Manager - Easy switching between multiple Azure accounts

## Installation

```bash
npm install -g azprofile
```

## Usage

### Add profiles
```bash
azprofile add work
azprofile add personal
```

### List profiles
```bash
azprofile list
```

### Use profile in current shell (recommended)
```bash
eval "$(azprofile use work)"
az account show  # Uses work profile

eval "$(azprofile use personal)"  
azd up  # Uses personal profile
```

### Execute single command with profile
```bash
azprofile exec work -- az account show
azprofile exec personal -- azd up
```

### Switch profile persistently
```bash
azprofile switch work
azprofile current  # Shows current profile
```

### Remove profile
```bash
azprofile remove work
```

## Features

- ✅ Multiple Azure account management
- ✅ Easy profile switching
- ✅ Works with both `az` and `azd` commands
- ✅ Persistent and temporary profile switching
- ✅ Interactive profile creation with auto-login
- ✅ Clean, colorful CLI interface

## How it works

azprofile manages separate Azure CLI configuration directories for each profile using the `AZURE_CONFIG_DIR` environment variable. This allows you to have completely isolated Azure authentication contexts.

## Requirements

- Node.js >= 16.0.0
- Azure CLI (`az`) installed
- Azure Developer CLI (`azd`) for azd commands

## License

MIT