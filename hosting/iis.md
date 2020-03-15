# IIS

Hosting in IIS requires the use of a web.config file and installing the Universal Automation module in a location that the Application Pool Identity can access. You will also need to ensure that you have the [ASP.NET Core Hosting Bundle ](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)installed on the IIS web server. 

You can install Universal Automation and the Universal Automation Dashboard in the All Users scope to allow it to be accessible by the Application Pool Identity.

```text
Install-Module UniversalAutomation -Scope AllUsers -AllowPrerelease
Install-Module UniversalAutomation.Dashboard -Scope AllUsers -AllowPrerelease
```

The first step is to create a web site within IIS. You will need to bind the web site to a port. If you are going to run the Universal Automation Dashboard on the same server as the UA server, you should select an alternate port. 

![](../.gitbook/assets/image%20%289%29.png)

The website should point to a local directory that will contain the web.config file and a PS1 file to start the UA server. 

![IIS Web Site Settings](../.gitbook/assets/image%20%2814%29.png)

Within the directory, create a web.config file that will start PowerShell.exe or Pwsh.exe with the -File parameter and point it to a PS1 file. You can optionally choose to enable STDOUT logging to see the output of the UA server. This is helpful when first configuring the server to aid in debugging issues. 

```text
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <!--
    Configure your application settings in appsettings.json. Learn more at http://go.microsoft.com/fwlink/?LinkId=786380
  -->
  <system.webServer>
    <security>
      <!-- <requestFiltering removeServerHeader ="true" /> -->
    </security>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="powershell.exe" arguments="-File .\universal-automation.ps1" stdoutLogEnabled="true" stdoutLogFile="C:\users\iis\AppData\Local\Logs\" />
    <httpProtocol>
      <customHeaders>
        <remove name="X-Powered-By" />
      </customHeaders>
    </httpProtocol>  
  </system.webServer>
</configuration>

```

Within the `universal-automation.ps1` file we will need to start to UA server using Start-UAServer. You will need to specify -InProcess so that the server starts in the same PowerShell process. If you don't specify the -InProcess switch, a new process will start up and IIS will not connect correctly. 

You can optionally configure where to store the database and Git repository. The IIS AppPool identity will need read and write access to these locations. 

Here is an example of the `universal-automation.ps1` file. 

```text
Import-Module UniversalAutomation

New-Item "C:\users\iis\AppData\Local\UniversalAutomation\" -ItemType Directory -ErrorAction SilentlyContinue
Start-UAServer -InProcess -ConnectionString "C:\users\iis\AppData\Local\UniversalAutomation\database.db"  -RepositoryPath C:\users\iis\AppData\Local\UniversalAutomation\Repository
```

Now that your web site is configured, you should be able to invoke commands against it. 

```text
PS C:\Users\adamr> Get-UAScript -ComputerName http://localhost:81


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

## Configuring the UA Dashboard

To configure the UA dashboard, you can follow the same steps above for creating a web.config file and PS1file. The website should be bound to a different port. The contents of the PS1 file will also be different. Below is an example of connecting your UA Dashboard to UA running in IIS. 

```text
Import-Module UniversalAutomation.Dashboard
Connect-UAServer -ComputerName http://localhost:81
$Dashboard = New-UADashboard -ComputerName http://localhost:81
Start-UDDashboard -Dashboard $Dashboard -Wait
```

