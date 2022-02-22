# Attire-ExecutionLogger
This is a PowerShell module that conforms to the logger API used by Invoke-AtomicRedTeam and generates ATTiRe logs that can be imported into VECTR.

To use the ATTiRe logger, first you must:

 1. Import Invoke-AtomicRedTeam
 2. Import Attire-ExecutionLogger
 3. Run Invoke-AtomicTest and pass in the ATTiRe logger as an argument

The example below runs Invoke-AtomicTest and generates an ATTiRe log for the T1087.001 test cases.

```
Import-Module ".\Invoke-AtomicRedTeam.psd1" -Force
Import-Module ".\Loggers\Attire-ExecutionLogger.psm1" -Force
Invoke-AtomicTest T1087.001 -LoggingModule "Attire-ExecutionLogger" -ExecutionLogPath "./attireLog.json"
```







## Developer documentation

### Invoke-Process.ps1

There is now a new method of process creation when running in non-interactive mode. This method uses System.Diagnostics.ProcessStartInfo from the .NET API to create and retrieve output from a process. This method does not require writing files to disk to retrieve output from the process.

The previous method of using Start-Process is still used when running in interactive mode.

### Invoke-AtomicTest.ps1

There are several changes in this file to facilitate logging. There is a new input argument for the user to specify a logger and new code to load and validate the logger module.

There is a section of code to re-create an equivalent command line passed into Invoke-AtomicTest. This is needed for the ATTiRe logger, but the code needs to be in Invoke-AtomicTest to access the input arguments. The rebuilt command line may explicitly contain arguments with default values that the user did not set.

There are now 3 places where Invoke-AtomicTest passes information to the logger module through the functions Start-ExecutionLog, Write-ExecutionLog, and Stop-ExecutionLog. Start-ExecutionLog and Stop-ExecutionLog are each called once: before and after all the test cases have run, respectively. These functions are to allow the logger to perform any setup or cleanup work it may need. Write-ExecutionLog is called once after each test case and is where any of the actual logging work is performed.

### Default-ExecutionLogger.psm1

Write-ExecutionLog.ps1 has been renamed to Default-ExecutionLogger.psm1 and moved to the Public directory. The Write-ExecutionLog function remains, but with additional arguments to match the new API. These new arguments are not used by the function. Start-ExecutionLog and Stop-ExecutionLog have also been added but are empty functions. It generates the same logs as before.

