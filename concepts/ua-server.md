# UA Server

The UA server runs within a PowerShell.exe or Pwsh.exe instance. It starts outside of the current running PowerShell session and runs as a REST service that you can interact with via the UA PowerShell module or with the REST API directly. 

## Starting the UA Server

The UA server is started with `Start-UAServer`. You can specify settings such as a port, git remote and database connection string. 

```text
Start-UAServer -Port 10000
```

## Authentication

After the service has started, it will connect to the configured database. If the database contains AppTokens, the REST API for UA will switch to an authenticated mode and you will need to specify an API AppToken to invoke commands against the API. You can grant an API token using the `Grant-UAAppToken` cmdlet. AppTokens are JWT bearer tokens and should be treated as such. You can also pass them to the `Connect-UAServer` cmdlet to authenticate the current PowerShell session against your UA server. 

