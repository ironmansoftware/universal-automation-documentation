# Tags

Tags provide a way to organize scripts. You can assign one or more tags to a script and the script for scripts based on particular tags. They can be created through the UA PowerShell module, the REST API or by syncing with git.

## Getting Tags

Tags can be retrieved with the `Get-UATag` command.

## Creating New Tags

```text
New-UATag -Name "TestTag" -Color "#2196f3"
```

## Assigning Tags to Scripts

```text
$Script = Get-UAScript -Id 4
$Tag = Get-UAScript
Add-UAScriptTag -Script $Script -Tag $TagToAdd
```

## Getting Tags currently applied to a script

```text
$Script = Get-UAScript -Id 4
Get-UAScriptTag -Script $Script
```

