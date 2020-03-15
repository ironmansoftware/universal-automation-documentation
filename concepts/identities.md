# Identities

Identities are the users or services connecting to your UA server. Identities are generated when you either create an AppToken via `Grant-UAAppToken` or when syncing with git. An Identity will be created based on the committer for a particular git commit.

## Command Line

### Getting Identities

To retrieve Identities, use the `Get-UAIdentity` .

### Creating New Identities

New Identities can be created with the `New-UAIdentity` command.

When creating a new Identity, the Identity can also be assigned to a Role.

```text
$Role = Get-UARole -Name "admin"
New-UAIdentity -Name "TestUser" -Role $Role
```

