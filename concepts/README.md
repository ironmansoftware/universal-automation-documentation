# Concepts

Universal Automation is a PowerShell Module that provides an robust Powershell Automation Platform.

## Scripts

Scripts are PowerShell Scripts. These are the main unit of automation that can be executed or scheduled for execution.

## Jobs

Jobs are an instance of an executed script. Each job runs in it's own PowerShell.exe or Pwsh.exe process to completely isolate the script from other scripts in the environment. A custom PowerShell host collects information about the running script and communicates it back to the UA job execution service. 

A Job can have a variety of statuses indicating the state of execution

* Queued - a job is currently preparing to be executed
* Running - a job is currently being executed
* Completed - a job has completed
* Failed - a job has failed
* WaitingOnFeedback - The job is pending feedback before continuing
* Canceled - The job was canceled before it could complete
* Canceling - The job is currently in the process of stopping after being instructed to cancel

## Job Output

Jobs can have output. There are two types of output: "Standard Output" and "Pipeline Output"

### Standard Job Output

Standard job output is composed of just simple strings. This typically contains some error messages or debug messages or very simple text output.

#### Example

Write-Host "Hello World" will simply show "Hello World" on the Job Output

### Pipeline Job Output

Pipeline job output are actual rich PowerShell objects output by the script. They are stored in the UA database as CliXml so that they can be deserialized and used in other scripts or passed to other jobs. 

#### Example

Using the Get-Process cmdlet will output a PowerShell object contain running processes.

#### Cmdlet

See the `Get-UAJobOutput` cmdlet.

## Feedback

Jobs can have feedback - this allows direct interaction with an in-progress job.

If a script has a `Read-Host` present, this will be interpreted as "feedback" and the script will be paused until a response \(Feedback\) is provided to the job.

See the `Get-UAJobFeedback` cmdlet.

## Schedules

Scripts can be configure with a Schedule so that is executed on a regular basis. Schedules currently are defined via CRON format.

See the `Get-UASchedule` cmdlet.

## Tags

Tags are a method used to organize scripts. Tags are applied to script to allow for lightweight organization of scripts by being able to retrieve scripts based on their tag.

See the `Get-UATag` and `New-UAScript` cmdlets.

## Variables

Variables allow for light storage and retrievable of values accessible to any Script.

Variables can be created, read, and updated by scripts.

This allows scripts to be used lightweight/simple storage for variables.

### Variable Examples

* Email Addresses
* Server Addresses
* URLs / Webhook API URIs
* Timestamps \("LastUpdated", "LastRecordProcessed"\)

### Encrypted Variables

Variables can be encrypted.

### Using Variables

`New-UAVariable` `Set-UAVariable` `Remove-UAVariable`

### Examples of Variables

### Advanced Example of Variables

