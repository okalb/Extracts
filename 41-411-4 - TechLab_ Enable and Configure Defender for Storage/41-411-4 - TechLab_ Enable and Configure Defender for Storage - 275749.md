# Survey

## This lab is a technical deep dive on **Microsoft Defender for Storage**. Before we begin, please take a moment to consider your experience and comfort level with this topic and answer these questions.

@lab.ActivityGroup(initialsurvey)

===

# **Lab: Defender for Storage Configuration**

During this lab, you'll be come familiar with Microsoft Defender for Storage.

Estimated time to complete: *120 minutes*

#### **Before you begin**

To complete the exercises in this lab, you'll be  provided a Microsoft 365 Business Premium tenant.  

The type text icon !IMAGE[Send text](instructions262909/TT-image.png "Send text") feature used in this lab will send the specified text string to the active window in the virtual machine. Always compare the text in the lab document with the typed text in the virtual machine and verify that the expected text displays.

#### **What you will learn**

After completing the exercises, you'll be able to:

- Prepare the environment for Microsoft Defender for Storage 
- Test Defender for Cloud for Storage
- Configure automation to delete malicious files
- Configure log analytics
- Implement Azure Based Access Control (ABAC)

 
## **Scenario**

You are administering Contoso's 

<br>

***<u>User names and passwords will be provided in the instructions on the following pages</u>***.  

In the lower right of the lab interface, select **Next** to continue to the first exercise.

===

# **Exercise 1: Prepare the environment for Defender for Storage** 

In this exercise, you will go through the basic steps for enabling Microsoft Defender for Storage.

Estimated time to complete: *15 minutes*

#### **What you will learn**

After completing the tasks in this exercise, you'll be  able to:

- Enable the Defender for Storage plan
- Create a storage account
- Exclude a folder in Windows Security

===

### Task 1 - Enable and upgrade the subscription 

___

1. [] Sign in to the @lab.VirtualMachine(W-11CL-1).SelectLink as **LabAdmin** using +++@lab.VirtualMachine(W-11CL-1).Password+++ as the password.

1. [] On W-11 CL-1, start Microsoft Edge and go to the Azure Portal at +++**https://portal.azure.com**+++.

1. [] Sign in to the Azure admin portal.
    - Microsoft 365 credentials:
        - Username: +++@lab.CloudPortalCredential(User1).Username+++
        - Password: +++@lab.CloudPortalCredential(User1).Password+++

1. [] On the Welcome to Microsoft Azure page, select **Get Started** and then on the next two pages, select **Skip**.

1. [] In the Microsoft Azure portal, search for and select **+++Microsoft Defender for Cloud+++**.

1. [] On the Microsoft Defender for Cloud | Getting Started blade, under **Enable Defender for Cloud on 1 subscriptions**, verify the checkbox is selected for your lab subscription and then select **Upgrade**.

    >[!NOTE] You will need to wait for several minutes for the upgrade to complete.

1. [] Do not continue until the upgrade has finished.

===

### Task 2 - Verify Defender for Storage is enabled

___

1. [] In Microsoft Defender for Cloud, on the navigation pane, under **Management**, select **Environment settings**. 

1. [] On the Environment settings blade, select **Expand All**.

1. [] In the tree of expanded items, select your subscription to open the **Defender Plans** blade.

1. [] Under **Cloud Workload Protection (CWP)**, verify **Storage** shows as **On**.

1. [] In the Monitoring coverage column for Storage, select **Settings**.

1. [] On the Settings & monitoring blade, verify **Malware scanning** is set to **On** and Limit is set to 5000 GB.

1. [] Verify **Sensitive data discovery** is set to **On**, and then select the **X** in the upper-right corner to close Settings & monitoring.

===

### Task 3 - Create a storage account

___

1. [] In the Microsoft Azure portal, search for and select **+++Storage accounts+++**.

1. [] On the Storage accounts blade, select **Create storage account**.

