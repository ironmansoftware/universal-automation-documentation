# Variables

Variables are available in all of your scripts. You can use `Get-Variable` and specify your variable by `$` in any of your scripts. You can define variables in UA with the `UAVariable` cmdlets.

All scripts have access to variables at runtime, so far example if you have a UAVariable named "myAPIuri" you can retrieve this value simple be using `$myAPIuri` in your script.

## Creating a variable

To create a new variable, use the `New-UAVariable` cmdlet to specify a name and value. When you invoke this cmdlet, it will update the `.ua/variables.ps1`file.

```text
New-UAVariable -Name 'TestVar' -Value 'TestVar1'
```

## Secrets

Secret variables are stored within a Secret Management vault. On Windows, the BuiltInLocalVault is the Credential Manager. You can use the Secret Management module to create secrets and then import them into UA. 

```text
Set-Secret -Name 'Secret' -Value 'MySecret'
```

The value of secrets are never stored in the UA database or git repo. You need to ensure that you do not log the secrets in your scripts in order to prevent exposing them through the logs.

