# Scheduling

## Scheduling a Script

Scripts can be scheduled using the `New-UDSchedule` cmdlet. You can specify a CRON expression along with a time zone.

You can use a tool like [crontab guru](https://crontab.guru/#4_*_*_*) to generate cron expressions.

This example runs a script every two minutes. Immediately after running `New-UASchedule`, the script configuration file will be updated, committed to git and the script will be scheduled in the job scheduler.

```text
$Script = Get-UAScript -Id 3
New-UASchedule -Cron '*/2 * * * *' -Script $Script
```

## Continuous Script Scheduling

You can schedule a script to run continuously. A new job will start as soon as the previous run of the script has finished. You can also configure the schedule to delay before starting the next job.

The below will schedule script 3 to run continuously with a delay of 10 seconds between each run.

```text
$Script = Get-UAScript -Id 3
New-UASchedule -Continuous -DelaySecond 10 -Script $Script
```

The `-DelaySecond`,`-DelayMinute`, or `-DelayHour` parameter can be used to control time between executions.

