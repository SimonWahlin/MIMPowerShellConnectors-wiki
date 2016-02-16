### Troubleshooting

The Lync Connector makes use of the logging and tracing capabilities of FIM PowerShell connector. Critical errors will be logged to the Application log on the Synchronization Service server and additional tracing can be enabled by the administrator by configuring the trace levels of the FIM PowerShell connector.

#### Enable Tracing Event Log

To enable tracing to log to event log:

* Copy the script `Register-EventSource.ps1` located in the `src\LyncConnector\EventLogConfig` folder to a local folder on the FIM Synchronization Server and execute it on an elevate PowerShell command prompt.
* The create / edit the <system.diagnostics> configuration elements in application config files as directed in the `app.config` file located in the `src\LyncConnector\EventLogConfig` folder. After the application config files are updated, restart the FIM Synchronization Service.

### Using PowerShell ISE for Interactive Debugging.

The Test Harness scripts, `TestHarness-Lync.ps1` and `TestHarness.Common.psm1` that are part of the Lync Connector script library are meant to be used for interactive debugging / development in PowerShell ISE. These scripts make it possible to develop / debug the connector scripts in PowerShell ISE independent of FIM Sync Service providing an experience very similar to "Attach to Process" develop / debug experience in Visual Studio for native .NET Connectors.

