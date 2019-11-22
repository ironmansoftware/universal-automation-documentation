# Getting Started

## Installing Universal Automation

The installation of Universal Automation can be accomplished by merely installing Universal Automation from the PowerShell Gallery. Installing Universal Automation from the PowerShell Gallery can be accomplished by using ``Install-Module`` PowerShell Command. The Universal Automation module comes in 3 variants:

* UniversalAutomation - Includes the Core Universal Automation Platform and a Universal Dashboard for Universal Automation
* Automation Core - Includes just the core Universal Automation and no web-based management dashboard
* UniversalAutomation.Dashboard - Starts just Universal Dashboard for Universal Automation (for those who wish to separate their dashboard host from their Universal Automation host.

To install both Universal Automation and the Universal Automation dashboard, use the `UniversalAutomation` module.

```powershellInclude
Install-Module -Name UniversalAutomation
```

To install only Universal Automation, install `UniversalAutomation.Core`.


```powershell
Install-Module -Name UniversalAutomation.Core
```

To install the Universal Automation Dashboard, install the `UniversalAutomation.Dashboard` module.

```powershell
Install-Module -Name UniversalAutomation.Dashboard
```

## Starting Universal Automation

First, we'll Import the Universal Automation PowerShell Module and the Universal Automation Dashboard Module.

```powershell
Import-Module "UniversalAutomation.psd1"
Import-Module "UniversalAutomation.Dashboard.psd1"
```

Once imported, we need to take a number of steps to Start and connector to our Universal Automation Instance. 

1. Start the Universal Automation Server
2. Connect our PowerShell Terminal to the Universal Automation Server

```powershell
Start-UAServer -Port 10000
$AppToken = Initialize-UAServer -ComputerName "http://localhost:10000"
Connect-UAServer -ComputerName "http://localhost:10000" -AppToken $AppToken.Token
```

Once connected, we'll immediately be able to execute commands against our Universal Automation Instance.

Optionally, if the Dashboard component of Universal Automation is installed, we can initiate the Dashboard component with the following commands:

```powershell
$Dashboard = New-UADashboard
Start-UDDashboard -Dashboard $Dashboard -Port 10001
```

At this point we'll have our Universal Automation server started and ready to receive commands.

## Using Universal Automation

### Create a Script

```powershell
$Script = New-UAScript -Name "My First Script" -ScriptBlock {Start-Sleep -Seconds 5; Write-Host "Hello World"}
```

### Get Script Object

```powershell
$Script = Get-UAScript -Id 5
```

### Execute a Script

```powershell
Invoke-UAScript -Script $Script
```

### Schedule a Script

```powershell
New-UASchedule -Script $Script -Cron '*/12 * * * *'
```

### Creating a New Tag

```powershell
$Tag = New-UATag -Name "Workstation Team" -Color "#e91e63"
```

## Learn More

* [Concepts](concepts.md)