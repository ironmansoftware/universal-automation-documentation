# Jobs

Jobs are executions of a particular script. You can start a new job with `Invoke-UAScript` or through a schedule. Jobs run in PowerShell.exe or Pwsh.exe, depending on the version of PowerShell that was selected. Jobs will report output, pipeline output, progress and will even wait for input. 

## Starting a job

To start a job with the PowerShell module, use `Invoke-UAScript`.

```text
$Script = Get-UAScript -Id 2
Invoke-UAScript -Script $Script
```

## Getting Output from a job

To get the output from a job, use `Get-UAJobOutput`. Job output is any string output that may common from one of the various output streams in PowerShell. This may include error, warning, verbose or debug output.

```text
$Job = Get-UAJob -Id 1
Get-UAJobOuput -Job $Job 
```

## Getting Pipeline output

Pipeline output is stored as CliXml. You can use the returned objects as any other object in PowerShell.

```text
$Job = Get-UAJob -Id 1
Get-UAJobPipelineOuput -Job $Job 
```

## Dealing with Feedback

Feedback can come from commands that request it, such as a cmdlet that requires additional parameters to be set, or from invoking a cmdlet like `Read-Host`. When feedback is requested by a job, the job will enter a `WaitingForFeedback` state. You can find jobs that are waiting for feedback by using `Get-UAJob`. You can set the feedback for a job using `Set-UAJobFeedback`.

## Variables Defined in Jobs

**$UAScript** 

The currently executing script. This will be a Script object. 

```text
Get-UAScript | Get-Member
```

**$UAJob**

The currently executing job. This will be a Job object. 

```text
Get-UAJob | Get-Member
```

**$UASchedule**

The schedule that started this script. If this script was not started from a schedule, this variable will not be present. If the variable is present, it will be a Schedule object. 

```text
Get-UASchedule | Get-Member
```

**$parentJobId**

The ID of the parent job that invoked the current job. This variable will be $null if this job was invoked by a user request or schedule. 