1. [] On the Create a storage account blade, on the Basics tab, in the **Subscription** drop-down menu verify it shows your lab  subscription.

1. [] In the **Resource group** field, leave it as **RG1**.

1. [] In the **Storage account name** field, type **+++sa@lab.LabInstance.Id+++**, leave the rest of the defaults and then select **Next**.

    >[!NOTE] The Storage account name field must be globally unique and contain only lowercase letters and numbers.

    <!--WK: Commented to allow for a unique Storage account name that can be used in validation scripts

    1. [] In the **Storage account name** field, enter a globally unique name and make a note of it, leave the rest of the defaults and then select **Next**.

        >[!NOTE] You can only use lowercase letters and numbers. Make a note of the name you select for later parts of this lab.

    -->

1. [] On the **Advanced** tab, review the default settings and select **Next**. 

1. [] Do the same for the **Networking**, **Data protection**, **Encryption** and **Tags** tabs.

1. [] On the **Review + Create** tab, review all of the settings and then select **Create**.

    >[!NOTE] You may need to wait for the account to be created.

1. [] Select the **X** in the top-right corner, but leave Storage Accounts open.

===

### Task 4 - Exclude a folder in Windows Security 

___

1. [] On W-11 CL-1, start File Explorer and then create a folder in **Documents** called **+++MDfStorage+++**.

1. [] On W-11 CL-1, in the **Search** field, type **+++Windows Security+++** and then select it.

1. [] In Windows Security, select **Virus & threat protection**.

1. [] In Virus & threat protection, under **Virus & threat protection settings**, select **Manage settings**.

1. [] In Virus & threat protection settings, under **Exclusions**, select **Add or remove exclusions**.

1. [] At the User Account Control prompt, select **Yes**.

1. [] In Exclusions, select **Add an exclusion** and then select **Folder**.

1. [] At the Select Folder prompt, select the **MDfStorage** folder you created earlier and then select **Select Folder**.

1. [] Close Windows Security.

@lab.Activity(Automated2)

===

# **Exercise 2: Test Defender for Storage** 

In this exercise, now that we have enabled Defender for Storage, you will perform a test.

Estimated time to complete: *15 minutes*

#### **What you will learn**

After completing the tasks in this exercise, you'll be  able to:

- Create anEICAR file
- Verify Defender for Cloud is enabled
- Create a container
- Upload malware file to the container
- Review the Defender for Cloud Security Alert 

===

### Task 1 - Create an EICAR file  

___

1. [] On W-11 CL-1, open **Notepad**.

1. [] In Notepad, enter the following **+++X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*+++**

1. [] Save this file in the **MDfStorage** folder you created earlier and name it **+++eicar.com+++**.

1. [] Close Notepad and File Explorer.

===

### Task 2 - Verify Defender for Cloud is enabled on the storage account

___

1. [] In the Microsoft Azure portal, in Storage Accounts, select the storage account you created earlier.

1. [] On the sa@lab.LabInstance.Id blade, on the navigation pane, under **Security + networking** select **Microsoft Defender for Cloud**.

1. [] On the Microsoft Defender for Cloud blade, wait while enablement completes.

    >[!NOTE] This may take a few hours to complete. It is recommended that you save the lab at this point if this enablement is taking a long time. To save the lab:
    -  Select the 3 lines at the top-right corner of the banner, and then select **Save**.
    !IMAGE[e7q26mfa.jpg](instructions262909/e7q26mfa.jpg)
    - On the I'd like to prompt, select **Save my lab instance and returnto it later** and then select **OK**.
    !IMAGE[kgqh8l0v.jpg](instructions262909/kgqh8l0v.jpg)
    - Resume the lab the following day and verify Microsoft Defender for Cloud shows as enabled before continuing.


===

### Task 3 - Create a container

___

1. [] When the Microsoft Defender for Cloud blade shows **On** and **Enabled on subscription**, on the navigation pane, expand **Data Storage**, and then select **Containers**.

