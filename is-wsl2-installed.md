Question : how check if windows has wsl2 with ubuntu installed?

To check if Windows has WSL2 with Ubuntu installed, follow these steps:

### Step 1: Check if WSL2 is installed
1. **Open PowerShell** (as Administrator).
2. **Run this command**:
   ```powershell
   wsl -l -v
   ```
   This will list all installed WSL distributions and show which version (WSL1 or WSL2) they are using. Look for the Ubuntu distribution and check if the version column shows "2" (for WSL2).

### Step 2: Check if Ubuntu is installed
The command `wsl -l -v` from above should list "Ubuntu" (or any other specific Ubuntu version like "Ubuntu-20.04" or "Ubuntu-22.04") as one of the installed distributions.

If Ubuntu does not appear in the list, you likely don't have it installed. You can install it from the Microsoft Store by searching for "Ubuntu" and installing the version you want.

### Example Output:
```powershell
  NAME                   STATE           VERSION
* Ubuntu-20.04           Running         2
  Ubuntu-22.04           Stopped         2
```

In this example, both Ubuntu-20.04 and Ubuntu-22.04 are installed and using WSL2.

Let me know if you'd like more details on any of these steps!
