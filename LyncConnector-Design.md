### Conceptual Architecture

The following diagram illustrates the conceptual architecture of the Lync Connector based on FIM PowerShell Connector.

![Lync Connector Conceptual Architecture](https://github.com/Microsoft/MIMPowerShellConnectors/wiki/images/LyncConnector-ConceptualArchitecture.png)

As depicted in the preceding diagram, the Lync Connector uses remote PowerShell to invoke various Lync command-lets to perform desired provisioning tasks in the Lync Sever.

### Connector Capabilities

Settings|Configuration|
--------|-------------|
Distinguished Name Style|Ldap|
Export Type|AttributeUpdate|
Data Normalization|None|
Object Confirmation|NoAddAndDeleteConfirmation|
Use DN As Anchor (Only LDAP style DN)|No|
Concurrent Operations Of Several Connectors|Yes|
Partitions (Only LDAP style DN)|Yes|
Hierarchy (Only LDAP style DN)|Yes|
Enable Import|Yes|
Enable Delta Import|Yes|
Enable Export|Yes|
Enable Full Export|No|
No Reference Values In First Export Pass|Yes|
Enable Object Rename|Yes|
Delete-Add As Replace|Yes|
Enable Password operations|No|
Enable Export Password In First Pass|No|

> **Note** : Delta Import is based on the "WhenChanged" attribute which is neither replicated not indexed. Since it is not replicated, connector fetch all objects modified since T – LastRunDateTimeOffsetMinutes. The default value for LastRunDateTimeOffsetMinutes additional configuration parameter is 30 minutes. Even though the "WhenChanged" attribute is not indexed, it has been found to provides good performance gain during Delta Import.
>
> Any deletion directly in Lync / AD will not be detected in Delta Import. A periodic Full import is needed to flush out any external deletes from the connector space. Another approach that may be adopted is that since this connector is meant to be used in conjunction with AD MA, the deletions detected in AD MA can be used to trigger deletions in this Connector as well.

### Connector Space Design

The Lync Connector connector space defines two object type: `OrganizationalUnit` and `User`. The schema for the User object type is defined in the following table:

Attribute|Description|Type|Multi-valued|Anchor|Required|
----|----|----|----|----|----|
ObjectGUID|The AD ObjectGUID of the user account.|Binary|N|Y|Y|
Identity|The Identity of the user account. The ECMA expects the User Identities be specified by using one of the formats: 1) the user's SIP address; 2) the user's user principal name (UPN); 3) the user's domain name and logon name, in the form domain\logon (for example, litwareinc\kenmyer); or 4) Active Directory distinguished name of the user.|String|N|N|N|
RegistrarPool|The FQDN of the Registrar pool where the user's Lync Server account will be homed|String|N|N|Y|
SipAddress|This attribute is required only if the SipAddressType configuration parameter is specified as None. Otherwise it is optional. If this attribute is populated, the users Lync SIP address will be updated to reflect this value.|String|N|N|N?|
AudioVideoDisabled|Indicates whether the user is allowed to make audio/visual (A/V) calls by using Lync 2010. If set to True, the user will largely be restricted to sending and receiving instant messages|Boolean|N|N|N|
Enabled|Indicates whether or not the user has been enabled for Lync Server. If you set this value to False, the user will no longer be able to log on to Lync Server; setting this value to True re-enables the user's logon privileges. If you disable an account by using the Enabled parameter, the information associated with that account (including assigned policies and whether or not the user is enabled for Enterprise Voice and/or remote call control) is retained. If you later re-enable the account by using the Enabled parameter, the associated account information will be restored. The default value for this attribute is assumed to be True.|Boolean|N|N|N|
EnterpriseVoiceEnabled|Indicates whether the user has been enabled for Enterprise Voice, which is the Microsoft implementation of Voice over Internet Protocol (VoIP). With Enterprise Voice, users can make telephone calls using the Internet rather than using the standard telephone network.|Boolean|N|N|N|
HostedVoiceMail|When set to True, enables a user’s voice mail calls to be routed to a hosted version of Microsoft Exchange Server. In addition, setting this option to True enables Lync 2010 users to directly place a call to another user’s voice mail.|Boolean|N|N|N|
LineURI|Phone number assigned to the user. The line Uniform Resource Identifier (URI) must be specified using the E.164 format and use the "TEL:" prefix. For example: TEL:+14255551297. Any extension number should be added to the end of the line URI, for example: TEL:+14255551297;ext=51297.|String|N|N|N|
LineServerURI|The URI of the remote call control telephone gateway assigned to the user. The LineServerUri is the gateway URI, prefaced by "sip:”. For example: sip:rccgateway@litwareinc.com|String|N|N|N|
PrivateLine|Phone number for the user's private telephone line. A private line is a phone number that is not published in Active Directory Domain Services (AD DS) and, as a result, is not readily available to other people. In addition, this private line bypasses most in-bound call routing rules; for example, a call to a private line will not be forwarded to a person's delegates. Private lines are often used for personal phone calls or for business calls that should be kept separate from other team members. The private line value should be specified using the E.164 format, and be prefixed by the "TEL:" prefix. For example: TEL:+14255551297.|String|N|N|N|
RemoteCallControl-TelephonyEnabled|Indicates whether the user has been enabled for remote call control telephony. When enabled for remote call control, a user can employ Lync Server to answer phone calls made to his or her desk phone. Phone calls can also be made using Lync 2010. These calls all rely on the standard telephone network, also known as the public switched telephone network (PSTN). To make and receive phone calls over the Internet, the user must be enabled for Enterprise Voice. For details, see the parameter EnterpriseVoiceEnabled. To be enabled for remote call control, a user must have both a LineUri and a LineServerUri.|Boolean|N|N|N|
ArchivingPolicy|Assigns instant messaging (IM) session archiving policy to the user. The specified value is the "Name" of the policy to be assigned. The PolicyName is simply the policy Identity minus the scope designator "tag:". For example, a policy with the Identity tag:Redmond has a PolicyName equal to Redmond. To remove a per-user policy that has been assigned to a user, set PolicyName to a null value|String|N|N|N|
ClientPolicy|Assigns a client policy to the user. Among other things, client policies help determine the features of Microsoft Lync 2010 that are available to users; for example, you might give some users the right to transfer files while denying this right to other users. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
ClientVersionPolicy|Assigns a client version policy to the user. Client version policies enable you to specify which clients (such as Microsoft Office Communicator 2007 R2) will be able to log on to your Microsoft Lync Server 2010 system. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
ConferencingPolicy|Assigns a conferencing policy at the user. Conferencing policies determine the features and capabilities that can be used in a conference. This includes everything from whether or not the meeting can include IP audio and video to the maximum number of people who can attend a meeting. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
DialPlan|Assigns a dial plan to the user. Dial plans provide information required to enable Enterprise Voice users to make telephone calls. Users who do not have a valid dial plan will not be enabled to make calls by using Enterprise Voice. A dial plan determines such things as how normalization rules are applied and whether a prefix must be dialed for external calls. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
ExternalAccessPolicy|Assigns an external access policy to the user. External access policies determine whether or not your users can: 1) communicate with users who have Session Initiation Protocol (SIP) accounts with a federated organization; 2) communicate with users who have SIP accounts with a public instant messaging (IM) provider such as MSN; and, 3) access Microsoft Lync Server 2010 over the Internet, without having to log on to your internal network. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
HostedVoicemailPolicy|Assigns a hosted voice mail policy at the user. A hosted voice mail policy specifies how to route unanswered calls to a user to a hosted Exchange Unified Messaging (UM) service. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
LocationPolicy|Assigns an Enhanced 9-1-1 (E9-1-1) location policy to the user. The E9-1-1 service enables those who answer 911 calls to determine the caller’s geographic location. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
MobilityPolicy|Grants a per-user mobility policy to a user or group of users. Mobility policies determine whether or not a user can use Microsoft Lync 2010 Mobile. These policies also manage a user's ability to employ Call via Work, a feature that enables users to make and receive phone calls on their mobile phone by using their work phone number instead of their mobile phone number. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
PinPolicy|Assigns a client personal identification number (PIN) policy to the user. PIN authentication enables users to access Microsoft Lync Server 2010 by providing a PIN instead of a user name and password. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
PresencePolicy|Grants a presence policy to the user. Presence information, among other things, lets you know whether a contact is available to take part in an instant messaging conversation. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|
VoicePolicy|Assigns a voice policy to the user. Voice policies are used to manage such Enterprise Voice-related features as simultaneous ringing (the ability to have a second phone ring each time someone calls your office phone) and call forwarding. The specified value is the "Name" of the policy to be assigned.|String|N|N|N|