1. [] On the Containers blade, at the top of the page select **+ Container**.

1. [] On the New container slide-out, in the **Name** field, enter +++**defaultcontainer**+++ and then select **Create**.

===

### Task 4 - Upload malware file to the container 

---

1. [] On the Containers blade, select **defaultcontainer**.

1. [] On the defaultcontainer blade, select **Upload**.

1. [] On the Upload blob slide-out, select **Browse for files**.

1. [] In the Open window, go to the **MDFStorage** folder you created, select **eicar.com** and then select **Open**.

1. [] Back on the Upload blob slide-out, select **Upload**.

1. [] On the Containers blade, select **eicar.com**.

1. [] On the eicar.com blade, under **Blob index tags**, verify the value of **Malware Scanning scan result** shows as **Malicious**.

1. [] Select the **X** icon in the top-right corner, but leave Storage Accounts open.

===

### Task 5 - Review the Defender for Cloud security alert 

---

1. [] In the Microsoft Azure portal, search for and select +++**Microsoft Defender for Cloud**+++.

1. [] On the Overview blade, expand **General** and then select **Security alerts**.

1. [] On the Security alerts blade, verify there is a high severity security alert named **Malicious file uploaded to storage account**.

1. [] Select the security alert and then select **View Full Details**.

1. [] Review the details and that **Take Action** is an option.

===

#**Exercise 3: Configure automation to delete malicious files** 

In this exercise, now that we have enabled Defender for Storage, you will perform a test.

Estimated time to complete: *15 minutes*

#### **What you will learn**

After completing the tasks in this exercise, you'll be  able to:

- Deploy the DeleteBlobLogicApp Azure Resource Manager template
- Grant the Storage Blob Data Owner role to the Logic App
- Grant the Storage Blob Data Owner role to the lab admin account
- Change the authentication method on the container 
- Create workflow automation
- Upload a test malicious file and review the results 

===

### Task 1 - Deploy the DeleteBlobLogicApp Azure Resource Manager template  

___

1. [] In a new tab in Microsoft Edge, go to this address +++**https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fstorageantimalwareprev.blob.core.windows.net%2Fworkflows%2FDeleteBlobLogicApp-template.json**+++

1. [] On the Custom deployment blade, in the **Subscription** drop-down list, verify your subscription is selected.

1. [] In the **Resource group** drop-down list, select **RG1** and then select **Review + Create**.

1. [] On the Review + Create blade, select **Create**.

>[!NOTE]Wait as this template is deployed. It should take around 2-5 minutes. 

===

### Task 2 - Grant the Storage Blob Data Owner role to the Logic App   

___

1. [] When the deployment shows as complete, expand **Deployment details** and then select **Remove-MalwareBlob**.

1. [] On the Remove-MalwareBlob blade, expand **Settings**, and then select **Identity**.

1. [] On the Identity blade, select **Azure role assignments**.

1. [] On the Azure role assignments blade, select **+ Add role assignment (Preview)**.

1. [] On the Add role assignment (Preview) slide-out, in the **Scope** drop-down list, select **Subscription** and verify your subscription is listed in the **Subscription** field.

1. [] In the **Select a role** drop-down, search for and select +++**Storage Blob Data Owner**+++ and then select **Save**.

===

### Task 3 - Grant the Storage Blob Data Owner role to the lab admin account  

___

1. [] In the Azure Portal, search for and select **+++Resource groups+++**.

1. [] On the Resource groups blade, select **RG1**.

1. [] On the RG1 blade, on the navigation pane, select **Access control (IAM)**.

1. [] On the Access control (IAM) blade, select the **Role assignments** tab.

1. [] On the Role assignments tab, at the top of the page select **+ Add** and then select **Add role assignment**.

1. [] On the Add role assignment blade, search for and select +++**Storage Blob Data Owner**+++

1. [] Select the **Members** tab and then select **+ Select members**.

