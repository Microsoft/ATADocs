---
# required metadata

title: Troubleshooting ATA known issues | Microsoft Docs 
description: Describes how you can troubleshoot known issues in Advanced Threat Analytics 
keywords:
author: rkarlin
ms.author: rkarlin
manager: mbaldwin
ms.date: 11/7/2017
ms.topic: article
ms.prod:
ms.service: advanced-threat-analytics
ms.technology:
ms.assetid: d89e7aff-a6ef-48a3-ae87-6ac2e39f3bdb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: arzinger

ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

*Applies to: Advanced Threat Analytics version 1.8*



# Troubleshooting ATA known issues

This section details possible errors in the deployments of ATA and the steps required for troubleshooting them.

## ATA Gateway and Lightweight Gateway errors

> [!div class="mx-tableFixed"]
|Error|Description|Resolution|
|-------------|----------|---------|
|System.DirectoryServices.Protocols.LdapException: A local error occurred|The ATA Gateway failed to authenticate against the domain controller.|1. Confirm that the domain controller’s DNS record is configured properly in the DNS server. <br>2. Verify that the time of the ATA Gateway is synchronized with the time of the domain controller.|
|System.IdentityModel.Tokens.SecurityTokenValidationException: Failed to validate certificate chain|The ATA Gateway failed to validate the certificate of the ATA Center.|1. Verify that the Root CA certificate is installed in the trusted certificate authority certificate store on the ATA Gateway. <br>2. Validate that the certificate revocation list (CRL) is available and that certificate revocation validation can be performed.|
|Microsoft.Common.ExtendedException: Failed to parse time generated|The ATA Gateway failed to parse syslog messages that were forwarded from the SIEM.|Verify that the SIEM is configured to forward the messages in one of the formats that are supported by ATA.|
|System.ServiceModel.FaultException: An error occurred when verifying security for the message.|The ATA Gateway failed to authenticate against ATA Center.|Verify that the time of the ATA Gateway is synchronized with the time of the ATA Center.|
|System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://center.ip.addr:443/IEntityReceiver|The ATA Gateway failed to establish connection to the ATA Center.|Ensure that the network settings are correct and that the network connection between the ATA Gateway and the ATA Center is active.|
|System.DirectoryServices.Protocols.LdapException: The LDAP server is unavailable.|The ATA Gateway failed to query the domain controller using the LDAP protocol.|1.Verify that the user account used by ATA to connect to the Active Directory domain has read access to all the objects in the Active Directory tree. <br>2.Make sure that the domain controller is not hardened to prevent LDAP queries from the user account used by ATA.|
|Microsoft.Tri.Infrastructure.ContractException: Contract exception|The ATA Gateway failed to synchronize the configuration from the ATA Center.|Complete configuration of the ATA Gateway in the ATA Console.|
|System.Reflection.ReflectionTypeLoadException: Unable to load one or more of the requested types. Retrieve the LoaderExceptions property for more information.|Message Analyzer is installed on the ATA Gateway.| Uninstall Message Analyzer.|
|Error [Layout] System.OutOfMemoryException: Exception of type 'System.OutOfMemoryException' was thrown.|The ATA Gateway does not have enough memory.|Increase the amount of memory on the domain controller.|
|Fail to start live consumer  ---> Microsoft.Opn.Runtime.Monitoring.MessageSessionException: The PEFNDIS event provider is not ready|PEF (Message Analyzer) was not installed correctly.|If using Hyper-V, try to upgrade Hyper-V Integration services otherwise, contact support for a workaround.|
|Installation failed with error: 0x80070652|There are other pending installations on your computer.|Wait for the other installations to complete and, if necessary, restart the computer.|
|System.InvalidOperationException: Instance 'Microsoft.Tri.Gateway' does not exist in the specified Category.|PIDs was enabled for process names in the ATA Gateway|Use [KB281884](https://support.microsoft.com/kb/281884) to disable PIDs in process names|
|System.InvalidOperationException: Category does not exist.|Counters might be disabled in the registry|Use [KB2554336](https://support.microsoft.com/kb/2554336) to rebuild Performance Counters|
|System.ApplicationException: Unable to start ETW session MMA-ETW-Livecapture-a4f595bd-f567-49a7-b963-20fa4e370329|There is a host entry in the HOSTS file pointing to the machine's shortname|Remove the host entry from C:\Windows\System32\drivers\etc\HOSTS file or change it to an FQDN.|
|System.IO.IOException: Authentication failed because the remote party has closed the transport stream.|TLS 1.0 is disabled on the ATA Gateway, but .Net is set to use TLS 1.2|Use one of the following options: </br> Enable TLS 1.0 on the ATA Gateway </br>Enable TLS 1.2 on .Net by setting the registry keys to use the operating system defaults for SSL and TLS, as follows: </br>`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SystemDefaultTlsVersions"=dword:00000001` </br>`[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319] "SystemDefaultTlsVersions"=dword:00000001`</br>`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001 `</br>`[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319] " SchUseStrongCrypto"=dword:00000001`|
|System.TypeLoadException: Could not load type 'Microsoft.Opn.Runtime.Values.BinaryValueBufferManager' from assembly 'Microsoft.Opn.Runtime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'|ATA Gateway failed to load required parsing files.|Check to see if Microsoft Message Analyzer is currently installed. Message Analyzer is not supported to be installed with the ATA Gateway / Lightweight Gateway. Uninstall Message Analyzer and restart the Gateway service.|
|System.Net.WebException: The remote server returned an error: (407) Proxy Authentication Required|The ATA Gateway communication with the ATA Center is being disrupted by a proxy server.|Disable the proxy on the ATA Gateway machine. <br></br>Note that proxy settings may be per-account.|
|System.IO.DirectoryNotFoundException: The system cannot find the path specified. (Exception from HRESULT: 0x80070003)|One or more of the services needed to operate ATA did not start.|Start the following services: <br></br>Performance Logs and Alerts (PLA), Task Scheduler (Schedule).|
|System.Net.WebException: The remote server returned an error: (403) Forbidden|The ATA Gateway or Lightweight Gateway could was forbidden from establishing an HTTP connection because the ATA Center is not trusted.|Add the NetBIOS name and FQDN of the ATA Center to the trusted websites list and clear the cache on Interne Explorer (or the name of the ATA Center as specified in the configuration if the configured is different than the NetBIOS/FQDN).|
|System.Net.Http.HttpRequestException: PostAsync failed [requestTypeName=StopNetEventSessionRequest]|The ATA Gateway or ATA Lightweight Gateway can't stop and start the ETW session that collects network traffic due to a WMI issue|Follow the instructions in [WMI: Rebuilding the WMI Repository](https://blogs.technet.microsoft.com/askperf/2009/04/13/wmi-rebuilding-the-wmi-repository/) to fix the WMI issue|
|System.Net.Sockets.SocketException: An attempt was made to access a socket in a way forbidden by its access permissions|Another application is using port 514 on the ATA Gateway|Use `netstat -o` to establish which process is using that port.|
|System.IdentityModel.Tokens.SecurityTokenValidationException: Failed to validate certificate thumbprint|ATA Gateway can't recognize the thumbprint of certificated|After you change the certificate on ATA Center follow this steps:<br>1. Download new ATA Gateway package.<br>2. Extract it and copy the value of key "CenterWebClientConfigurationServiceCertificateThumbprints" of GatewayInstallationConfiguration JSON file on notepad.<br> Now you need to update the value of key "CenterWebClientConfigurationServiceCertificateThumbprints" on file <ATA Install directory>\Gateway\GatewayConfiguration.json <br> After this, wait five minutes for the normal starting of service.|
 
## Deployment errors
> [!div class="mx-tableFixed"]
|Error|Description|Resolution|
|-------------|----------|---------|
|.Net Framework 4.6.1 installation fails with error 0x800713ec|The pre-requisites for .Net Framework 4.6.1 are not installed on the server. |Before installing ATA, verify that the windows updates [KB2919442](https://www.microsoft.com/download/details.aspx?id=42135) and [KB2919355](https://support.microsoft.com/kb/2919355) are installed on the server.|
|System.Threading.Tasks.TaskCanceledException: A task was canceled|The deployment process timed out as it could not reach the ATA Center.|1.	Check network connectivity to the ATA Center by browsing to it using its IP address. <br></br>2.	Check for proxy or firewall configuration.|
|System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The remote server returned an error: (407) Proxy Authentication Required.|The deployment process timed out as it could not reach the ATA Center due to a proxy misconfiguration.|Disable the proxy configuration before deployment, then enable the proxy configuration again. Alternatively, you can configure an exception in the proxy.|
|System.Net.Sockets.SocketException: An existing connection was forcibly closed by the remote host||Use one of the following options: </br>Enable TLS 1.0 on the ATA Gateway </br>Enable TLS 1.2 on .Net by setting the registry keys to use the operating system defaults for SSL and TLS, as follows:</br> `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SystemDefaultTlsVersions"=dword:00000001`</br> `[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319] "SystemDefaultTlsVersions"=dword:00000001`</br>`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001` </br>`[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319] " SchUseStrongCrypto"=dword:00000001`|
|Error [\[]DeploymentModel[\]] Failed management authentication [\[]CurrentlyLoggedOnUser=<domain>\<username>Status=FailedAuthentication Exception=[\]]|The deployment process of the ATA Gateway or ATA Lightweight Gateway could not successfully authenticate against the ATA Center|Open a browser from the machine on which the deployment process failed and see if you can reach the ATA Console. </br>If not, start troubleshooting to see why the browser can't authenticate against the ATA Center. </br>Things to check: </br>Proxy configuration</br>Networking issues</br>Group policy settings for authentication on that machine that differs from the ATA Center.|





## ATA Gateway and Lightweight Gateway issues

> [!div class="mx-tableFixed"]
|Issue|Description|Resolution|
|-------------|----------|---------|
|No traffic received from domain controller, but monitoring alerts are observed|	No traffic was received from a domain controller using port mirroring through an ATA Gateway|On the ATA Gateway capture NIC, disable these features in **Advanced Settings**:<br></br>Receive Segment Coalescing (IPv4)<br></br>Receive Segment Coalescing (IPv6)|
|This monitoring alert is displayed: **Some network traffic is not being analyzed**|If you have an ATA Gateway or Lightweight Gateway on VMware virtual machines, you might receive this monitoring alert. This happens because of a configuration mismatch in VMware.|Set the following settings to **0** or **Disabled** in the virtual machine NIC configuration: TsoEnable, LargeSendOffload, TSO Offload, Giant TSO Offload|TLS 1.0 is disabled on the ATA Gateway but .Net is set to use TLS 1.2|




## See Also
- [ATA prerequisites](ata-prerequisites.md)
- [ATA capacity planning](ata-capacity-planning.md)
- [Configure event collection](configure-event-collection.md)
- [Configuring Windows event forwarding](configure-event-collection.md#configuring-windows-event-forwarding)
- [Check out the ATA forum!](https://social.technet.microsoft.com/Forums/security/home?forum=mata)
