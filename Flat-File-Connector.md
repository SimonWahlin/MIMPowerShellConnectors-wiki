#Configuration
To create the connector described in this section, configure the tabs in the Management Agent Designer as described in the  [Windows PowerShell Connector for FIM 2010 R2 Technical Reference](http://technet.microsoft.com/en-us/library/dn640417(v=ws.10).aspx).

##Template File
The sample connector uses a template file to provide schema information to the Synchronization Service. Create a text file in the Extension folder called SampleInputFile.txt.

By default, the Extensions folder can be found at:

    %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions\

Paste the data located in [this file](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/src/FlatFileConnector/SampleInputFile.txt) in to SampleInputFile.txt.

##Connectivity
| Parameter     | Value         |
| ------------- |-------------| 
| Server      | <blank>
| Domain      | <blank>
| User | <blank>      
| Password | <blank> 
| Impersonate Connector Account | Unchecked
| Load User Profile When Impersonating | Unchecked 
| Logon Type When Impersonating | None 
| Signed Scripts Only | Unchecked 
| Common Module Script Name (with extension) | xADSyncPSConnectorModule.psm1 
| Common Module Script | Paste  [AD Sync PS Connector Module code](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/src/Modules/xADSyncPSConnectorModule.psm1) as value 
| Validation Script | <blank> 
| Schema Script | Paste  [GetSchema code](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/src/FlatFileConnector/GetSchema.ps1) as value 
| Additional Config Parameter Names | FileName,Delimiter,Encoding 
| Additional Encrypted Config Parameter Names | <blank> 

##Capabilities

##Global Parameters

##Configure Partitions and Hierarchies

##Select Object Types

##Select Attributes

##Configure Anchors

##Run Profiles