---
description: >-
  This document outlines the types of configuration files in Universal
  Automation.
---

# Files

## Script Configuration Files

Script configuration files define various properties of scripts that aren't stored directly within them. They are not required but provide configuration such as schedules, tags, descriptions and required PowerShell versions. 

The script configuration files are stored in the `.ua` folder in the same structure as the root folder. They will have the same name as scripts within your repository. 

A script configuration file must return a `UniversalAutomation.ScriptInfo` object. The script configuration file is automatically generated when you use the UA REST API or PowerShell module. 

### Schema 

The following is the class definition for ScriptInfo.

```text
public class ScriptInfo {
    public string Name { get; set; }
    public string Description { get; set; }
    public double ManualTime { get; set; }
    public string[] Tags { get; set; } = new string[0];
    public ScheduleInfo[] Schedules { get; set; } = new ScheduleInfo[0];
    public ScriptStatus Status { get; set; }
    public string FullPath { get; set; }
    public string RequiredPowerShellVersion { get; set; }
    public ActionPreference ErrorAction { get; set; }
}

public class ScheduleInfo
{
    public ScheduleInfo(string cron, string timeZone)
    {
        Cron = cron;
        TimeZone = timeZone;
    }

    public ScheduleInfo(string cron)
    {
        Cron = cron;
    }

    public string Cron { get; set; }
    public string TimeZone { get; set; }
}
```

## Variables

The variables are also stored in configuration files. They are stored within the `.ua\variables.ps1`file. The variables script must return zero or more `UniversalAutomation.VariableInfo`  objects. When you modify variables via the Universal Automation REST API or PowerShell module, the file will automatically be modified and committed to git. 

### Schema

The following is the class definition for VariableInfo.

```text
public class VariableInfo 
{
    public VariableInfo(string name, string value)
    {
        this.Name = name;
        this.Value = value;
    }

    public string Name { get;set; }
    public string Value { get;set; }
}
```

