Notes on PowerShell on MS Learn and others.

## Installation of the latest version
To get the current version of PowerShell installed from a PowerShell console:  
`$PSVersionTable`  

Use WinGet to install PowerShell on Windows.  
`winget search Microsoft.PowerShell`  
If your version is less than the current version.  
`winget install --id Microsoft.Powershell --source winget`  

## Execution Policy
The cmdlets *Get-ExecutionPolicy* and *Set-ExecutionPolicy* allow you to change the default policies.  
The default values are fine for these notes, but if you run into problems look up execution policies to see if you can fix things.

## Variables
`$VariableName = 99`  

They are not just for scripting. Variables can be defined in the console and used later.  

### Interpolation
Using double quotation marks allow you to use interpolation (displays the variable value)  
Use the back tick "\`" to display the variable name instead of its value.  
``Write-Host "`$VariableName = $VariableName``: Displays "$VariableName - 99"  

Single quotes will only print the variable name. Interpolation only works using double quotes.  
`Write-Host '$VariableName'`: Displays "$VariableName"  

$() allows you to interpolate calculations.  
``Write-Host "\`$VariableName + 1 = $($VariableName = 1)"``

### Scopes
**Global**: Anything that is present when you start a new PowerShell session is in global scope.  
**Script Scope**: When you run a script, a *script scope* is created. All variables and functions defined in the script are out of scope when the script ends.  
**Local Scope**: The current scope, whether it is *global* or *script* scope.  
**Child Scope**: The parent scope shares it's variables with nested *child* scopes, unless they are declared `private`.  

You can reference the global scope from a script using the `global` keyword: `$global:globalVariable = 'Hi!'`.  
By default, items can only be changed in the scope they are created in.  

Example:  
1. Declare a variable in the console/terminal: `$globalHey = 'Hey!`
2. Create a script with the name "scope.ps1" and the following contents:
```
Write-Host $globalHey
$global:globalHey = 'Hi!'
Write-Host $globalHey
```
4. `.\scope.ps1`: Runs the script to see the global variable output, global variable changed in the script scope, and then output again with the new value.

### Profiles
A profile script runs when PowerShell starts.  
By default, there are no profiles created when you install PowerShell.  
There is a `$Profile` variable that points to the path where each profile should be placed.  
`$Profile | Select-Object *` to see the profile types and full paths associated with them.  

**Profile Types** - applied at various levels  
| Description                | Path
|----------------------------|---------------------|
| All users, all hosts       | $PSHOME\Profile.ps1 |
| All users, current host    | $PSHOME\Microsoft.PowerShell_profile.ps1 |
| Current user, all hosts	   | $Home[My ]Documents\PowerShell\Profile.ps1 |
| Current user, current host | $Home[My ]Documents\PowerShell\Microsoft.PowerShell_profile.ps1 |

$PSHOME is the installation directory for PowerShell  
$HOME is the current user's home directory  

Note: There may be additional profiles. If you use Visual Studio Code, for example, you will also have a profiles specific to it Running `$Profile | Select-Object *` in VS Code produces.  

```
AllUsersAllHosts       : C:\Program Files\PowerShell\7\profile.ps1
AllUsersCurrentHost    : C:\Program Files\PowerShell\7\Microsoft.VSCode_profile.ps1
CurrentUserAllHosts    : C:\Users\Username\Documents\PowerShell\profile.ps1
CurrentUserCurrentHost : C:\Users\Username\Documents\PowerShell\Microsoft.VSCode_profile.ps1
```

To create a profile:
1. Decide the level the profile should encompass using: `$Profile | Select-Object *`
    * The output will help you create the path.  
3. Select a profile type and create a text file at its location by using a command such as `New-Item -Path $Profile.CurrentUserCurrentHost` for a personal profile.
4. Add your customizations to the text file and save it.

You will need to restart any existing sessions for the change to take place.  

Example:  
```
New-Item -ItemType "file" -Value 'Write-Host "Hello human slave, welcome back" -foregroundcolor Green ' -Path $Profile.CurrentUserCurrentHost -Force
```

`-Force` switch overwrites any existing content, so use with care.  

Note: running the above from Visual Studio Code will create a profile specific to VS Code only in Microsoft.VSCode_profile.ps1.  

## Azure Cloud Shell
`pwsh` launches a PowerShell terminal  

## Creating a File from PowerShell Terminal
To create a script named "file-name.ps1" in the current directory denoted by the dot operator ".":  
`New-Item -Path . -Name "file-name.ps1" -ItemType "file"`  