1. [] On the Select members slide-out, search for and select the lab admin username you are using for this lab, **+++@lab.CloudPortalCredential(User1).Username+++** and then select **Select**.

1. [] Back on the Add role assignment blade, select **Review + assign**.

1. [] On the Review + assign tab, select **Review + assign**.

===

### Task 4 - Change the authentication method on the container 

___

1. [] In the Azure Portal, search for and select +++**Storage Accounts**+++.

1. [] On the Storage Accounts blade, select the storage account you created earlier.

1. [] On the Storage account **sa@lab.LabInstance.Id** blade, on the navigation pane, expand **Data storage**, and then select **Containers**.

1. [] On the Containers blade, select **defaultcontainer**.

1. [] On the defaultcontainer blade, at the top of the page select **Switch to Microsoft Entra user account**.

===

### Task 5 - Create a workflow automation  

___

1. [] In the Azure Portal, search for and select +++**Microsoft Defender for Cloud**+++.

1. [] On the Microsoft Defender for Cloud blade, on the navigation pane, expand **Management** and then select **Workflow automation**.

1. [] On the Workflow automation blade, select **+ Add workflow automation**.

1. [] On the Add workflow automation slide-out, in the **Name** field, enter +++**Delete-malicious-blob**+++

1. [] In the **Subscription** drop-down list, verify your lab subscription is selected.

1. [] In the **Resource group** drop-down list, select **RG1**.

1. [] In the **Alert name contains** field, enter +++**Malicious file uploaded to storage account**+++.

1. [] In the **Logic App name** drop-down list, enter +++**Remove-MalwareBlob**+++ and then select **Create**.

===

### Task 6 - Upload a malware file to the container 

---

1. [] In the Azure Portal, search for and select +++**Storage Accounts**+++.

1. [] On the Storage Accounts blade, select the storage account you created earlier.

1. [] On the Storage account **sa@lab.LabInstance.Id** blade, on the navigation pane, expand **Data storage**, and then select **Containers**.

1. [] On the Containers blade, select **defaultcontainer**.

1. [] On the defaultcontainer blade, select **Upload**.

1. [] On the Upload blob slide-out, select **Browse for files**.

1. [] On the Open window, browse to the **MDFStorage** folder you created, select **eicar.com** and then select **Open**.

1. [] Back on the Upload blob slide-out, select the checkbox for **Overwrite if files already exist**, and then select **Upload**.

===

### Task 7 - Review results 

___

1. [] On the Containers blade, select **Refresh** and watch as the file gets deleted.

1. [] The page should show **No blobs found** when the file is automatically deleted.

@lab.Activity(Automated3)

===

#**Exercise 4: Configure Log Analytics ** 

In this exercise, now that we have enabled Defender for Storage, you will perform a test.

Estimated time to complete: *15 minutes*

#### **What you will learn**

After completing the tasks in this exercise, you'll be  able to:

- Create a log analytics workspace
- Edit Defender for Cloud settings  
- Upload a malware file to a storage account 
- Review log analytics results 

===

### Task 1 - Create a Log Analytics workspace 
___

1. [] In the Microsoft Azure portal, search for and select **+++Log Analytics workspaces+++**.

1. [] On the Log Analytics workspaces blade, select **Create log analytics workspace**.

1. [] On the Create Log Analytics workspace blade, in the **Subscriptions** drop-down list, verify your lab subscription is selected.

1. [] In the **Resource group** drop-down list, select **RG1**.

1. [] In the **Name** field, type +++**DefaultLAWorkspace**+++ and then select **Review + Create**.

1. [] On the Review + Create blade, review the settings and then select **Create**.

===

### Task 2 - Edit Defender for Cloud settings   
___

1. [] In the Microsoft Azure portal, search for and select **+++Storage accounts+++**.

1. [] On the Storage accounts blade, select the storage account you created earlier.

