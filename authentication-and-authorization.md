# Authentication and Authorization

Universal Automation uses token-based authentication and authorization to manage access to the API. You can configure additional authentication and authorization via external systems like Universal Dashboard. This document will highlight how to create identities, configure roles, grant tokens and how to setup a simple Universal Dashboard claims policy to grant tokens on behalf of users logging into to UA Dashboard. 

## Identities

Identities are any application or user logging into the UA system. They can be created manually by running the `New-UAIdentity` cmdlet or created automatically by the git synchronization process. Identities will have a Name, Source \(git or API\), and Role associated with them. 

```text
New-UAIdentity -Name 'Adam'
```

## Roles 

There are three roles defined within Universal Automation. These roles dictate what the user has access to and what they can do within the system. 

### Administrator

The Administrator user can do everything. They can manage settings of UA, create and edit any entity within UA and view all the entities within UA. 

### Operator

Operator users have access to manage and execute scripts, create other entities within UA but cannot manage UA itself. 

### Reader

Readers have read-only access to UA. They cannot make changes to any entity within the system.

### Assigning a Role to an Identity

```text
$Role = Get-UARole -Name 'Operator'
New-UAIdentity -Name 'Adam' -Role $Role
```

## App Tokens 

App tokens are granted to Identities to allow them to access the API. UA uses JWT Bearer tokens to valid user access and provide them with the ability to access resources based on their role. 

App Tokens are stored within the UA database and with an expiration time and with the ability to manually revoke them at any time. 

Administrators can grant and revoke App Tokens to any user. Operator and Reader users can only grant tokens for their own Identity. 

### Granting an App Token to Another Identity

This operation will require an Administrator role to execute. 

```text
$Identity = Get-UAIdentity -Name 'Adam'
Grant-UAAppToken -Identity $Identity
```

### Granting an App Token to your own Identity

This operation will require an existing App Token. You will be granting a new App Token. 

```text
Grant-UAAppToken
```

### Revoking an App Token for another Identity

This operation will require an Administrator role to execute. 

```text
$Identity = Get-UAIdentity -Name 'Adam'
$AppToken = Get-UAAppToken -Identity $Identity | Select-Object -First 1
Revoke-UAAppToken -AppToken $AppToken
```

## Granting a System App Token

When you configure UA, you can set various properties of the JSON Web Token. These include Audience, Issuer and Signing Key. You specify these values when starting the UA server via `Start-UAServer`.

If you want to grant access to the API, you can also generate app tokens locally. These app tokens will not be generate by the server but will require that you know the audience, issuer and signing key for you UA instance. This parameter set is useful if you no longer have any valid app tokens in your UA database or you lose track of you app tokens. 

```text
Grant-UAAppToken -IdentityName 'System' -Issuer 'Ironman Software' -Audience 'Universal Automation' -SigningKey 'This is my signing key. It needs to be a long'
```



