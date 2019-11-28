# Variables

Variables are available in all of your scripts. You can use `Get-Variable` and specify your variable by `$` in any of your scripts. You can define variables in UA with the `UAVariable` cmdlets. 

## Creating a variable

To create a new variable, use the `New-UAVariable` cmdlet to specify a name and value. When you invoke this cmdlet, it will update the `.ua/variables.ps1`file. 

```text
New-UAVariable -Name 'TestVar' -Value 'TestVar1'
```



