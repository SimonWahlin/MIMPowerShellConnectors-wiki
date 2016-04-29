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