1. [] On the Storage account **sa@lab.LabInstance.Id** blade, on the navigation pane, expand **Security + networking** and then select **Microsoft Defender for Cloud**.

1. [] On the Microsoft Defender for Cloud blade, under **Microsoft Defender for Storage**, select **Settings**.

1. [] On the Settings slide-out, under **Advanced Settings**, set **Override Defender for Storage subscription-level setting** to **On**.

1. [] Select the checkbox for **Send scan results to Log Analytics**, then in the drop-down select **DefaultLAWorkspace** and select **Save**.

===

### Task 3 - Disable the workflow automation  

___

1. [] In the Azure Portal, search for and select +++**Microsoft Defender for Cloud**+++.

1. [] On the Microsoft Defender for Cloud blade, on the navigation pane, expand **Management** and then select **Workflow automation**.

1. [] On the Workflow automation blade, select the checkbox for **Delete-malicious-blob** and then select **Disable**.

1. [] Verify Delete-malicious-blob shows as **Disabled**.

===

### Task 4 - Upload a malware file to the container 

---

1. [] In the Microsoft Azure portal, search for and select **+++Storage accounts+++**.

1. [] On the Storage accounts blade, expand **Data Storage** and then select **Containers**.

1. [] On the Containers blade, select **defaultcontainer**.

1. [] On the defaultcontainer blade, select **Upload**.

1. [] On the Upload blob slide-out, select **Browse for files**.

1. [] In the Open window, browse to the **MDFStorage** folder you created, select **eicar.com**, and then select **Open**.

1. [] Back on the Upload blob slide-out, select the checkbox for **Overwrite if files already exist**, and then select **Upload**.

1. [] On the Containers blade, select **eicar.com**.

1. [] On the eicar.com blade, under **Blob index tags** verify the value of **Malware Scanning scan result** shows as **Malicious**.

1. [] Select the **X** in the top-right corner, but leave Storage Accounts open.

===

### Task 5 - Review Log Analytic results  
___

1. [] In the Microsoft Azure portal, search for and select **+++Log Analytics workspaces+++**.

1. [] On the Log Analytics workspaces blade, select **DefaultLAWorkspace**.

1. [] On the DefaultLAWorkspace blade, on the navigation pane, select **Logs**.

1. [] On the Logs blade, on the Welcome to Log Analytics prompt, select the **X** in the upper-right corner.

1. [] On the Queries prompt, select the **X** icon in the top-right corner.

1. [] On the New Query 1 window, enter the following +++**StorageMalwareScanningResults | sort by TimeGenerated asc**+++ and then select **Run**.

1. [] In the query results window, notice the malicious file found and the fields displayed:
  - TimeGenerated [UTC]
  - CorrelationId
  - OperationName
  - StorageAccountName
  - BlobUri
  - BlobEtag
  - ScanFinishedTimeUtc [UTC]
  - ScanResultDetails
  - Type
  - _ResourceId
  - TenantId
  - SourceSystem

    >[!NOTE] It may take a few minutes for the logs to arrive.

@lab.Activity(Automated4)

===

#**Exercise 5: Implement Azure Based Access Control (ABAC)** 

To make sure that your apps and users can only read non-malicious files, which means those that Defender for Storage found without threats, you can implement Azure Based Access Control (ABAC). In this exercise we will explore how you can have roles with a condition to only read the files that have no threats found.

Estimated time to complete: *15 minutes*

#### **What you will learn**

After completing the tasks in this exercise, you'll be  able to:

- Review available roles
- Add role assignment and add a condition
- Test the new condition

===

### Task 1 - Rollback previous exercise tasks
___

1. [] In the Microsoft Azure portal, search for and select +++**Logic Apps**+++.

1. [] On the Logic apps blade, select the **Remove-MalwareBlob** Logic app we worked with earlier.

1. [] On the Remove-MalwareBlob blade, select **Disable**.

1. [] In the Microsoft Azure portal, search for and select +++**Resource groups**+++.

