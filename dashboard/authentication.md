# Authentication

By default, the UA dashboard is not configured for authentication. You can use any Universal Dashboard authentication method with your UA dashboard. You'll need to set the `$Session:AppToken`variable in an authorization policy as the user logs in. This will ensure that the user that is accessing the dashboard will have access to the UA API. 

Here is an example of using forms authentication with UA. 

```text
$Dashboard = New-UADashboard -ComputerName "http://localhost:10001"

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
    if (-not $Session:AppToken)
    {
        $Identity = Get-UAIdentity -Name $User.Identity.Name 
        if ($null -ne $Identity)
        {
            $AppToken = Get-UAAppToken -Identity $Identity
            if (-not $AppToken.Revoked)
            {
                $Session:AppToken = $AppToken.Token
            }
        }

        if (-not $Session:AppToken)
        {
            $Session:AppToken = (Grant-UAAppToken -Role Administrator -Identity $User.Identity.Name).Token
        }
    }
    
    $true
}

$LoginPage = New-UDLoginPage -AuthenticationMethod $AuthMethod -AuthorizationPolicy $AuthPolicy
$Dashboard.LoginPage = $LoginPage
```

