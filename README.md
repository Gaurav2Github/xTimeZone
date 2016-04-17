[![Build status](https://ci.appveyor.com/api/projects/status/7m4cwgkr5x4igpck/branch/master?svg=true)](https://ci.appveyor.com/project/PowerShell/xtimezone/branch/master)

# xTimezone

The **xTimeZone** module the **xTimeZone** DSC resource for setting the timezone on a machine.
The resource will use CIM to retrieve the current timezone and use .NET reflection to update the timezone if required.
If .NET reflection is not supported on the node (in the case of Nano Server) then tzutil.exe will be used to set the timezone.

## Contributing

Please check out common DSC Resources [contributing guidelines](https://github.com/PowerShell/DscResource.Kit/blob/master/CONTRIBUTING.md).


## Resources

### xTimeZone

* **TimeZone**: Specifies the Time Zone. To discover all valid time zones for this property, use this PowerShell command: `[System.TimeZoneInfo]::GetSystemTimeZones().Id`
* **IsSingleInstance**: Specifies if the resource is a single instance, the value must be 'Yes'

## Versions

### Unreleased

* xTimeZone: Unit tests updated to use standard test template.
             Added Integration tests.
             Resource code updated to match style guidelines.
             Get-TargetResource returns IsSingleInstance value.
             Moved Get-Timezone and Set-Timezone to TimezoneHelper.psm1
             Added unit tests for TimezoneHelper.psm1
             Converted Get-Timezone to use CIM cmdlets.
             Added support for Set-Timezone to use .NET reflection if possible.
* Copied SetTimeZone.ps1 example into Readme.md.
* AppVeyor build machine set to WMF5.

### 1.3.0.0

* Updated tests: now we are deploying xTimeZone instead of overwriting PSModulePath to make tests pass on local machine
* Updated validation attribute of IsSingleInstance parameter to match *.schema.mof

### 1.2.0.0

* Modified schema to follow best practices for singleton resources (changed xTimeZone key to IsSingleInstance)

### 1.1.0.0

* Added tests

### 1.0.0.0

* Initial release with the following resource:
    - xTimeZone

## Examples

### Setting the Time Zone

Set the local time zone to "Tonga Standard Time".

```powershell
Configuration SetTimeZone
{
   Param
   (
       [String[]]$NodeName = $env:COMPUTERNAME,

       [Parameter(Mandatory = $true)]
       [ValidateNotNullorEmpty()]
       [String]$SystemTimeZone
   )

   Import-DSCResource -ModuleName xTimeZone

   Node $NodeName
   {
        xTimeZone TimeZoneExample
        {
            IsSingleInstance = 'Yes'
            TimeZone         = $SystemTimeZone
        }
   }
}

SetTimeZone -NodeName "localhost" -SystemTimeZone "Tonga Standard Time"
Start-DscConfiguration -Path .\SetTimeZone -Wait -Verbose -Force
```
