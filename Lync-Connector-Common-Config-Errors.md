#### Win-RM Error code 403

Check that URI for /OCSPowerShell contains both HTTPS:// and FQDN server name, so the link is HTTPS://server.domain.com/OCSPowerShell and not http://server/OCSPowerShell

#### Access Denied

If you get "Access Denied" errors when configuring the connector, make sure that there is no typo in the User and Domain fields and that you entered the correct password for the service account. Also confirm that this user has been granted remote management permission on the Lync admin servers and User Administrator privilege in Lync and Win RM was activated on the Lync server.

#### Management object not found for identity "xxxx"

This may be due to AD replication delays if the AD MA provisions the user on one domain control while Lync Server uses a domain controller from a remote AD site. Configuring a list of preferred domain controllers using PreferredDomainControllerFQDN configuration parameter will minimise / eliminate this error.

Another reason for this error might be that the connector is configured to auto-generate the SIP address, but the values for the required atrribute(s) are not populated in AD. e.g. if the SIP address is to be generated from UPN, the value of UPN must be explicitly populated in AD.

#### There is no primary object class on this image / unexpected-error

If you have an Export run profile set to drop an audit log file, the Export run profile will encounter **unexpected-error** when exporting the objects. Modify the Export run profile not to generate an audit log file.

#### Unable to retrieve schema

If you get an error message saying *"Unable to retrieve schema. Error: An anchor attribute defined by the extension must not be of type Reference or Boolean..."*, make sure that you have copied the **Schema-Lync.xml** file in the *Synchronization Service\Extensions* of FIM Sync Server.


