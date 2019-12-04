# Sample Script

This sample script can be used to get up and running with an example of UA with several scripts, tags, a folder and UD authentication. 

```text
Import-Module "UniversalAutomation" -Force
Import-Module "UniversalAutomation.Dashboard" -Force

$ComputerName = "http://localhost:10000"

Start-UAServer -Port 10000 

$Cache:ComputerName = $ComputerName 
$AppToken = Grant-UAAppToken -Identity System -Role Administrator -ComputerName $ComputerName
Connect-UAServer -ComputerName $ComputerName -AppToken $AppToken.Token
$Dashboard = New-UADashboard

$AuthMethod = New-UDAuthenticationMethod -Endpoint {
    param([pscredential]$Credential)
    New-UDAuthenticationResult -Success -UserName $Credential.UserName
}

$AuthPolicy = New-UDAuthorizationPolicy -Name "Policy" -Endpoint {
    if (-not $Session:AppToken)
    {
        $Identity = Get-UAIdentity -Name $User.Identity.Name 
        if ($null -ne $Identity)
        {
            $AppToken = Get-UAAppToken -Identity $Identity
            if (-not $AppToken.Revoked)
            {
                $Session:AppToken = $AppToken.Token
            }
        }

        if (-not $Session:AppToken)
        {
            $Session:AppToken = (Grant-UAAppToken -Role Administrator -Identity $User.Identity.Name).Token
        }
    }
    
    $true
}

$LoginPage = New-UDLoginPage -AuthenticationMethod $AuthMethod -AuthorizationPolicy $AuthPolicy
$Dashboard.LoginPage = $LoginPage

Start-UDDashboard -Dashboard $Dashboard -Port 10001 -AdminMode -AllowHttpForLogin

Start-Process http://localhost:10001

$Folder = New-UAFolder -Name 'Test Scripts'

$Settings = Get-UASetting
$Settings.LogLevel = "Debug"
Set-UASetting -Setting $Settings

$Script = New-UAScript -Name "Step 1" -ManualTime 20000 -ScriptBlock { Write-Host 'Step 1'; Get-UAScript | Where Name -eq 'Step 2.ps1' | Invoke-UAScript | Wait-UAJob } -Status "Published" -Folder $Folder
New-UAScript -Name "Step 2" -ManualTime 20000 -ScriptBlock { 
    Write-Host 'Step 2';    
    Get-UAScript | Where Name -eq 'Step 3.ps1' | Invoke-UAScript | Wait-UAJob 
} -Status "Published" -Folder $Folder
New-UAScript -Name "Step 3" -ManualTime 20000 -ScriptBlock { Write-Host 'Step 3'; } -Status "Published" -Folder $Folder
Invoke-UAScript -Script $Script

$Script = New-UAScript -Name "Execute Backups" -ManualTime 500 -ScriptBlock { Start-Sleep -Seconds 10; Write-Host "Step 3" } -Status "Pending_Review" -Folder $Folder
Invoke-UAScript -Script $Script

$Script = New-UAScript -Name "Runs forever" -ManualTime 600 -ScriptBlock { while(1) { Start-Sleep -Seconds 1 } } -Status "Published" -Folder $Folder

$Script = New-UAScript -Name "Shutdown Virtual Labs" -ManualTime 600 -ScriptBlock { Write-Host "Step 3" } -Status "Published"
$Script = New-UAScript -Name "New User Onboarding Request" -ManualTime 28000 -ScriptBlock ([scriptblock]::Create((Get-Content "$PSScriptRoot\UniversalAutomation.Dashboard\examplescripts\onboarding.ps1"))) -Status "Draft"

$Tag = New-UATag -Name "MyTag" -Color "#ffffff"
$Tag = New-UATag -Name "Testing" -Color "#f44336"
$Tag = New-UATag -Name "Workstation Team" -Color "#e91e63"
$Tag = New-UATag -Name "Infrastructure Team" -Color "#9c27b0"
$Tag = New-UATag -Name "Development" -Color "#673ab7"
$Tag = New-UATag -Name "SQL" -Color "#3f51b5"
$Tag = New-UATag -Name "Experimental" -Color "#2196f3"
$Tag = New-UATag -Name "Retired" -Color "#03a9f4"
$Tag = New-UATag -Name "Not Working" -Color "#00bcd4"
$Tag = New-UATag -Name "DevOps" -Color "#009688"
$Tag = New-UATag -Name "Linux" -Color "#4caf50"
$Tag = New-UATag -Name "Windows" -Color "#8bc34a"
$Tag = New-UATag -Name "Core" -Color "#cddc39"
$Tag = New-UATag -Name "ETL" -Color "#ffeb3b"
$Tag = New-UATag -Name "Americas" -Color "#ffc107"
$Tag = New-UATag -Name "EMEA" -Color "#ff9800"
$Tag = New-UATag -Name "APAC" -Color "#ff5722"

$Script = New-UAScript -Name 'Parameters' -ManualTime 1000 -ScriptBlock { param($Test1, $Test2, $Test3) $Test1; $Test2; $Test3 } -Status "Published" 

$Script = New-UAScript -Name 'ErrorTest Script' -ManualTime 1000 -ScriptBlock { Write-Error "Shit! $username"; Start-Sleep 4 } -Status "Disabled" -ErrorAction Stop
Invoke-UAScript -Script $Script
$Script = New-UAScript -Name 'WarningTest Script' -ManualTime 1000 -ScriptBlock { Write-Warning "Warning"; Start-Sleep 4 } -Status "Published"
$Script = New-UAScript -Name 'Debug Test Script' -ManualTime 1000 -ScriptBlock { Write-Error "Debug" } -Tag $Tag -Status "Published"
$Script = New-UAScript -Name 'Progress Test Script' -ManualTime 1000 -ScriptBlock { 1..100 | % { Write-Progress -Activity "Progress $_" -PercentComplete $_; Start-Sleep -Milliseconds 1000 } } -Tag $Tag

New-UASchedule -Script $Script -Cron '*/2 * * * *'

$Script = New-UAScript -Name 'Output to Pipeline Test Script' -ManualTime 1000 -ScriptBlock { 
    "String"
    123
    [PSCustomObject]@{
        Name = "Value"
    }
    Get-Date
}  -Status "Pending_Review"

$Script = New-UAScript -Name "Read Host Test Script" -ManualTime 1000 -ScriptBlock { Write-Host (Read-Host -Prompt "Enter some text") } -Status "Published"

New-UAVariable -Name username -Value "adam"
New-UAVariable -Name password -Value "test" -Secret
New-UAVariable -Name myServer1 -Value "test.domain.com"

Wait-Debugger

Stop-UAServer
```



