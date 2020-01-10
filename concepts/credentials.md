# Credentials

By default, jobs will be started as the same user account that is hosting the UA server. This service account may not be what you want all your jobs running under. Instead, you can take advantage of credentials and secret managers to configure jobs to run as another user. 

Credentials are used to start jobs as a different user. This allows you to execute scripts under a context other than the user logging in or executing the job. You can assign credentials to schedules as well as manually invoked jobs. 

## Creating a credential

You create a credential with `New-UACredential`. You will need to have a secret manager configured in order to create credentials. Passwords for credentials must be passed via a secret variable that is retrieved from a secret manager. You cannot pass passwords on the command line. 

The value of the password variable is never returned from the API and is only used when starting the execution process. 

```text
$Variable = Get-UAVariable -Name 'Password'
New-UACredential -UserName 'Adam' -Password $Password
```

## Starting a Job as another user 

You can use `Invoke-UAScript` and specify the credentials for an account that you will wish to start the process as. 

This example starts a process as `Adam`.

```text
$Variable = Get-UAVariable -Name 'Password'
$Credential = New-UACredential -UserName 'Adam' -Password $Password
$Script = Get-UAScript -Name 'Script1.ps1'
Invoke-UAScript -Script $Script -Credential $Credential
```

## Scheduling a job to run as a user 

Just as with `Invoke-UAScript`, you can just pass a credential object to `New-UASchedule` to schedule a script with an alternate credential. 

```text
$Variable = Get-UAVariable -Name 'Password'
$Credential = New-UACredential -UserName 'Adam' -Password $Password
$Script = Get-UAScript -Name 'Script1.ps1'
New-UASchedule -SCript $Script -Credential $Credential
```

