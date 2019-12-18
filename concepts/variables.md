# Variables

Variables are available in all of your scripts. You can use `Get-Variable` and specify your variable by `$` in any of your scripts. You can define variables in UA with the `UAVariable` cmdlets. 

## Creating a variable

To create a new variable, use the `New-UAVariable` cmdlet to specify a name and value. When you invoke this cmdlet, it will update the `.ua/variables.ps1`file. 

```text
New-UAVariable -Name 'TestVar' -Value 'TestVar1'
```

## Secrets

You can create secret variables that are not stored in Git by specifying a SecretManager when creating a new variable. The secret manager will be used to retrieve the value of the variable at runtime. You can configure what ever secret manager you like with the UASecretManager cmdlets. 

```text
$SecretManager Get-UASecretManager -Name 'DAPI'
New-UAVariable -Name 'ApiKey' -SecretManager $SecretManager
```

The value of secrets are never stored in the UA database or git repo. You need to ensure that you do not log the secrets in your scripts in order to prevent exposing them through the logs. 

