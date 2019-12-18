# Secret Managers

Secret Managers allow you to get and optionally set secrets via Universal Automation. This way, Universal Automation doesn't manage secrets itself but rather stores and retrieves secrets from a system built to do so. You can configure a new secret manager with `New-UASecretManager`.

## Creating a new secret manager 

You can create a secret manager using `New-UASecretManager`. You need to specify at least the Get ScriptBlock. This script block will receive a variable name and will need to return a value that will then be passed into scripts that request the secret. 

```text
New-UASecretManager -Name 'DAPI' -Get { param($Name) Get-ProtectedVariable -Name $Name }
```

You can then create variables in UA that request the value from the secret manager. 

```text
New-UAVariable -Name 'ApiKey' -SecretManager $DapiSecretManager
```



