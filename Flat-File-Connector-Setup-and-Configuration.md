To create the connector described in this section, configure the tabs in the Management Agent Designer as described in the  [Windows PowerShell Connector for FIM 2010 R2 Technical Reference](http://technet.microsoft.com/en-us/library/dn640417(v=ws.10).aspx).

## Template File
The sample connector uses a template file to provide schema information to the Synchronization Service. Create a text file in the Extension folder called SampleInputFile.txt.

By default, the Extensions folder can be found at:

    %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions\

Paste the data located in [this file](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/src/FlatFileConnector/SampleInputFile.txt) in to SampleInputFile.txt.

## Connectivity
| Parameter     | Value       |
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

## Capabilities
| Parameter     | Value       |
| ------------- |-------------| 
| Distinguished Name Style | None
| Export Type | ObjectReplace 
| Data Normalization | None 
| Object Confirmation | Normal 
| Use DN as Anchor | Unchecked 
| Concurrent Operations of Several Connectors | Checked 
| Partitions | Unchecked 
| Hierarchy | Unchecked 
| Enable Import | Checked 
| Enable Delta Import | Unchecked 
| Enable Export | Checked 
| Enable Full Export | Checked 
| No Reference Values In First Export Pass | Unchecked 
| Enable Object Rename | Unchecked 
| Delete-Add As Replace | Checked 
| Enable Password Operations | Unchecked 
| Enable Export Password in First Pass | Checked 

## Global Parameters
| Parameter     | Value       |
| ------------- |-------------| 
| Partition Script | <blank> 
| Hierarchy Script | <blank> 
| Begin Import Script | <blank> 
| Import Script | Paste [ImportData code](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/src/FlatFileConnector/ImportData.ps1) as value 
| End Import Script | <blank> 
| Begin Export Script | <blank> 
| Export Script | Paste [ExportData code](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/src/FlatFileConnector/ExportData.ps1) as value 
| End Export Script | <blank> 
| Begin Password Script | <blank> 
| Password Extension Script | <blank> 
| End Password Script | <blank> 
| FileName_Global | InputFile.txt  
| Delimiter_Global | ; 
| Encoding_Global | <blank> 

## Configure Partitions and Hierarchies
Preserve the defaults.
## Select Object Types
Select the Row Object Type as shown below.
![](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/wiki/FlatFileConnector/Fig0025.jpg)
## Select Attributes
Select each of the four attributes (EmployeeID, FaxPhone, MobilePhone, OfficePhone) as shown below.
![](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/wiki/FlatFileConnector/Fig0026.jpg)
## Configure Anchors
Specify the EmployeeID attribute as the Anchor attribute for the Row Object Type as shown below.
![](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/wiki/FlatFileConnector/Fig0027.jpg)
## Run Profiles
Once the connector has been created, create a run profile with a Full Import run step. Create an additional run profile with a Full Export run step. 