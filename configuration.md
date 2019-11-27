# Configuration

## Database 

Universal Automation uses a LiteDB single-file database to store cached script, schedule and variable information. It also stores job status and output information. You can configure the database file path by setting the `ConnectionString` parameter of `Start-UAServer`.

By default, the database is stored in the `%AppData%` folder.

```text
Start-UAServer -ConnectionString "$Env:APPDATA\UniversalAutomation\database.db"
```

## Git 

To configure git a remote, you can set the `GitRemote` and `GitRemoteCredential` parameters of `Start-UAServer`.

```text
Start-UAServer -GitRemote 'https://github.com/adamdriscoll/myRemote.git'
```

You can also configure a git remote using the `Set-UASetting` cmdlet. If a git remote is already configured in the git repository that is cloned on disk, there is no need to configure a git remote manually. 

