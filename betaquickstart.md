# Beta QuickStart

## Installing Universal Automation

To obtain the latest beta module of Universal Automation`UniversalAutomation` module, first download the latest build from the Special Beta Build container link you received in your beta participant email.

1. Download the Zip File for the latest module version
2. Extract the Zip, this will output two module folders.
3. Import the Universal Automation PowerShell Module and the Universal Automation Dashboard Module.


{% hint style="warning" %}
NOTE: You should ensure to UNBLOCK the extracted files if using Windows. After downloading a zip file from the web Windows may prevent the script module from loading properly by "blocking" the extracted module files. Run the following PowerShell command in the extracted module folders to ensure no files are being blocked by Windows.
``dir -recurse | Unblock-File``
{% endhint %}


```text
Import-Module "PATHTOMODULEFOLDER\UniversalAutomation.psd1" -Force
Import-Module "PATHTOMODULEFOLDER\UniversalAutomation.Dashboard.psd1" -Force
```

## Starting Universal Automation.

Once imported, we need to take a number of steps to Start and connect to our Universal Automation Instance specifically these are:

1. Start the Universal Automation Server
2. Connect our PowerShell Terminal to the Universal Automation Server

```text
Start-UAServer -Port 10000
$AppToken = Grant-UAAppToken -Identity System -Role Administrator -ComputerName "http://localhost:10000"
Connect-UAServer -ComputerName "http://localhost:10000" -AppToken $AppToken.Token
```

Once connected, we'll immediately be able to execute commands against our Universal Automation Instance.

Optionally, if the Dashboard component of Universal Automation is installed, we can initiate the Dashboard component with the following commands:

```text
$Dashboard = New-UADashboard
Start-UDDashboard -Dashboard $Dashboard -Port 10001
```

At this point we'll have our Universal Automation server started and ready to receive commands, we'll also have our dashboard availbile and ready to use on: [localhost:10001](http://localhost:10001)

Default UserName and Password for the UA Dashboard is: "Admin/Admin"

## Learn More

* [Getting Started](gettingstarted.md)