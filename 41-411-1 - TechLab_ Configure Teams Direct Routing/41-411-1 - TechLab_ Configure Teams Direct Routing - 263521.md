<!-- v3 -->

# Survey

## This lab is a technical deep dive on Teams Phone advanced features and calling plans. Before we begin, please take a moment to consider your experience and comfort level with this topic and answer these questions.

@lab.ActivityGroup(initialsurvey)

===

# **Lab 2: Deploying Direct Routing in Microsoft Teams** {:#lab1}

During this lab, you will configure the PSTN connectivity type Direct Routing in Microsoft Teams. You will configure and use an on-premises session border controller and then enable users for Teams Phone using this PSTN connectivity type.  
In this lab you will use an AudioCodes Session Border Controller for your lab environment but there are many more supported SBC vendors available.

>[!KNOWLEDGE] If you want to learn more, go to [**Plan Direct Routing - Microsoft Teams**](https://learn.microsoft.com/en-us/microsoftteams/direct-routing-plan "Plan Direct Routing - Microsoft Teams")  
>and  
>[**Session Border Controllers certified for Direct Routing - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/direct-routing-border-controllers "Session Border Controllers certified for Direct Routing - Microsoft Teams")

Estimated time to complete: *45 minutes*

#### **Before you begin**

To complete the exercises in this lab, you will be provided a Microsoft 365 tenant with an Enterprise E5 trial subscription.  

The type text ![TypeTextIcon.png](typetexticon.png "Type text icon") and copy/paste
![clipboard.png](clipboard.png "Clipboard icon") features used in this lab will send the specified text string to the active window in the virtual machine. Always compare the text in the lab document with the typed text in the virtual machine and virtual machine and verify the expected text is shown.

#### **What you will learn**

After completing the exercises, you will be able to:

- Pair an on-premises AudioCodes Mediant virtual edition session border controller (SBC) with Microsoft Teams.
- Configure a user for Teams Phone, voicemail, and assign a phone number.
- Test calling inbound and outbound using direct routing.

Select **Next** to continue to the lab exercises.  
*All credentials* are provided in the lab steps.

===

## **Exercise 1: Configuring the lab environment**

As the Teams administrator for your organization, you've started the planning and  deployment of the PSTN connectivity type Direct Routing. You completed all required planning steps and now you are ready to start the configuration for your environment.

First, you must configure the environment with a custom domain and request a trusted third-party public certificate that will be used by the SBC. Then you will deploy a configuration file and required certificates to the SBC in preparation for connecting to Microsoft Teams.

___

### Tasks


### 1. Retrieve your lab number

This task updates the O365Ready.com DNS server with your lab's public IP address and creates a DNS delegation zone for your lab domain pointing to the DNS server running on RRAS01. Requests for hosts in your lab domain will be resolved by the DNS server running in RRAS01.  

<details>
<summary>Expand this section if you have restarted this lab or if it expired *and* you are using the same lab number and Microsoft 365 tenant.</summary>
>If you have restarted this lab or if it expired and the virtual machines were reset *and* you are using the same lab number and Microsoft 365 tenant, perform the steps in the tip section at the end of this task. You do not need to be issued a new lab number. If you will be using a new lab number, continue with this task.  
</details>

![dnsprocessRRAS01.png](dnsprocessRRAS01.png)

1. [] Sign in to @lab.VirtualMachine(CLIENT01).SelectLink as **@lab.VirtualMachine(CLIENT01).Username** with +++@lab.VirtualMachine(CLIENT01).Password+++ as the password.

1. [] In Microsoft Edge, go to +++http://www.O365Ready.com+++.

1. [] On the Welcome page, select the **Generate Lab Number** tab.  

1. [] In the **IP Address** box, enter +++@lab.VirtualMachine(RRAS01).NetworkAdapter(Network Adapter).IpAddress+++.

    >[!ALERT] This is the public IP address assigned to the gateway server, RRAS01.  
    >RRAS01 is hosting the public DNS records for your custom lab domain name.  

1. [] In the **Lab Code** box, enter +++41228TEAMS+++ and then press Enter or select **Submit**.  
    This lab code will expire 90 days after the start of this course.

1. [] In the results, locate and write down the five-digit lab number that is assigned to you. You will refer to this five-digit number throughout the labs.  
    You will be using all five digits as part of your organization's on-premises domain.

1. [] Enter your five-digit lab number in the following text box. This will automatically populate your lab number in the lab document:  
    @lab.TextBox(customlabnumber)

1. [] Select the following button to create the lab configurations and files that will be used later:

    @lab.Activity(labartifacts)

    >[!KNOWLEDGE]
    <details>
    <summary>**<u>Expand this section if</u>** you are using the same lab number and have restarted this lab or if the lab timer has expired and the virtual machines were reset. You will likely have a new public IP address. Perform the following steps to update your lab domain delegation zone's public IP address.</summary> 
    >- Open Windows PowerShell and then run the following command. Replace the five x's with your lab number.
    >
    >   ```PowerShell-wrap
    >   Resolve-DnsName labxxxxx.o365ready.com -type NS -server o365ready.com
    >   ```
    >
    >- Write down the identified public IP. This should be the public IP address you previously used.
    >- In Microsoft Edge, go to +++http://www.O365Ready.com+++.  
    >- On the Welcome page, select the **Update Public IP Address** tab.  
    >- In the **Student Lab Number** box, type your five digit lab number.  
    >   If you did not write down your original lab number, you can find it by signing into Microsoft 365 and browsing to the **Domains** feature.  
    >- In the **Old public IP address** box, type the previously used public IP address.  
    >- In the **New public IP address** box, type +++@lab.VirtualMachine(RRAS01).NetworkAdapter(Network Adapter).IpAddress+++ and then press Enter.  
    >- Select **Submit** and wait for the update to complete. This may take a couple of minutes.
    </details>

===

### 2. Add your custom lab domain to Azure Active Directory

1. [] In CLIENT01, right-click **Start** and then select **Terminal (Admin)**.  

1. [] In the **User Account Control** dialog box, select **Yes**.  


1. [] Connect to Azure AD. In Windows PowerShell, enter the following and then press Enter:  
    
    ``` PowerShell-wrap
    Connect-MgGraph -Scopes "User.ReadWrite.All","Domain.ReadWrite.All"
     
    ```
      |   |   |
    |---|---|
    | **User name** | +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ |
    | **Password** | +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ |

    If prompted for to change password enter:

    - Current password: +++@lab.CloudCredential(M365Calling).AdministrativePassword+++

    - New Password: +++@lab.CloudCredential(M365Calling).UserPassword+++

1. [] In the dialog box select consent on behalf of my organization and the select **Accept**
  

1. [] Add your lab domain to Azure. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    New-MgDomain -BodyParameter @{Id="lab@lab.Variable(customlabnumber).o365ready.com";IsDefault="False"}
    ```  
1. [] Assign the verification text record to a variable. In Windows PowerShell, enter the following and then press Enter:
    ```PowerShell-wrap
    $verifyTXT = (Get-MgDomainVerificationDnsRecord -DomainId  "lab@lab.Variable(customlabnumber).o365ready.com" | Where-Object {$_.RecordType -eq "Txt"}).AdditionalProperties.text
    ``` 
Confirm by entering:
    ```PowerShell-wrap
    $verifyTXT
    ``` 

1. [] The RRAS01 server is hosting the DNS zone used by your custom domain. Rather than connecting to the remote server's console, you can do this remotely using Windows PowerShell. Create a CIM session to the DNS server named RRAS01. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    $Cimsession = New-CimSession -Name "RRAS01" -ComputerName "RRAS01" -Credential $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList ("@lab.VirtualMachine(RRAS01).Username", (ConvertTo-SecureString -AsPlainText -Force -String "@lab.VirtualMachine(RRAS01).Password"))) -Authentication Negotiate
    ```
1. [] Using the remote session, add the verification text record to the DNS server. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Add-DnsServerResourceRecord -ComputerName "RRAS01" -CimSession $Cimsession -ZoneName "@lab.Variable(labDomain)" -Name "@" -Txt -DescriptiveText $verifyTXT
    ```

1. [] Complete domain verification in Azure AD by confirming the value you have in your DNS zone with the expected value from Microsoft 365. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Confirm-MgDomain -DomainId "lab@lab.Variable(customlabnumber).o365ready.com"
    ```

1. [] For Direct Routing, you cannot use the default domain &ast;.onmicrosoft.com. You must use one or more domains added to the organization. Set the lab domain name as the default domain in Azure AD. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Update-MgDomain -DomainId "lab@lab.Variable(customlabnumber).o365ready.com" -IsDefault:$true
    ```

    There is no output for this command.

@lab.Activity(Automated2)

>[!ALERT] If your domain is not able to be verified, confirm all configurations were successful in the previous task. If necessary, review the [**Add your custom domain name using the Azure Active Directory portal**](https://learn.microsoft.com/azure/active-directory/fundamentals/add-custom-domain) Microsoft Learn document.

>[!KNOWLEDGE] To learn more about why a DNS Record is required for the SBC, go to [**Plan Direct Routing - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/direct-routing-plan#sbc-domain-names "Plan Direct Routing - Microsoft Teams")  
>and
>To learn more about the Verification TXT Records, go to [**Add DNS records to connect your domain - Microsoft 365 admin**](https://learn.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider?view=o365-worldwide#recommended-verify-with-a-txt-record "Add DNS records to connect your domain - Microsoft 365 admin")

===

### 3. Complete the domain setup in the Microsoft 365 admin center

1. [] In CLIENT01, open Microsoft Edge and then go to +++https://admin.microsoft.com+++.  

1. [] Sign in as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ using +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ as the password.

1. [] In the Microsoft 365 admin center, in the left navigation, select **Show all**.

1. [] Select **Settings** > **Domains**.

1. [] On the Domains page, select **lab@lab.Variable(showcustomlabnumber).o365ready.com**.

1. [] On the lab@lab.Variable(showcustomlabnumber).o365ready.com page, select **Continue setup**.

1. [] On the How do you want to connect your domain page, select **Continue**.

    >[!NOTE] The lab's DNS records have already been created for you in the publicly facing DNS server on RRAS01.

1. [] On the Add DNS records, select **Continue**.  
    You may choose to review the different DNS record option requirements.

1. [] On the Domain setup is complete page, select **Done**.

1. [] In the left navigation, select **Domains**.

1. [] On the Domains page, verify the **Status** for the lab@lab.Variable(showcustomlabnumber).o365ready.com is listed as **Healthy**.

===

### 4. Request your public certificate from DigiCert

An SBC needs to have a valid public certificate to build a trusted connection to Microsoft Teams and to encrypt the traffic. Therefore, we are going to request such a certificate from a valid public CA (Certification Agency) and install it locally on the SBC in the lab.

1. [] Open File Explorer and then go to **C:\\LabFiles**.

1. [] Double-click or double-tap **CertReq-lab@lab.Variable(customlabnumber).o365Ready.com.txt**.  
    This certificate request was created by the configuration script.

1. [] In Notepad, select all text in the file and then press Ctrl+C to copy the contents to the clipboard.

1. [] Switch to Microsoft Edge and then go to +++https://www.digicert.com/friends/exchange.php+++.

1. [] On the Microsoft Event CSR Submission page, in the **Paste CSR** box, right-click and then select **Paste**.

1. [] Under **Certificate Details**, review the common name and subject alternative names (SAN) information that will be assigned to the certificate.  

1. [] Under Certificate Delivery, in the **Email Address** and **Email Address (again)** boxes, enter +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++.

1. [] Select the **I agree to the Terms of Service above** checkbox.

1. [] Select **Submit**.

1. [] Close Notepad and File Explorer.

===

# **Exercise 2: Configuring the session border controller**

In this exercise, you will verify your custom domain exists in Microsoft 365 and is assigned to a user. Then, you will upload a new configuration file and update an AudioCodes Mediant virtual edition SBC.  

___
## Tasks

___
<!-- 
### 1. Verify the custom domain has been added to your Microsoft 365 subscription

1. [] On @lab.VirtualMachine(CLIENT01).SelectLink, in Microsoft Edge, go to +++https://admin.microsoft.com+++.

1. [] Sign in as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with password +++@lab.CloudCredential(M365Calling).AdministrativePassword+++.

1. [] In the left navigation, select **Show all**.

1. [] Select **Settings** > **Domains**.

1. [] Verify your custom **lab@lab.Variable(customlabnumber).O365Ready.com** domain has been added to Microsoft 365 and is set as Default.  
     The domain may still be listed as **Incomplete setup**. This will not cause problems in the lab. This indicates not all domain DNS records have been verified.
 -->


### 1. Assign the custom lab domain to Diego Siciliani

1. [] On @lab.VirtualMachine(CLIENT01).SelectLink, in Microsoft Edge, go to +++https://admin.microsoft.com+++.

1. [] Sign in as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ as the password.

1. [] In the left navigation, select **Users** > **Active users**.

1. [] In the **Active users** list, select **Diego Siciliani**.

1. [] In the Diego Siciliani user card, under **Username and email** select **Manage username and email**.

1. [] In the **Manage username and email** pane, to the right of **DiegoS@@lab.CloudCredential(M365Calling).TenantName**, select the **Edit** pencil icon.

1. [] To the right of **Diego**, select the menu and then select **lab@lab.Variable(customlabnumber).o365Ready.com**.

1. [] Select **Done** and then select **Save changes**.

1. [] Close the **Manage username and email** pane.

    >[!NOTE] Your custom domain must be assigned to at least one Teams enabled user to ensure the domain is available for the SBC configured later in this lab.

1. [] Perform the same steps to update the username and email address for **Patti Fernandez** from PattiF@@lab.CloudCredential(M365Calling).TenantName to **PattiF@lab@lab.Variable(customlabnumber).O365Ready.com**.

===

### 2. Download your public certificate file 

1. [] In Microsoft Edge, open a new tab and then go to +++https://outlook.office365.com+++.  
    If necessary, sign to in Outlook on the web as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with password +++@lab.CloudCredential(M365Calling).AdministrativePassword+++.

1. [] In the message list, locate and select the message from **DigiCert** with the zip file attachment.  
    The message may arrive in the Focused or Other folder.

    >[!ALERT] If the message is not in the Inbox, wait 2-5 minutes. Refresh the browser if the message list is not automatically updated. If the message has not been delivered, continue to the next lab and return later. In some cases, processing and issuance of the certificate may take longer than expected.

1. [] Download the **sbc01_Lab@lab.Variable(customlabnumber).O365Ready.com**XXXXXXX.**zip** file.  

    >[!ALERT] Do not open or extract the zip file. You will do next this using PowerShell.

1. [] Right-click **Start** and then select **Terminal (Admin)**.  

1. [] In the **User Account Control** dialog box, select **Yes**.  

1. [] Verify the zip file is located in the Downloads folder. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Get-ChildItem "C:\Users\$env:Username\Downloads" | where{ $_.Name -like "sbc01*.zip" }
    ```

1. [] Review the output and verify the file is listed. If it is not, return to Outlook on the web and download the file again.

===

### 3. Import the certificate and export the file to PFX

1. [] Create a variable to hold the certificate zip file. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    $CertZipFileName = (Get-ChildItem "C:\Users\\$env:Username\Downloads" | where{ $_.Name -like "sbc01*.zip" }).Name
    ```

1. [] Expand the archive to the C:\\Script folder. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Expand-Archive -LiteralPath "C:\Users\$env:Username\Downloads\$CertZipFileName" -DestinationPath "C:\Scripts" -Force
    ```

1. [] Complete the certificate request that was created at the beginning of this lab. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    certreq.exe -accept (Get-ChildItem C:\Scripts\$($CertZipFileName.Split(".")[0]) | Where-Object { $_.Name -like "sbc01_lab@lab.Variable(customlabnumber)_o365ready_com*" }).VersionInfo.FileName
    ```

    >[!NOTE] The certificate request was created when you ran the **Update lab configurations and files** automation activity.

1. [] Review the output of the command and verify the certificate has been installed.

1. [] Retrieve the certificate thumbprint. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    $cert = (Get-ChildItem Cert:\LocalMachine\My | Where-Object { $_.Subject -like "CN=sbc01.@lab.Variable(labDomain)*" }).Thumbprint
    ```

1. [] Create a secure password string. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    $securePassword = "@lab.VirtualMachine(CLIENT01).Password" | ConvertTo-SecureString -AsPlainText -Force
    ```

1. [] Export the certificate to a PFX file so it can be used later. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Export-PfxCertificate -Cert Cert:\LocalMachine\My\$cert -FilePath C:\LabFiles\Labcert.pfx -ChainOption BuildChain -Password $securePassword
    ```

===

### 4. Upload the Session Border Controller (SBC) configuration

>[!NOTE] The ini file used in this task is the configuration file for the SBC and includes all the required configuration details for the SBC like IPs, Connection to Teams, Ports, Media, Security etc.  
>This lab is not intended to teach how to configure an SBC. The focus is to understand the configuration steps and requirements for a Direct Routing.


1. [] In Microsoft Edge, open a new tab and then go to +++http://192.168.0.200+++.  
    This is the address of the AudioCodes SBC.

1. [] On the Web Login page, sign in as +++Admin+++ with password +++Admin+++.  

1. [] In the top navigation menu, select **ADMINISTRATION**.

1. [] In the left navigation, select **MAINTENANCE > Configuration File**.

1. [] In the Configuration File window, under **INI FILE**, under **Load INI file to the device**, select **Choose File**.  

1. [] go to **C:\\LabFiles**, select **Lab@lab.Variable(customlabnumber)-SBC01-Config.ini**, and then select **Open**.  

1. [] To the right of the configuration file path, select **Load INI file**.  

1. [] Review the dialog box and then select **OK**.  
    Wait for the SBC to restart. When finished restarting, the browser will refresh to the sign in page.

===

### 5. Upload root certificates to the SBC

The SBC must trust the full certificate chain Microsoft uses for the Direct Routing infrastructure to allow a TLS connection between the SBC and Microsoft.

1. [] In Microsoft Edge, on the Web Login page, sign in as +++Admin+++ with password +++Admin+++.  
    If necessary, refresh the browser page. You may need to wait for the SBC to finish restarting.

1. [] On the top menu, select **IP NETWORK**.  

1. [] In the left navigation, select **SECURITY > TLS Contexts**.  

1. [] In the results pane, in the **TLS Contexts** table, select **Teams-TLSContext**.  

1. [] Under **#1[Teams-TLSContext]**, at the bottom of the page, select **Trusted Root Certificate >>**.  

1. [] In the Trusted Root Certificates window, select **Import**.  

1. [] In the **Import New Certificate** dialog box, select **Choose File**.

1. [] Go to **C:\\LabFiles**, select **BaltimoreTrustedRootCA.cer**, and then select **Open**.

1. [] In the **Import New Certificate** dialog box, select **Ok**.  

1. [] Review the information for the selected root certificate that was installed.

1. [] At the top of the page, select **Import**.

1. [] Perform the same steps and load the following root certificates:  
    - **DigicertTrustedRoot.cer**
    - **DigicertTrustedRootIntermediate.cer**

1. [] When complete, in the Trusted Root Certificates window, to the left of **TLS Context[#1]**, select the back arrow icon.

===

### 6. Upload the lab certificate to the SBC

The specific certificate for the SBC must be imported to ensure the device connection is valid.

1. [] In the TLS Contexts window, in the **TLS Contexts** table, select **Teams-TLSContext**.  

1. [] Under **#1[Teams-TLSContext]**, at the bottom of the page, select **Change Certificate >>**.  

1. [] In the Change Certificates window, scroll down to the **UPLOAD CERTIFICATE FILES FROM YOUR COMPUTER** section.  

1. [] In the **Private key pass-phrase (*optional*)** box, enter +++@lab.VirtualMachine(CLIENT01).Password+++.

1. [] Under **Send Private Key file from your computer to the device**, select **Choose File**.  

1. [] In the Open window, go to **C:\\LabFiles**, select **Labcert.pfx**, and then select **Open**.  

1. [] To the right of the lab certificate file path, select **Load File**.  

1. [] Review the message below the **Change Certificates** page name and verify that the certificate was loaded.

===

### 7. Update the outbound manipulation set

A message manipulation rule defines a manipulation sequence for SIP messages. SIP message manipulation enables the normalization of SIP messaging fields between communicating network segments.

1. [] In Microsoft Edge, go to +++http://192.168.0.200/AdminPage+++.

1. [] In the left navigation, select ***ini* Parameters**.  

1. [] In the **Parameter Name** box, enter +++GWOutboundManipulationSet+++.

1. [] In the **Enter Value** box, enter +++0+++.  
    This is the manipulation set ID value.

1. [] Select **Apply New Value**.  

1. [] Review the information in the **Output Window**.  

1. [] In the left navigation, select **Back to Main**.  

1. [] In the top navigation menu, select **Save**.  

1. [] In the **Save Configuration** dialog box, select **Yes**.  

===

# **Exercise 3: Deploying Direct Routing in Microsoft 365**

In this exercise, you will learn how to connect  the on-premises SBC to Microsoft Teams Direct Routing and how to configure Teams users to use Direct Routing to connect to the Public Switched Telephone Network (PSTN). You will test calling from Microsoft Teams to a SIP software-based phone and from the softphone to Microsoft Teams.

>[!KNOWLEDGE] If you want to learn more, go to  [**Configure Direct Routing - Microsoft Teams**](https://learn.microsoft.com/MicrosoftTeams/direct-routing-configure "Configure Direct Routing - Microsoft Teams")

## Tasks

___
### 1. Verify DNS records and test connectivity
___

1. [] Perform a lookup on the SBC host name and verify the name resolves to a public IP address. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Resolve-DnsName sbc01.@lab.Variable(labDomain)
    ```

1. [] The lab virtual environment cannot be used to telnet to the public IP address of the SBC.  
    Telnet to the SBC using the public DNS name and signaling port of the SBC. Using your own computer, in Windows PowerShell, enter the following and then press Enter:
    
    ++Telnet sbc01.@lab.Variable(labDomain) 5061++

    >[!HINT] If you don't have telnet already installed, on Windows you can install telnet using the following command:
    >
    >```PowerShell-wrap
    >Enable-WindowsOptionalFeature -Online -FeatureName TelnetClient
    >```

1. [] A successful connection will result in a blank console screen.  
    Type any letters into the console session to cause it to close or wait for the session to close automatically.

1. [] Close Windows PowerShell.

===

### 2. Connect SBC to Microsoft Teams Direct Routing

1. [] In CLIENT01, right-click **Start** and then select **Windows Terminal (Admin)**.  

1. [] In the **User Account Control** dialog box, select **Yes**.  

1. [] Connect to Microsoft Teams using the Teams PowerShell module. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Connect-MicrosoftTeams
    ```  

1. [] In the Windows PowerShell credential request window, enter the following information and then select **OK**:

    |   |   |
    |---|---|
    | **Username** | +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ |
    | **Password** | +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ |


1. [] Review the PSTN gateway commands. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Get-Command *PSTNGateway*
    ```

1. [] Review the available cmdlets.

1. [] Create an SBC (Online PSTN Gateway) in Microsoft Teams. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    New-CsOnlinePSTNGateway -Fqdn sbc01.lab@lab.Variable(customlabnumber).o365ready.com -SipSignalingPort 5061 -MaxConcurrentSessions 100 -Enabled $true
    ```

    >[!ALERT] If you receive an error stating:  
    >**Can not use the "sbc01.lab@lab.Variable(customlabnumber).o365ready.com" domain as it was not configured for this tenant.**  
    >You may have to wait up to 15 minutes until the newly added custom domain is available for use. Periodically retry the command until it is successful.

1. [] Review the output of the command.

@lab.Activity(Automated3)

    >[!HINT] We highly recommend setting the SBC Maximum Concurrent Sessions which can be found in the SBC documentation. The limit will help to be notified if the SBC is at the capacity level.

>[!KNOWLEDGE] If you want to learn more, go to  [**Connect your Session Border Controller &#40;SBC&#41; to Direct Routing**](https://learn.microsoft.com/MicrosoftTeams/direct-routing-connect-the-sbc "Connect your Session Border Controller &#40;SBC&#41; to Direct Routing")

===

### 3. Validate the pairing

<!-- 1. [] Retrieve the PSTN gateway information. In Windows PowerShell, enter the following and then press Enter:  

        ```PowerShell-wrap
        Get-CSOnlinePSTNGateway
        ```

    1. [] Review the output of the command and verify that the new PSTN gateway is listed. 
-->

1. [] Switch to Microsoft Edge and then go to +++https://admin.teams.microsoft.com+++.  
    If necessary, sign to in Outlook on the web as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with password +++@lab.CloudCredential(M365Calling).AdministrativePassword+++.

1. [] In the left navigation, select **Voice** > **Direct Routing**.

1. [] On the **SBCs** tab verify that **sbc01.lab@lab.Variable(customlabnumber).o365ready.com** is listed.

    >[!HINT] The SBC may show warnings. The TLS connectivity status warning is related to the certificate expiring within 30 days. The SIP Options status warning should be transient and will not impact this lab.

1. [] Select **sbc01.lab@lab.Variable(customlabnumber).o365ready.com**.

1. [] Although there is no current call information, on the **Usage** tab, review the different graphs that are available.

1. [] Select the **Settings** tab.

1. [] Review the different settings reported for the SBC.

@lab.Activity(Question10)
===

### 4. Enable debug logging in the SBC

1. [] Switch to Microsoft Edge and the AudioCodes tab.  
    If necessary, sign in as +++Admin+++ with password +++Admin+++.

1. [] On the audiocodes page, at the top, select **TROUBLESHOOT**.

1. [] In the left navigation, under **MESSAGE LOG** > **LOGGING**, select **Syslog Settings**.

1. [] On the Syslog Settings page, under **SYSLOG**, verify **Enable Syslog** is set to **Enable**.

1. [] In the **Syslog Server IP** box, enter +++192.168.0.101+++.  
    This is the IP address of CLIENT01.

1. [] Select the **Debug Level** menu and then select **Basic**.

1. [] At the bottom of the page, select **APPLY**.

1. [] At the top of the page, select **Save**.

1. [] In the **Save Configuration** dialog box, select **Yes**.

===

### 5. Review SIP OPTIONS in Syslog Viewer and confirm successful two-way communications

1. [] On CLIENT01, select **Start** and then select **Syslog Viewer**.

    >[!NOTE] If prompted, you do not need to update the application.

1. [] Maximize the Syslog Viewer window.

1. [] Wait for logs to begin displaying in the window.

1. [] In the menu, select **Tools** > **SIP Flow Diagram**.

1. [] In the SIP Flow Diagram window, in the table, select one of the flows from the SBC.

1. [] Under the table, in the SIP stack, locate and review the **CONTACT** entry and confirm this header contains the SBC fully qualified domain name (FQDN) instead of an IP address.

1. [] In the visualization on the left, verify receipt of the **200** OK option.

    !IMAGE[Syslog viewer showing the SIP Flow Diagram connection from 192.168.0.200 to 192.168.0.200](sipoptionssbctoms2.png "Syslog viewer showing the SIP Flow Diagram connection from 192.168.0.200 to 192.168.0.200"){700}

    !IMAGE[Syslog viewer showing the SIP Flow Diagram connection from Device to the IP address 52.114.132.46](microsoftdirectroutingendpoint.png "Syslog viewer showing the SIP Flow Diagram connection from Device to the IP address 52.114.132.46"){300}

1. [] In the table, select one of the flows from **sip:sip-du-a-us.pstnhub.microsoft.com**, **sip:sip-du-a-eu.pstnhub.microsoft.com**, or **sip:sip-du-a-as.pstnhub.microsoft.com** to **192.168.0.200**.

    >[!HINT] These are the connection points Microsoft uses for Direct Routing within the documented IP ranges of 52.112.0.0/14 and 52.120.0.0/14.

1. [] In the visualization on the left, verify receipt of the **200** OK option.

    !IMAGE[Syslog viewer showing the SIP Flow Diagram connection from sip:sip-du-a-us.pstnhub.microsoft.com to 192.168.0.200](sipoptionmstosbc.png "Syslog viewer showing the SIP Flow Diagram connection from sip:sip-du-a-us.pstnhub.microsoft.com to 192.168.0.200")

Leave the SIP Flow Diagram window open. It will be used later.

===

### 6. Create a voice route with one PSTN usage

1. [] Return the current PSTN usage records in the tenant. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Get-CsOnlinePstnUsage
    ```

1. [] Review the output of the command.

    >[!KNOWLEDGE] If you have several usages defined, the names of the usages might truncate. Use the command, (Get-CSOnlinePSTNUsage).Usage, to display a list of the defined PSTN usages.

1. [] Add a usage to the list of online PSTN usages. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Set-CsOnlinePstnUsage -Identity Global -Usage @{Add="US and Canada"}
    ```

1. [] Create a new online voice route to tell Microsoft Teams how to route calls from Microsoft 365 users to phone numbers on the public switched telephone network (PSTN) or a private branch exchange (PBX). In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    New-CsOnlineVoiceRoute -Identity "10 Digit Dialing" -NumberPattern "^\+1(\d{10})$" -OnlinePstnGatewayList sbc01.lab@lab.Variable(customlabnumber).o365ready.com -OnlinePstnUsages "US and Canada"
    ```

===

### 7. Verify SBC Connectivity using the SBC Health Dashboard in the Teams admin center

1. [] Switch to Microsoft Edge and the Microsoft Teams admin center.

1. [] In the left navigation, select **Voice** > **Direct Routing**.

1. [] On the **SBCs** tab, for **sbc01.lab@lab.Variable(customlabnumber).o365ready.com**, check the different status fields.  
    You may need to refresh the screen to see updated information.

1. [] Confirm the **SIP Options status** is listed as **Active**.

1. [] Select the **Voice routes** tab.

1. [] Review the Voice routes and verify **sbc01.lab@lab.Variable(customlabnumber).o365ready.com** is listed in the **SBCs enrolled** column for the recently created voice route.

>[!KNOWLEDGE] If you want to learn more, go to  [**Health Dashboard for Direct Routing - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/direct-routing-health-dashboard "Health Dashboard for Direct Routing - Microsoft Teams")

===

### 8. Configure user phone numbers, Teams Phone, and phone number type

1. [] Switch to Windows Terminal.

1. [] Set Diego Siciliani's phone number and phone number type. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Set-CsPhoneNumberAssignment -Identity DiegoS@lab@lab.Variable(customlabnumber).o365ready.com -PhoneNumber +14255551234 -PhoneNumberType DirectRouting
    ```

1. [] Set Patti Fernandez's phone number and phone number type. In Windows PowerShell, enter the following and then press Enter:

    ```PowerShell-wrap
    Set-CsPhoneNumberAssignment -Identity PattiF@lab@lab.Variable(customlabnumber).o365ready.com -PhoneNumber +14255551235 -PhoneNumberType DirectRouting
    ```  

1. [] In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Get-CsOnlineUser -Filter "DisplayName eq 'Diego Siciliani' or DisplayName eq 'Patti Fernandez'" |fl Userprin*,DisplayN*,Enter*,LineU*
    ```

    >[!ALERT] If there is no output for the command, wait 2-3 minutes and then retry the command.

1. [] Review the information and notice that Teams Phone is enabled.

===

### 9. Configure and assign a voice routing policy

Microsoft Phone System has a routing mechanism that allows a call to be sent to a specific Session Border Controller (SBC) based on:
- The called number pattern.
- The called number pattern plus the specific user who makes the call.

1. [] Create a new online voice routing policy. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    New-CsOnlineVoiceRoutingPolicy "North America" -OnlinePstnUsages "US and Canada"
    ```

1. [] Assign the policy to Diego Siciliani. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Grant-CsOnlineVoiceRoutingPolicy -Identity DiegoS@lab@lab.Variable(customlabnumber).o365ready.com -PolicyName "North America"
    ```  

    >[!ALERT] If you receive an error stating that the **Policy "North America" is not a user policy. You can assign only a user policy to a specific user**, wait 2-3 minutes and then retry the command. You may need to retry the command several times before it is successful. It may take up to 15 minutes before it becomes available. If the policy is still not updated in the service, you continue to the next lab and return later. 

1. [] Assign the policy to Patti Fernandez. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Grant-CsOnlineVoiceRoutingPolicy -Identity PattiF@lab@lab.Variable(customlabnumber).o365ready.com -PolicyName "North America"
    ```  

1. [] Confirm the voice routing policy has been assigned to Diego Siciliani and Patti Fernandez. In Windows PowerShell, enter the following and then press Enter:  

    ```PowerShell-wrap
    Get-CsOnlineUser -Filter "DisplayName eq 'Diego Siciliani' or DisplayName eq 'Patti Fernandez'" |ft Userprin*,OnlineVoiceR*
    ```

    >[!ALERT] If there is no output for the command, wait 2-3 minutes and then retry the command.

1. [] Review the output of the command.  
    If the policy is empty, try the command again. If the policy is still empty, wait approximately 2-3 minutes and then try the command again.

>[!KNOWLEDGE] If you want to learn more, go to  [**Configure call routing for Direct Routing - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/direct-routing-voice-routing#call-routing-overview "Configure call routing for Direct Routing - Microsoft Teams")

@lab.Activity(Question11)

===

### 10. Open the 3CX softphone

The 3CX softphone is a software-based IP phone and is being used to represent calls made from a user connecting through the SBC.

1. [] Select **Start** and then select **3CX Phone** > **3CX Phone**.  

1. [] Verify that **On Hook** is displayed on the screen.  

    >[!NOTE] The softphone has already been configured to connect to the AudioCodes Mediant VE SBC.

===

### 11. Sign in to the Microsoft Teams desktop app and test calling from a Direct Routing user

1. [] Verify Syslog Viewer and SIP Flow Diagram are running or open **Syslog Viewer** and the **SIP Flow Diagram**. Logs will be reviewed in later tasks.

1. [] On the **Windows task bar**, select **Microsoft Teams**.

    >[!ALERT] The currently installed version of Microsoft Teams may require updating to be current with Microsoft requirements for connectivity. If this happens, in Microsoft Edge, go to +++https://www.microsoft.com/en-us/microsoft-teams/download-app+++ and download the desktop Teams app for Teams for work or school.

1. [] In the Microsoft Teams window, select **Get started**.

1. [] In the **Sign in** box, enter +++DiegoS@lab@lab.Variable(customlabnumber).o365ready.com+++ and then select **Next**.

1. [] In the **Password** box, enter +++@lab.CloudCredential(M365Calling).UserPassword+++ and then select **Sign in**.

     If prompted for to change password enter:

    - Current password: +++@lab.CloudCredential(M365Calling).UserPassword+++

    - New Password: +++@lab.CloudCredential(M365Calling).AdministrativePassword+++

1. [] In the Stay signed in to all your apps window, select **No, sign in to this app only**.

1. [] Close the introduction wizard and any other notifications.

1. [] In the app bar on the left, select **Calls**.

    >[!ALERT] If the **Calls** option is not available, sign out of Microsoft Teams, wait 2-3 minutes, and then sign in again. You may need to repeat this if the option is not available after your first retry.

1. [] Using the dial pad, call +++12065559876+++.  
    This is the number for the account configured in the 3CX softphone.  

    >[!ALERT] If the dial pad is not available, continue to the next task and test calling from the softphone. It may take several minutes before the feature is available.

1. [] If prompted, in the **Windows Security Alert** dialog box, select **Allow access**.

1. [] Switch to the 3CX softphone.

1. [] In the incoming call notification, select **Answer** and verify the call is connected.

    >[!ALERT] If the call fails to complete, you may need to wait 1-2 minutes and then try again. 

1. [] When complete, in the 3CX window, select the **hang up** icon.

===

### 12. Test calling to a Direct Routing user

1. [] In the softphone, using the dial pad, call +++14255551234+++.

1. [] Answer the call using the Teams client and verify the call has connected.

1. [] Switch to the softphone and hang up the call.  

1. [] From the softphone, using the dial pad, call +++14255551235+++.

1. [] Verify that you can connect to Patti's voicemail and then hang up.  

    >[!ALERT] If you are unable to hear the voicemail message, hang up and continue with the lab.

===

### 13. Review calls using CDR History and Syslog Viewer (SIP stack)

1. [] Switch to Microsoft Edge and the AudioCodes tab.  
    If necessary, sign in as +++Admin+++ with password +++Admin+++.

1. [] On the audiocodes page, at the top, select **MONITOR**.

1. [] In the left navigation, select **VOIP STATUS** > **SBC CDR HISTORY**.

1. [] In the table, review the incoming and outgoing calls.  
    Locate the calls from the Microsoft Teams user to the softphone.

1. [] Switch to the AudioCodes Syslog Viewer app and the SIP Flows Diagram window.

1. [] Under the table, on the menu, select **Refresh**.

1. [] In the table, scroll to the bottom of the captured connections and move back up to locate and then select the call from **sip:+14255551234@sip.pstnhub.microsoft.com** to **sip:+12065559876@sbc01.lab69392.o365ready.com**.

1. [] Under the table, review the SIP stack information.  
    These are the entries from **INVITE** to **SESSION-EXPIRES**.

1. [] Below the SIP stack, review the Session Description Protocol (SDP) content.

1. [] In the visualization on the left, review the call from **INVITE** through the final **200** OK to end the session.

    !IMAGE[SIP Flow diagram showing a successful call from a Teams user through the SBC to a recipient](sipflowdiagramofsuccessfulcall.png "SIP Flow diagram showing a successful call from a Teams user through the SBC to a recipient")

>[!KNOWLEDGE] If you want to learn more, go to [**Teams Phone System Direct Routing: SIP protocol - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/direct-routing-protocols-sip "Teams Phone System Direct Routing: SIP protocol - Microsoft Teams").

===
# Survey

## Please take a moment to answer these questions. Providing answers to these questions will complete this lab. Your answers will help us understand the effectiveness of the lab and help to improve these labs as a whole.

@lab.ActivityGroup(completionsurvey)

===
# Congratulations!

You've completed the **Deploying Direct Routing in Microsoft Teams** lab.
