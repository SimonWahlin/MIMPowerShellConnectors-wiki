### Troubleshooting

The Lync Connector makes use of the logging and tracing capabilities of FIM PowerShell connector. Critical errors will be logged to the Application log on the Synchronization Service server and additional tracing can be enabled by the administrator by configuring the trace levels of the FIM PowerShell connector.

#### Enable Tracing Event Log

To enable tracing to log to event log:

* Copy the script `Register-EventSource.ps1` located in the `src\LyncConnector\EventLogConfig` folder to a local folder on the FIM Synchronization Server and execute it on an elevate PowerShell command prompt.
* The create / edit the <system.diagnostics> configuration elements in application config files as directed in the `app.config` file located in the `src\LyncConnector\EventLogConfig` folder. After the application config files are updated, restart the FIM Synchronization Service.

### Using PowerShell ISE for Interactive Debugging.

The Test Harness scripts, `TestHarness-Lync.ps1` and `TestHarness.Common.psm1` that are part of the Lync Connector script library are meant to be used for interactive debugging / development in PowerShell ISE. These scripts make it possible to develop / debug the connector scripts in PowerShell ISE independent of FIM Sync Service providing an experience very similar to "Attach to Process" develop / debug experience in Visual Studio for native .NET Connectors.

To use the Test Harness:

* Download the source code to a desktop / development server which has connectivity to the Lync Server.

> **Important**: Make sure you have UNBLOCKED the source code zip file before extracting the files.

* Make sure the desktop / development server has PowerShell ISE installed. It's comes as part of Windows PowerShell feature. 
* Copy the `Microsoft.MetadirectoryServices.dll` and `Microsoft.MetadirectoryServicesEx.dll` files from `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Bin\Assemblies` and place them in the `src\ReferencedAssemblies` directory.
* Go to the `src\LyncConnector\Scripts` folder and right-click on the `TestHarness-Lync.ps1` script and using Edit menu open the file in the PowerShell ISE.
* Locate the "MA Configuration" region and modify the `Get-ConfigParameterKeyedCollection` function as per your environment. Typically you'll modify the configuration for `Server`, `Domain`, `User`, `Password` and `PartitionDN` as applicable. If you want to debug an export issue, you may want to edit the name of Export Drop file as well by modifying the `$exportDropFile` variable suitably.
* Towards the end of the script, there are functions that call individual Lync Connector Scripts. You can put a beak-point in the appropriate function / line or you can open the script you want to debug, e.g. `10. ExportScript-Lync.ps1` in the same PowerShell ISE window and put a break point in it. Then with the tab for `TestHarness-Lync.ps1` script in focus, hit "F5" to start the debugging.