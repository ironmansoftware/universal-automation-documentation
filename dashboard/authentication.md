# Authentication

By default, the UA dashboard is not configured for authentication. You can use any Universal Dashboard authentication method with your UA dashboard. You'll need to set the `$Session:AppToken`variable in an authorization policy as the user logs in. This will ensure that the user that is accessing the dashboard will have access to the UA API. 

Here is an example of using forms authentication with UA. 

```text
$AuthMethod = New-UDAuthenticationMethod -Endpoint {
    param([pscredential]$Credential)

    $Identity = Get-UAIdentity -Name $Credential.UserName 
    if ($null -ne $Identity)
    {
        $AppToken = Get-UAAppToken -Identity $Identity
        if (-not $AppToken.Revoked)
        {
            $AppToken = $AppToken.Token
        }
    }

    if (-not $AppToken)
    {
        $AppToken = (Grant-UAAppToken -Role Administrator -Identity $Credential.UserName).Token
    }

    New-UDAuthenticationResult -Success -UserName $Credential.UserName -Token $AppToken
}

$AuthPolicy = New-UDAuthorizationPolicy -Name "Policy" -Endpoint {
    param($ClaimsPrincipal)

    $UserName = $ClaimsPrincipal.Identity.Name 
    $Session:UserRole = ""

    $Identity = Get-UAIdentity -Name $UserName 
    if ($Identity -eq $null)
    {
        if ($UserName -eq 'OperatorFred')
        {
            $Role = Get-UARole -Name "Operator"
            $Identity = New-UAIdentity -Name $UserName -Role $Role
            $Session:UserRole = "Operator"
        }            
        elseif ($UserName -eq 'ReaderJane')
        {
            $Role = Get-UARole -Name "Reader"
            $Identity = New-UAIdentity -Name $UserName -Role $Role
            $Session:UserRole = "Reader"
        }
        else 
        {
            $Role = Get-UARole -Name "Administrator"
            $Identity = New-UAIdentity -Name $UserName -Role $Role
            $Session:UserRole = "Administrator"
        }

        $AppToken = (Grant-UAAppToken -Identity $Identity).Token
    }
    else 
    {
        $AppToken = (Get-UAAppToken -Identity $Identity).Token
        if ($null -eq $AppToken)
        {
            $AppToken = (Grant-UAAppToken -Identity $Identity).Token
        }
    }

    $Session:AppToken = $AppToken 

    $true
}

$LoginPage = New-UDLoginPage -AuthenticationMethod $AuthMethod -AuthorizationPolicy $AuthPolicy
Start-UADashboard -LoginPage $LoginPage -Port 10000
```

