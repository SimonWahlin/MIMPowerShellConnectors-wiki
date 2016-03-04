### Before You Begin

Before you begin you must download and install FIM PowerShell connector version [1.0.419.911](https://support.microsoft.com/en-us/kb/3008179) or later.

You also need to create a user account in AD to be used as the management agent account. This is the user that will be used by the connector to connect to the Lync Server and perform management tasks.

This management agent account needs to be granted Lync User Administrator privilege. This privilege can be granted by adding the account to the RTCUniversalUserAdmins universal security group.

The management agent account also needs to be granted remote management permission on the Lync server. On Windows Server 2012, this permission can be granted by making the management account a member of "Remote Management Users" local security group on the Lync Server. On Windows Server 2008, this permission can be granted by executing following cmdlet at the elevated PowerShell command prompt and assigning Full Control (All Operations) right.

`Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI`

### Deploying Lync Connector Space Schema File

The connector space schema for the Lync Connector is defined in the **Schema-Lync.xml**. This files needs to be copied to the **<FIM_INSTALLDIR>\2010\Synchronization Service\Extensions** folder before attempting to configure the connector.

### Configuring Connector

You have two options to create the management agent. 1. Create a new management agent from scratch or 2. Import the management agent from the MA export config file. Both the options lead of an identical configuration wizard with the only major difference being in the second option the configuration is pre-populated.

> **Important**: While it is recommended that you import the management agent from the MA export config file, please copy and paste the code of all the scripts from the source code zip afresh as the content in the export file may not be up-to-date.

To Import Lync connector for the MA export config file,

* Ensure that you have installed FIM PowerShell Connector.

* Ensure that **Schema-Lync.xml** is copied to the **<FIM_INSTALLDIR>\2010\Synchronization Service\Extensions** folder.

* Ensure that the connector service account can access Lync Admin server Remote PowerShell url. A sample script that can be executed on a PowerShell command prompt to test connectivity is listed below:
```PowerShell
$server = "https://s4badmin.contoso.com/OcsPowerShell" # the endpoint typically is on HTTPS
$credential = Get-Credential -Credential CONTOSO\svc_fim_s4bma
$skipCertificate = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$session = New-PSSession -ConnectionUri $server -Credential $credential -SessionOption $skipCertificate 
```
* In the **Synchronization Service Manager**, on **Management Agents** tab in **Actions** pane, click **Import Management Agent**.

* Browse to the folder where you downloaded **LyncConnectorConfigExport.xml** and open the file. This will launch the **Create Management Agent** wizard.

* On the **Create Management Agent** page, type a suitable name for the management agent.

![Lync Connector - Create Management Agent](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page1.png)

* On the **Connectivity** page, enter the following details:

Settings|Configuration|
--------|-------------|
Server|Lync Admin Server PowerShell URI e.g. https://s4badmin.contoso.com/OcsPowerShell|
Domain|CONTOSO|
User|svc_fim_s4bma|
Password|**********|
Impersonate Connector Account|Unchecked|
Load User Profile When Impersonating|NA|
Logon Type When Impersonating|NA|
Signed Scripts Only|Unchecked|
Common Module Script Name (with extension)|Lync.Common.psm1|
Common Module Script|Copy and paste content of Lync.Common.psm1 script|
Validation Script|Copy and paste content of ValidationScript-Lync.ps1 script|
Schema Script|Copy and paste content of SchemaScript-Lync.ps1 script|
Additional Config Parameter Names|SipAddressType,SipDomain,ForceMove,UserPages, OrganizationalUnitPages,PreferredDomainControllerFQDN, LastRunDateTimeOffsetMinutes|
Additional Encrypted Config Parameter Names|-|

![Lync Connector - Connectivity](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page2.png)

![Lync Connector - Connectivity](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page2b.png)

![Lync Connector - Connectivity](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page2c.png)

* On the **Capabilities** page, specify the following details:

Settings|Configuration|
--------|-------------|
Distinguished Name Style|Ldap|
Export Type|AttributeUpdate|
Data Normalization|None|
Object Confirmation|NoAddAndDeleteConfirmation|
Use DN As Anchor (Only LDAP style DN)|Unchecked|
Concurrent Operations Of Several Connectors|Checked|
Partitions (Only LDAP style DN)|Checked|
Hierarchy (Only LDAP style DN)|Checked|
Enable Import|Checked|
Enable Delta Import|Checked |
Enable Export|Checked|
Enable Full Export|Unchecked|
No Reference Values In First Export Pass|Checked|
Enable Object Rename|Checked|
Delete-Add As Replace|Checked|
Enable Password operations|Unchecked|
Enable Export Password In First Pass|Unchecked|

![Lync Connector - Capabilities](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page3.png)

![Lync Connector - Capabilities](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page3b.png)

> **Note**: Delta Import is based on the "WhenChanged" attribute which is neither replicated nor indexed. Since it is not replicated, connector fetches all objects modified since T – LastRunDateTimeOffsetMinutes. The default value for LastRunDateTimeOffsetMinutes additional configuration parameter is 30 minutes. Even though the "WhenChanged" attribute is not indexed, it has been found to provides good performance gain during Delta Import.
>
> Any deletion directly in Lync / AD will not be detected in Delta Import. A periodic Full import is needed to flush out any external deletes from the connector space. Another approach that may be adopted is that since this connector is meant to be used in conjunction with AD MA, the deletions detected in AD MA can be used to trigger deletions in this Connector as well.

* On the **Global Parameters** page, specify the following details:

Settings|Configuration|
--------|-------------|
Partition Script|Copy and paste content of PartitionScript-Lync.ps1 script|
Hierarchy Script|Copy and paste content of HierarchyScript-Lync.ps1 script|
Begin Import Script|Copy and paste content of Begin-ImportScript-Lync.ps1 script|
Import Script|Copy and paste content of ImportScript-Lync.ps1 script|
End Import Script|Copy and paste content of End-ImportScript-Lync.ps1 script|
Begin Export Script|Copy and paste content of Begin-ExportScript-Lync.ps1 script|
Export Script|Copy and paste content of ExportScript-Lync.ps1 script|
End Export Script|Copy and paste content of End-ExportScript-Lync.ps1 script|
Begin Password Script|-|
Password Extension Script|-|
End Password Script|-|
SipAddressType_Global|UserPrincipalName|
SipDomain_Global|-|
ForceMove_Global|Yes|
UserPages_Global|a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z,0,1,2,3,4,5,6,7,8,9|
OrganizationalUnitPages_Global|Leave Blank|
PreferredDomainControllerFQDN_Global|Leave Blank OR specify the same list (comma separated) of Domain Controllers in the same order as used by AD MA to avoid AD replication delays if this is a single domain environment. For multiple domain setup, specify the domain controller list on the corresponding partition configuration.|
LastRunDateTimeOffsetMinutes|Leave Blank OR specify a value based on the estimated AD replication delays.|

![Lync Connector - Global Parameters](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page4.png)

![Lync Connector - Global Parameters](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page4b.png)

![Lync Connector - Global Parameters](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page4c.png)

![Lync Connector - Global Parameters](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page4d.png)

> **Note**: The connector scripts will prioritize the most specific configuration and use it when provided. The configuration is prioritize the configuration value in the following descending order:
>	1. RunStep
>	2. Partition
>	3. Global
>	
> As an example, the configuration value specified for PreferredDomainControllerFQDN_Partition will be used to connect to specified partition. If no value is specified for a partition, PreferredDomainControllerFQDN_Global will be used.

* On the **Configure Provisioning Hierarchy** page, map DN Component **Ou** to  Directory ObjectClass **OrganizationalUnit**:

![Lync Connector - Configure Provisioning Hierarchy](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page5.png)

* On the **Configure Partitions and Hierarchies** page, select the directory partitions and containers where users are located:

![Lync Connector - Configure Partitions and Hierarchies](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page6.png)

![Lync Connector - Configure Partitions and Hierarchies](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page6b.png)

* On the **Select Object Types** page, select OrganizationalUnit and User:

![Lync Connector - Select Object Types](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page7.png)

* On the **Select Attributes** page, select all attributes (of interest):

![Lync Connector - Select Attributes](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page8.png)

* On the **Configure Anchors** page, click Next:

![Lync Connector - Configure Anchors](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-Import-Page9.png)

* From here on, rest of the pages of the wizard need to be configured as per your solution requirements.

* Once the MA is configured, configure the MA Run Profiles for each domain partition in the connected forest. 

### Lync Provisioning

The Lync Connector uses Active Directory DN of the User objects as the "Identity" of the user in Lync. The Lync Connector also assumes that you already have the ADDS MA configured to provision users in the ADDS forest. The users need to exist in the ADDS forest first before the Lync connector can provision them in Lync.

Provisioning using Lync Connector requires, at the minimum, two attributes, DN and RegistrarPool. The third mandatory attribute SipAddress required by Lync can be auto-generated by Lync based on the settings of additional config parameters SipAddressType and SipDomain specified on the MA configuration.

#### Provisioning Using a Metaverse Rules Extension 

If your provisioning implementation is based on a metaverse rules extension, the following code snippet shows a simplified example for this.

```C#

private static void ProvisionLyncUser(MVEntry mventry)
{
    const string AgentName = "Skype"; // TODO: Read the Management Agent name from a config file

    ConnectedMA managementAgent = mventry.ConnectedMAs[AgentName];

    int connectors = managementAgent.Connectors.Count;
    if (connectors == 0)
    {
        CSEntry csentry = managementAgent.Connectors.StartNewConnector("User");

        csentry.DN = managementAgent.CreateDN(mventry["xActiveDirectoryDN"].StringValue);
 
        // set any additional mandatory attributes
        ////csentry["SipAddress"].StringValue = "sip:" + mventry["xSipAddress"].StringValue; // Let it auto-generate based on configuration options specified on MA.
        ////csentry["RegistrarPool"].StringValue = "pool1.contoso.com"; // Advance export attribute flow is a better place to populate RegistrarPool attribute.

        csentry.CommitNewConnector();
    }
    else if (connectors == 1)
    {
        CSEntry csentry = managementAgent.Connectors.ByIndex[0];

        csentry.DN = managementAgent.CreateDN(mventry["xActiveDirectoryDN"].StringValue);

        csentry.CommitNewConnector();
    }
    else if (connectors > 1)
    {
        string error = "Multiple connectors on the management agent";
        throw new UnexpectedDataException(error);
    }
}

```

> **Note**: The above sample code assumes that you have extended the metaverse schema with an attribute **xActiveDirectoryDN** of type string (index) and configured an import flow for it on the AD MA so that the it gets populated from the DN value from AD.

#### Provisioning Using Declarative Provisioning

When you configure an outbound synchronization rule, you need to configure an initial flow for the DN attribute. You can define the Outbound System Scoping filter such that only the users confirmed to an Active Directory account are provisioned in Lync connector space. A sample OSR is demonstrated below:

<table>
    <tr>
        <td colspan="4"><b>Synchronization Rule: </b>!Skype: Entitled Users OSR</td>
    </tr>
    <tr>
        <td colspan="4"><b>General</b></td>
    </tr>
    <tr>
        <td><b>Display Name</b></td>
        <td colspan="3">!Skype: Entitled Users OSR</td>
    </tr>
    <tr>
        <td><b>Description</b></td>
        <td colspan="3"></td>
    </tr>
    <tr>
        <td><b>Dependency</b></td>
        <td colspan="3"></td>
    </tr>
    <tr>
        <td><b>Data Flow Direction</b></td>
        <td colspan="3">Outbound</td>
    </tr>
    <tr>
        <td><b>Apply Rule</b></td>
        <td colspan="3">Filter-based Outbound Synchronization</td>
    </tr>
    <tr>
        <td colspan="4"><b>Scope</b></td>
    </tr>
    <tr>
        <td><b>Metaverse Resource Type</b></td>
        <td colspan="3">person</td>
    </tr>
    <tr>
        <td><b>External System</b></td>
        <td colspan="3">Skype</td>
    </tr>
    <tr>
        <td><b>External System Resource Type</b></td>
        <td colspan="3">User</td>
    </tr>
    <tr>
        <td rowspan="3"><b>Outbound System Scoping Filter</b></td>
        <td><b>Attribute<b></td>
        <td><b>Operator<b></td>
        <td><b>Value<b></td>
    </tr>
    <tr>
        <td>xActiveDirectoryDN</td>
        <td>NOTEQUAL</td>
        <td></td>
    </tr>
    <tr>
        <td colspan="4"><b>Relationship</b></td>
    </tr>
    <tr>
        <td><b>Create Resource In External System</b></td>
        <td colspan="3">True</td>
    </tr>
    <tr>
        <td colspan="4"><b>Outbound Attribute Flow</b></td>
    </tr>
    <tr>
        <td><b>Initial Flow Only</b></td>
        <td><b>Use as Existence Test</b></td>
        <td><b>Allow Null</b></td>
        <td><b>Flow(FIM Value => Destination Attribute)</b></td>
    </tr>
    <tr>
        <td>True</td>
        <td>False</td>
        <td>False</td>
        <td>xActiveDirectoryDN → dn</td>
    </tr>
    <tr>
        <td>False</td>
        <td>False</td>
        <td>False</td>
        <td>pool1.contoso.com → RegistrarPool</td>
    </tr>
    <tr>
        <td>False</td>
        <td>False</td>
        <td>False</td>
        <td>CustomExpression(IIF(Eq(xSkypeEntitlement,"Active"),"true","false" → Enabled</td>
    </tr>
</table>

> **Note**: The above sample code assumes that you have extended the metaverse schema with an attribute **xActiveDirectoryDN** of type string (index) and an attribute **xSkypeEntitlement** of type boolean. The **xActiveDirectoryDN** attrbute is configured an import flow for it on the AD MA so that the it gets populated from the DN value from AD.


### Lync Connector Customization

#### SIP Address

If the connector space User object of Lync Connector does not have the "SipAddress" attribute populated, the Lync connector will instruct Lync Server to auto-generate the SIP address based on the data specified in the SipAddressType / SipDomain "Additional Config Parameter" configuration.
 
* To have the SIP address auto-generated as the user's first name and a period followed by the user's last name and the SIP domain, use:

Additional Config Parameter Name|Configuration|
--------------------------------|-------------|
SipAddressType|FirstLastName|
SipDomain|contoso.com|

* To have the SIP address auto-generated as the user's email address, use:

Additional Config Parameter Name|Configuration|
--------------------------------|-------------|
SipAddressType|EmailAddress|

* To have the SIP address auto-generated as the user's UPN, use:

Additional Config Parameter Name|Configuration|
--------------------------------|-------------|
SipAddressType|UserPrincipalName|

* To have the SIP address auto-generated as the user's SamAccountName (logon name) use:

Additional Config Parameter Name|Configuration|
--------------------------------|-------------|
SipAddressType|SamAccountName|
SipDomain|contoso.com|

#### Force Move

When you move a user to a new Registrar pool, you can choose what happens to the associated user data (such as conferences that the user has scheduled). By fault, both the account and the associated data are moved. To delete any associated data during move, use:

Additional Config Parameter Name|Configuration|
--------------------------------|-------------|
ForceMove|Yes|

#### Paging Filter

During Import Operation, the Lync Connector fetched user in batches based on the first character of their SIP address. All users starting their SIP address with character "a" are fetched first, then those with their SIP address with character "b", and so on.
 
If there are users starting their SIP address with any character other than a to z and 0 to 9, you can modify the "pages" array by specifying those additional characters in UserPages_Global configuration parameter.
 
You can also modify this parameter, if any of the pages return more than batch size defined on the Import Run Profile.

#### Preferred Domain Controller

The configuration parameter PreferredDomainControllerFQDN enables you to connect to the specified domain controller when running Lync cmdlets that support “DomainController” parameter. Specify the same list (comma separated) of Domain Controllers in the same order as used by AD MA to avoid AD replication delays. During every export run, the connector will go thru the preferred domain controller list and select the first online domain controller. If the list is not configured, the DC automatically used by the Lync cmdlets is the one what is close to the Lync server (and not FIM Synchronization server).
