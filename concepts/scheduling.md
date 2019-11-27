# Scheduling

## Scheduling a Script

Scripts can be scheduled using the `New-UDSchedule` cmdlet. You can specify a CRON expression along with a time zone. 

You can use a tool like [crontab guru](https://crontab.guru/#4_*_*_*) to generate cron expressions.

This example runs a script every two minutes. Immediately after running `New-UASchedule`, the script configuration file will be updated, committed to git and the script will be scheduled in the job scheduler. 

```text
$Script = Get-UAScript -Id 3
New-UASchedule -Cron '*/2 * * * *' 
```



