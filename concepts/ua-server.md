# UA Server

The UA server runs within a PowerShell.exe or Pwsh.exe instance. It starts outside of the current running PowerShell session and runs as a REST service that you can interact with via the UA PowerShell module or with the REST API directly. 

## Starting the UA Server

After installing the server via the MSI, you will have two Windows Services running. One will be UniversalAutomation while the other is UniversalAutomationDashboard. They will automatically start after installation and after restarting the machine. 

## Configuring the UA Server

When the UA server is installed using the MSI, the folder `%ProgramData\UniversalAutomation` will be created. This folder will contain the following. 

* PS1 file for configuring the UA Server
* PS1 file for configuring the UA Dashboard
* Folder for the git repository 
* The LiteDB database

You can configure the settings for the UA Server and Dashboard within these PS1 files. 

## Starting the UA Server from the Command Line

The UA server is started with `Start-UAServer`. You can specify settings such as a port, git remote and database connection string. 

```text
Start-UAServer -Port 10000
```

## Authentication

After the service has started, it will connect to the configured database. If the database contains AppTokens, the REST API for UA will switch to an authenticated mode and you will need to specify an API AppToken to invoke commands against the API. You can grant an API token using the `Grant-UAAppToken` cmdlet. AppTokens are JWT bearer tokens and should be treated as such. You can also pass them to the `Connect-UAServer` cmdlet to authenticate the current PowerShell session against your UA server. 