1. [] On the Resource groups blade, select **RG1**.

1. [] On the RG1 blade, on the navigation pane, select **Access Control (IAM)**.

1. [] On the Access Control (IAM) blade, select the **Role assignments** tab.

1. [] On the Role assignments tab, under **Storage Blob Data Owner**, select the checkbox next to the lab admin user, and then select **Delete**.

1. [] On the Remove role assignments prompt, select **Yes**.

===

### Task 2 - Add role assignment and add a condition
___

1. [] In the Microsoft Azure portal, search for and select +++**Storage Accounts**+++.

1. [] On the Storage accounts blade, select the storage account you created earlier.

1. [] On the **sa@lab.LabInstance.Id** blade, on the navigation pane, select **Access Control (IAM)**.

1. [] On the Access Control (IAM) blade, select the **Role assignments** tab.

1. [] On the Role assignments tab, select **+ Add** and then select **Add role assignment**.

1. [] On the Add role assignment blade, search for and select +++**Storage Blob Data Contributor**+++.

1. [] Select the **Members** tab and then select **+ Select members**.

1. [] On the Select members slide-out, in the **Search by name or email address** field, enter the lab Azure admin accountname you are using for this lab, **+++@lab.CloudPortalCredential(User1).Username+++**, and then select **Select**.

1. [] Back on the Add role assignment blade, select the **Conditions** tab.

1. [] On the Conditions tab, select **+ Add condition**.

1. [] On the Add role assignment condition blade, under **Add action**, select **+ Add action**.

1. [] On the Select an action slide-out, select **Read a blob** and then select **Select**.

1. [] Back on the Add role assignment condition blade, under **Build expression**, select **+ Add expression**.

1. [] In the **Attribute source** drop-down list, select **Resource**.

1. [] In the **Attribute** drop-down list, select **Blob index tags [Values in key]**.

1. [] In the **Key** field, enter +++**Malware scanning scan result**+++.

1. [] In the **Operator** drop-down list, select **StringeEqualsIgnoreCase**.

1. [] In the **Value** field, enter +++**No threats found**+++.

1. [] Once all of these values have been entered, at the top of the page next to Editor type, select **Code**.

    >[!NOTE]This code can be saved and used in future conditions.

1. [] Select **Save**.

1. [] Back on the Add role assignment blade, select  **Review + assign**.

1. [] On the Review + assign tab, select **Review + assign**.

1. [] Back on the Access Control (IAM) blade, select the **X** icon in the top-right corner.

===

### Task 3 - Configure storage account settings

---

1. [] On the Storage accounts blade, select the storage account you created earlier.

1. [] On the **sa@lab.LabInstance.Id** blade, on the navigation pane, expand **Settings** and then select **Configuration**.

1. [] On the Configuration blade, disable **Allow storage account key access**.

1. [] Enable **Default to Microsoft Entra authorization in the Azure portal** and then select **Save**.

===

### Task 4 - Test the new role condition 

---

1. [] Still on the Configuration blade, on the navigation pane, expand **Data Storage** and then select **Containers**.

1. [] On the Containers blade, select **defaultcontainer**.

1. [] On the defaultcontainer blade, select **Upload**.

1. [] On the Upload blob slide-out, select **Browse for files**.

1. [] On the Open window, browse to the **MDFStorage** folder you created, select **eicar.com** and then select **Open**.

1. [] On the Upload blob slide-out, select the **Overwrite if files already exist** checkbox, and then select **Upload**.

1. [] On the Containers blade, select **eicar.com**.

1. [] Verify you receive a message saying this request is not authorized to perform this operation using this permission.

1. [] Select the **X** icon in the top-right corner.

@lab.Activity(Automated5)

===

# Survey

## Please take a moment to answer these questions. Providing answers to these questions will complete this lab. Your answers will help us understand the effectiveness of the lab and help to improve these labs as a whole.

@lab.ActivityGroup(completionsurvey)

===