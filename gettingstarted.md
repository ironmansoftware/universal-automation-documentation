---
description: This pages provides getting start topics
---

# Getting Started

## Universal Automation 

{% hint style="info" %}
This section only applies to Windows. 
{% endhint %}

### Installation

1. Download the Universal Automation MSI
2. Run the Universal Automation MSI 
3. A web browser will open to the Universal Automation Dashboard 



## Universal Automation Desktop

### Installation

{% hint style="info" %}
This section only applies to Windows. 
{% endhint %}

1. Download the Universal Automation Desktop installer
2. Run the Universal Automation Desktop installer
3. UA Desktop will open after installation

## Configuration

### Adding and running your first script

Click the new script button to add a new script. 

![Click the new script button](.gitbook/assets/image%20%2812%29.png)

You can enter information about the new script in the modal that appears. Only the name is required. 

![](.gitbook/assets/image%20%2810%29.png)

The edit the contents of the script, you can click View and then Edit.

![](.gitbook/assets/image%20%283%29.png)



After editing your script, click Save. You can run your script by clicking the Run button. 

![](.gitbook/assets/image%20%281%29.png)

## Installing Universal Automation Manually

The installation of Universal Automation can be accomplished by merely installing Universal Automation from the PowerShell Gallery. Installing Universal Automation from the PowerShell Gallery can be accomplished by using `Install-Module` PowerShell Command. The Universal Automation module comes in 3 variants:

* UniversalAutomation - Includes the Universal Automation agent and cmdlets. 
* UniversalAutomation.Dashboard - Starts just Universal Dashboard for Universal Automation \(for those who wish to separate their dashboard host from their Universal Automation host.

To install the agent and cmdlets, install UniversalAutomation.

```text
Install-Module -Name UniversalAutomation
```

To install the Universal Automation Dashboard, install the `UniversalAutomation.Dashboard` module.

```text
Install-Module -Name UniversalAutomation.Dashboard
```

## Starting Universal Automation Manually

First, we'll Import the Universal Automation PowerShell Module and the Universal Automation Dashboard Module.

```text
Import-Module "UniversalAutomation.psd1"
Import-Module "UniversalAutomation.Dashboard.psd1"
```

Once imported, we need to take a number of steps to Start and connector to our Universal Automation Instance.

1. Start the Universal Automation Server
2. Connect our PowerShell Terminal to the Universal Automation Server

```text
$AppToken = Start-UAServer -Port 10000
Connect-UAServer -ComputerName "http://localhost:10000" -AppToken $AppToken
```

Once connected, we'll immediately be able to execute commands against our Universal Automation Instance.

Optionally, if the Dashboard component of Universal Automation is installed, we can initiate the Dashboard component with the following commands:

```text
$Dashboard = New-UADashboard
Start-UDDashboard -Dashboard $Dashboard -Port 10001
```

At this point we'll have our Universal Automation server started and ready to receive commands.

## Using Universal Automation from the Command Line

### Create a Script

```text
$Script = New-UAScript -Name "My First Script" -ScriptBlock {Start-Sleep -Seconds 5; Write-Host "Hello World"}
```

### Create a Script from an external .PS1 file

```text
$Script = New-UAScript -Name "Imported Script" -ScriptBlock (Get-Command "C:\Test\MyScript.ps1" | Select -ExpandProperty ScriptBlock)
```

### Get Script Object

```text
$Script = Get-UAScript -Id 5
```

### Execute a Script

```text
Invoke-UAScript -Script $Script
```

### Schedule a Script

```text
New-UASchedule -Script $Script -Cron '*/12 * * * *'
```

### Creating a New Tag

```text
$Tag = New-UATag -Name "Workstation Team" -Color "#e91e63"
```

### Tagging a Script with a Tag

```text
$Tag = Get-UATag -Name "Workstation Team"
$Script = Get-UAScript -Id 5
Add-UAScriptTag -Script $Script -Tag $Tag
```

## Learn More

* [Concepts](concepts/)

