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
