The Windows PowerShell connector supports logging and tracing of connector and script activities for troubleshooting purposes.

Critical errors will be logged to the Application log on the Synchronization Service server and additional tracing can be enabled by the administrator. 

#Event Log
When a fatal error occurs while the PowerShell connector is running, or during the configuration of the connector in the Management Agent Designer, the Synchronization Service will log an event with the following details:

| Parameter | Value |
| --- | --- |
| Log | Application 
| Level | Error 
| Source | FIMSynchronizationService 
| Event ID | 6801 
| Task Category | Server 

#Logging/Tracing
The Windows PowerShell connector can be configured to emit tracing information to any .NET TraceListener (e.g. event log, XML file, text file, etc.).

Administrators can also configure the connector to log information produced by the Write-Warning, Write-Verbose, and Write-Debug cmdlets to the trace log. 

##Enabling Tracing
1. Open the %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\bin\miiserver.exe.config file using a text editor. 
2. Paste the following XML in to the file on the line immediately following the sources tag.

 ```xml
<source name="ConnectorsLog" switchValue="Verbose">
       <listeners>
             <add initializeData="C:\Logs\ConnectorsTrace.log"
                   type="System.Diagnostics.TextWriterTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
                   name="ConnectorsTraceListener">
                   <filter type="" />
             </add>
       </listeners>
 </source> 
```
 The <System.Diagnostics> section of the miiserver.exe.config file should now resemble the following excerpt:
![](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/wiki/FlatFileConnector/Fig0030.jpg)

3. Create the directory C:\logs. 
4. Grant the Synchronization Service service account Modify permissions to the C:\logs directory. 
5. Restart the Synchronization Service. 

##PowerShell Script Tracing
To trace information from Windows PowerShell scripts, you must complete two steps:
1. Add logging to your scripts in the form of Write-Verbose, or Write-Debug statements. 
2. Enable $VerbosePreference or $DebugPreference in the properties of the Run Step as show below. 

![](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/wiki/FlatFileConnector/Fig0040.jpg)

