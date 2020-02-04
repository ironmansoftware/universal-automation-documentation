# Windows Service

You can host Universal Automation as a Windows Service using NSSM. In this example, we assume you have installed the Universal Automation module to a location that the user running the service has access to. 

```text
Install-Module UniversalAutomation -Scope AllUsers -Force -AllowPrerelease
```

You can create a`universal-automation.ps1` file to start the UA server. The connection string and repository path are optional. 

```text
Import-Module UniversalAutomation

New-Item "C:\users\iis\AppData\Local\UniversalAutomation\" -ItemType Directory -ErrorAction SilentlyContinue
Start-UAServer -InProcess -ConnectionString "C:\users\iis\AppData\Local\UniversalAutomation\database.db"  -RepositoryPath C:\users\iis\AppData\Local\UniversalAutomation\Repository -Port 20000
```

The following command line can be used to install and start the service using NSSM. 

```text
 .\nssm.exe install "Universal Automation" powershell.exe -File "C:\users\iis\AppData\Local\universal-automation.ps1"
 Start-Service -Name "Universal Automation"
```

You should now be able to invoke commands against your UA service. 

```text
PS C:\WINDOWS\system32> Get-UAScript -ComputerName http://localhost:20000


Id                        : 1
Name                      : Test.ps1
Description               :
CreatedTime               : 2/4/2020 2:59:06 PM
ManualTime                : 0
CommitId                  : 9591f9ebd2b1efed18103c5a8b4dbbb63aabbec0
Content                   : Get-Process
ScriptParameters          :
Identity                  : UniversalAutomation.Identity
Tags                      : {}
Schedules                 :
Status                    : Draft
Folder                    : UniversalAutomation.Folder
FullPath                  : Test.ps1
RequiredPowerShellVersion :
ErrorAction               : Stop
CommitNotes               : Setting script content: Test.ps1
DisableManualInvocation   : False


```

