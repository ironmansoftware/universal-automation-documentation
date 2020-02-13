# Configuration

## Configuration as Script

Universal Automation uses PS1 scripts to configure itself. You can either edit settings for a script, schedule or variable via the REST API or PowerShell module or via the PS1 scripts that are stored in the `.ua` folder. Whenever you make a change through the REST API or PowerShell module, it will automatically update a PS1 file with the same name in the `.ua` folder with the changes settings. You can also add these files yourself or edit them via git. 

![Script Info File](../.gitbook/assets/image.png)

## Database 

Universal Automation uses a LiteDB single-file database to store cached script, schedule and variable information. It also stores job status and output information. You can configure the database file path by setting the `ConnectionString` parameter of `Start-UAServer`.

By default, the database is stored in the `%LocalAppData%` folder.

```text
Start-UAServer -ConnectionString "$Env:LOCALAPPDATA\UniversalAutomation\database.db"
```

## Git 

To configure git a remote, you can set the `GitRemote` and `GitRemoteCredential` parameters of `Start-UAServer`.

```text
Start-UAServer -GitRemote 'https://github.com/adamdriscoll/myRemote.git'
```

You can also configure a git remote using the `Set-UASetting` cmdlet. If a git remote is already configured in the git repository that is cloned on disk, there is no need to configure a git remote manually. 

