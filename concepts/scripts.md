# Scripts

Scripts are individual PS1 files that you can execute within your environment. You can either invoke them via the PowerShell module using `Invoke-UAScript`, execute them via the UA REST API or schedule them using `New-UASchedule`. 

## Adding scripts to UA

To add a script to UA, you need to either clone a remote git repo with PS1 files in it, point the UA server at a folder with PS1 files in it, or add the script via `New-UAScript`. 

## Invoking a script with PowerShell

You can invoke a script using the `Invoke-UAScript` cmdlet. Pass in either the Id of the script or a Script object returned by `Get-UAScript`. 

#### Passing Parameters to Scripts

`Invoke-UAScript`supports passing parameters to scripts via dynamic parameters. You can simply add new parameters to `Invoke-UAScript` to have it pass the parameter name and value to the target script. 

```text
$Script = Get-UAScript -Id 3
Invoke-UAScript -Script $Script -MyScriptParameter "Hello, World!"
```

## Removing Scripts from UA

You can remove a script from UA by either using the PowerShell module, REST API or by deleting the file and committing the change to git. 

