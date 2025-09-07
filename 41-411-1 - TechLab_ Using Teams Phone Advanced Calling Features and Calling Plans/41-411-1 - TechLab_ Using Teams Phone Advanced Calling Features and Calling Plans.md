
# Survey

## This lab is a technical deep dive on Teams Phone advanced features and calling plans. Before we begin, please take a moment to consider your experience and comfort level with this topic and answer these questions.

@lab.ActivityGroup(initialsurvey)

===

# **Lab 1: Using Teams Phone advanced features and calling plans**


You and your group of Microsoft 365 administrators are taking on a new Microsoft Teams project that will help bring the company one step closer to using the available Teams voice solutions. During this pilot program you need to become familiar with some of the associated global Teams settings and policies. You'll want to learn how you can connect and use PowerShell modules to manage your Teams settings as well. The company uses an on-premises solution for voice calling over the public switched telephone network (PSTN) and that solution is close to its end of support. There is a strong interest in solving for PSTN calls using Microsoft to provide that capability. As one of the solutions you'll be investigating, your team must set up different voice scenarios for individual user calling as well as basic call center capabilities in Microsoft Teams.

Estimated time to complete: *75 minutes*

#### **Before you begin**

To complete the exercise in this lab, you will be provided a Microsoft 365 tenant with an Enterprise E5 trial subscription.  

The type text ![TypeTextIcon.png](typetexticon.png "Type text icon") and copy/paste
![clipboard.png](clipboard.png "Clipboard icon") features used in this lab will send the specified text string to the active window or active text box in the virtual machine. Always compare the text in the lab document with the typed text in the virtual machine and virtual machine and verify the expected text is shown.

#### **What you will learn**

After completing the exercises, you will be able to:

- Create and assign Calling policies.
- Create voicemail policies.
- Deploy calling plans and phone numbers.
- Create auto attendant and call queues.
- Create different caller policies to support voice users.

Select **Next** to continue to the lab exercises.  
*All credentials* are provided in the lab steps.

===

## Exercise 1: Configuring global Teams settings and user policies

Contoso has established that all users not having a specific calling policy assigned must be enabled for call recording. In addition, a specific group of users should also have the option to enable transcription and busy-on-busy. This does not apply to calls during meetings. The company also wants you to create a new voicemail policy to support German as the primary language for the employees that are located in Germany. This voicemail policy must also have the profanity filter and transcription enabled.
___

### Tasks

### Reset Global User password
This task may not be required depending on the tenant being issued in this lab. If the you are not presented with the password change prompt, you can continue to the next task.

1. []  In the CLIENT01 virtual machine sign in as **@lab.VirtualMachine(CLIENT01).Username** with password +++@lab.VirtualMachine(CLIENT01).Password+++

1. [] Open Microsoft Edge **In-Private Browser** and then go to +++https://portal.microsoft.com+++.

1. [] Sign in as +++AllanD@@lab.CloudCredential(M365Calling).TenantName+++ with +++@lab.CloudCredential(M365Calling).UserPassword+++ as the password.

    
### Review user settings before making changes (Optional)
You will be making several changes to user accounts, take a few moments to review what default settings are before moving on with the lab.

1. [] If not already open; in the **Task Bar** select **Microsoft Teams**.

1. [] In the Microsoft Teams window, select **Sign in**.

1. [] In the **Sign in** box, enter +++AllanD@@lab.CloudCredential(M365Calling).TenantName+++ and then select **Next**.  

1. [] In the **Password** box, enter +++@lab.CloudCredential(M365Calling).UserPassword+++ and then select **Sign in**.

1. [] In the Stay signed in to all your apps window, select **No, sign in to this app only**.

1. [] Close the introduction wizard and any other notifications.

1. [] Within the Teams application you can review the default configuration. 

    - No extra options in the Team Calling app settings
    - No dial pad available and no assigned phone number

===

### 1. Configure the default policy Cloud recording for calling setting

In Microsoft Teams, calling policies control which calling and call forwarding features are available to users. Calling policies determine whether a user can make private calls, use call forwarding or simultaneous ringing to other users or external phone numbers, route calls to voicemail, send calls to call groups, use delegation for inbound and outbound calls, and more settings.


1. [] Open Microsoft Edge and then go to +++https://admin.teams.microsoft.com+++.

    >[!ALERT] If you are unable to access the Microsoft Teams admin center, perform the following steps:
    >
    >- Using the same browser tab, go to +++https://admin.microsoft.com+++.
    >- In the Microsoft 365 admin center, in the left navigation, select **Show all**.
    >- Under **Admin centers**, select **Teams**.

1. [] Sign in as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ as the password.

1. [] In the left navigation, select **Voice** > **Calling policies**.  

1. [] In the **Calling policies** list, select **Global (Org-wide default)**.

1. [] On the Global (Org-wide default) page, locate **Cloud recording for calling** and set it to **On**.

    <!-- ref for "turn on" in example at https://learn.microsoft.com/style-guide/a-z-word-list-term-collections/t/turn-on-turn-off -->

1. [] At the bottom of the page, select **Save**.

1. [] In the **Changes will take time to take effect** dialog box, select **Confirm**.

>[!KNOWLEDGE] If you want to learn more, go to [**Calling and call-forwarding features - Microsoft Teams**](https://learn.microsoft.com/MicrosoftTeams/teams-calling-policy "Calling and call-forwarding features - Microsoft Teams")

===

### 2. Create a new Calling policy to allow Cloud recording, transcription, and busy on busy when in a call

1. [] On the Calling policies page, select **+ Add**.

1. [] At the top of the page, in the **Add a name for your calling policy** box, enter +++Allow cloud recording policy+++.  

1. [] In the **Add a description so you know why it was created** box, enter +++Allow cloud recording, transcription, and busy on busy+++.

1. [] Verify **Cloud recording for calling** is enabled to allow users to record their individual calls.

1. [] Turn on the **Transcription** toggle.

1. [] Next to **Busy on busy during calls**, select the **More information** ![Information icon](informationicon.png "Image of the More information icon"){15} icon and review the setting's description.

1. [] Select the **Busy on busy during calls** menu, review the different options, and then select **Let users decide**.

1. [] At the bottom of the Calling policies page, select **Save**.

===

### 3. Assign the Calling policy named Allow cloud recording policy to multiple users

1. [] On the Calling policies page, select the checkbox to the left of the **Allow cloud recording policy**.

1. [] On the menu, select **Manage users** > **Assign users**.

1. [] In the **Manage users** pane, in the **Search by display or username** box, enter +++Allan+++.

1. [] In the resolved names, hover over **Allan Deyoung** and select **Add**.

1. [] Using the same steps, add +++Megan Bowen+++ and +++Diego Siciliani+++.

1. [] When complete, select **Apply**.

1. [] In the **Assignment will take time to take effect** dialog box, select **Confirm**.

>[!KNOWLEDGE] If you want to learn more, go to [**Assign policies in Teams - Microsoft Teams**](https://learn.microsoft.com/MicrosoftTeams/policy-assignment-overview "Assign policies in Teams - Microsoft Teams")

===

### 4. Create a new Voicemail policy setting the primary and secondary languages, profanity mask and transcription

1. [] In the left navigation, under **Voice**, select **Voicemail policies**.

1. [] On the Voicemail policies page, select **+ Add**.

1. [] In the **Add a name for your voicemail policy** box, enter +++Voicemail policy for Germany+++.

1. [] In the **Description** box, enter +++This voicemail policy is configured to use German as the primary language and English as the secondary language+++.

1. [] Select the **Primary prompt language** menu and then select **German (Germany)**.

1. [] Select the **Secondary prompt language** menu and then select **English (United States)**.

1. [] Verify **Voicemail transcription** is turned **On**.

1. [] Turn on the **Mask profanity in voicemail transcription** toggle.

1. [] At the bottom of the page, select **Save**.  

>[!KNOWLEDGE] If you want to learn more, go to [**Manage Voicemail Policies - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/manage-voicemail-policies "Manage Voicemail Policies - Microsoft Teams")

===

## Exercise 2: Streamlining administration with PowerShell

Your team will be using Windows PowerShell to manage Microsoft Teams resources. You'll create and assign a new Voicemail policy to all members of the Marketing department.
___

### Tasks

___

### 1. Retrieve current voicemail policies and create a new marketing group using PowerShell. Assign members to group.

1. [] In the lab virtual machine, right-click **Start** and then select **Terminal (Admin)**.  

    >[!NOTE] The PowerShell modules used in this exercise have already been installed in CLIENT01.

1. [] In the **User Account Control** dialog box, select **Yes**.


1. [] Connect to Microsfot Teams. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Connect-MicrosoftTeams
     
    ```
      |   |   |
    |---|---|
    | **User name** | +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ |
    | **Password** | +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ |


1. [] Retrieve the current Teams voicemail policies. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Get-CsOnlineVoicemailPolicy | fl Identity
    ```

1. [] Review the output of the command and verify the **Voicemail policy for Germany** is listed.  

1. [] Create Marketing Group enter the following:
    ```New-Team -DisplayName "Marketing department (DE)" -Description "Collaboration space for Contoso's Marketing department" ```

1. [] Create a variable to hold the name of a voicemail policy name. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    $vmPolicyName = "Tag:Voicemail policy for Germany"
    ```
Verify by running:
    ``` PowerShell-wrap
    $vmPolicyName
    ```
    <!-- ORIGINAL BEFORE MS REVIEW-UPDATE 1. [] Create a variable to hold the name for the group whose members will be assigned the voicemail policy. In Windows PowerShell, enter the   following and then press Enter:

        ``` PowerShell-wrap
        $groupName = 'Marketing department (DE)'
        ```
    -->

    <!-- ORIGINAL BEFORE MS REVIEW-UPDATE 1. [] This command will identify the members of the Marketing department (DE) group and then assign the voicemail policy to each member of the  group.  
        In Windows PowerShell, enter the following and then press Enter:
    
        ``` PowerShell-wrap
        Get-MsolGroupMember -GroupObjectId (Get-MsolGroup | where {$_.DisplayName -eq $groupName}).ObjectId | ForEach-Object {$Account = $_.DisplayName; $sipAddr=$_.EmailAddress; Write-Host "Processing $Account - $sipAddr"; Get-CSOnlineUser $_.EmailAddress -ErrorAction   SilentlyContinue | Grant-CsOnlineVoicemailPolicy -PolicyName $vmPolicyName}
        ```
   
    -->
1. [] Return to the Teams admin center in Edge, in the left menu choose **Teams > Manage Teams**. You will see the group you created with Powershell. 

1. [] On the Manage teams page, select the **Marketing department (DE)** team.

1. [] On the Marketing department (DE) page, select the **Add members**.

1. [] In the **Add members** pane, in the **Search by display or username** box, enter +++Allan+++.

1. [] In the resolved names, hover over **Allan Deyoung** and select **Add**.

1. [] Using the same steps, add +++Megan Bowen+++ and +++Diego Siciliani+++.

1. [] When complete, select **Apply**.

### 2. Assign members a voicemail policy and to Marketing group, verify with PowerShell.

1. [] 
    ``` PowerShell-wrap
    Connect-Graph -Scopes "Group.ReadWrite.All"
    ```
      |   |   |
    |---|---|
    | **User name** | +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ |
    | **Password** | +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ |

1. [] In the dialog box select consent on behalf of my organization and the select **Approve**

1. [] Import the groups module; in Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Import-Module Microsoft.Graph.Groups
    ```
1. [] Create a variable that will hold the ID of the Marketing Group; in Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    $groupName = "Marketing department (DE)"
    ```
Verify by running:
    ``` PowerShell-wrap
    $groupName
    ```
1. [] This command will identify the Marketing department (DE) group: In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    $groupId = (Get-MgGroup -Filter "displayName eq '$groupName'").Id
    ```
1. [] This command will identify the members of the Marketing department (DE) group
In Windows PowerShell, enter the following and then press Enter::

    ``` PowerShell-wrap
    $members = Get-MgGroupMemberAsUser -GroupId $groupId
    ```
Verify by running:
    ``` PowerShell-wrap
    $members
    ```
1. [] Assign Voicemail policy to members of the Marketing group; in Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    $members | ForEach-Object {grant-csonlinevoicemailpolicy -identity $_.id -policyname "Voicemail policy for Germany"}
    ```
@lab.Activity(1)

1. [] Return to the Teams admin center in Edge, in the left menu choose **Voice > Voicemail Policies**.

1. [] On the Voicemail policies page, select view users in **Voicemail policy for Germany** see the new users added to the policy.

===

## Exercise 3: Configure Calling Plans

Phone System is Microsoft's technology for enabling call control and Private Branch Exchange (PBX) capabilities in the Microsoft 365 cloud with Microsoft Teams. With Phone System, users can use Teams to place and receive calls, transfer calls, and mute or unmute calls. Phone System users can click a name in their address book, and place Teams calls to that person. Calls between users in your organization are handled internally and external calls will use one of the available PSTN Connectivity Types. In the exercise you will configure Calling Plans as connectivity type including advance calling features offered by Phone System.  
More information can be found here [**What is Phone System - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/what-is-phone-system-in-office-365 "What is Phone System - Microsoft Teams")

For the pilot, you will add the Microsoft Teams Domestic Calling Plan trial licenses to your Microsoft 365 tenant. You will assign the licenses to accounts and then configure a user to with a new phone number.
___

### Tasks

___

<details>
<summary>**Purchase the Microsoft Teams Domestic Calling Plan trial** (This task may not be needed depending on your license)</summary>

1. [] In Microsoft Edge, go to +++https://admin.microsoft.com+++.  
    If necessary, sign in as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with password +++@lab.CloudCredential(M365Calling).AdministrativePassword+++.

1. [] On the Microsoft 365 admin center page, in the left navigation, select **Marketplace** then **All products tab**.

1. [] On the Marketplace page, in the **Search all product categories** box, enter +++Calling Plan+++ and then press Enter.

1. [] Scroll down and, under **Add-ons** > **Microsoft Teams Domestic Calling Plan**, select **Details**.

1. [] On the **Microsoft 365 Domestic Calling Plan page**, select **Start free trial**.

1. [] On the Check out page, select **Try now**.

1. [] On the order receipt page, select **Continue**.
>
</details>
===

### 1. Assign the Domestic Calling Plan license to users

As an admin, you can enable users to make phone calls with a Domestic Calling Plan or an International Calling Plan in Microsoft 365. Allan Deyoung and Megan Bowen recently joined Contoso and their jobs requires them to make domestic phone calls.

1. [] On the Microsoft 365 admin center page, in the left navigation, select **Users** > **Active users**.

1. [] In the **Active users** list, select **Allan Deyoung**.

1. [] In the **Allan Deyoung** pane, select the **Licenses and apps** tab.

1. [] In the **Licenses** list, select the **Microsoft Teams Domestic Calling Plan** checkbox.

    >[!ALERT] If the Microsoft Teams Domestic Calling Plan license is not displayed, continue to the next step since that means this has already been assigned.

1. [] Select **Save changes** and then close the pane.

1. [] Using the same steps, assign the **Microsoft Teams Domestic Calling Plan** license to **Megan Bowen**.

1. [] When complete both Allan and Megan will be assigned the Microsoft Teams Domestic Calling Plan license.

===

### 2. Add an emergency location

There are several options when deciding upon the appropriate method in providing emergency calling services to end users.
You must create a required location for the users enabled for Calling Plans.

A location is a civic address-with an optional place. If your business has more than one physical location, it's likely that you'll need more than one emergency location.

When you create an emergency address, a unique location ID is automatically created for this address. If you add a place to an emergency address-for example, if you add a floor to a building address-a location ID is created for the combination of the emergency address and place.

1. [] On the Microsoft 365 admin center page, in the left navigation, select **Show all**.

1. [] Under **Admin centers**, select **Teams**.

1. [] In the Microsoft Teams admin center, in the left navigation, select **Locations** > **Emergency addresses**.

1. [] On the Emergency addresses page, select **+ Add**.

1. [] In the **Add a name for your emergency address** box, enter +++Main office Memphis+++.

1. [] Select the **Country or region** menu and then select +++United States+++.

1. [] In the **Address** box, enter +++6750 Poplar Ave+++ and then, in the resolved addresses list, select **6750 Poplar Ave, Germantown, TN 38138**.  
This specific address is a known address but is not mandatory for this lab. You may choose to use another address.

1. [] Review the map and then scroll down, verify the **I acknowledge and agree that I have read this information and understand how emergency calling works in Microsoft Teams.** check box.

1. [] Review the information in the newly opened pane, select **Cancel**, and then select **Save**.

>[!KNOWLEDGE] If you want to learn more, go to [**Plan and manage emergency calling - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/what-are-emergency-locations-addresses-and-call-routing "Plan and manage emergency calling - Microsoft Teams")

===

### 3. Order a phone number

Before you can set up users in your organization to make and receive phone calls, you must get phone numbers for them.

There are three ways to get user numbers:

- Use the Microsoft Teams admin center. For some countries and regions, you can get numbers for your users using the Microsoft Teams admin center. See Get new phone numbers for your users.

- Port your existing numbers. You can port or transfer existing numbers from your current service provider or phone carrier. See Transfer phone numbers to Teams or Manage phone numbers for your organization for more information to help you do this.

- Use a request form for new numbers. Sometimes, depending on your country or region, you won't be able to get your new phone numbers using the Microsoft Teams Admin Center or you'll need specific phone numbers or area codes.

1. [] In the Microsoft Teams admin center, in the left navigation, select **Voice** > **Phone numbers**.

1. [] On the Phone numbers page, under **Numbers**, select **+ Add**.

1. [] In the **Add a name for your order** box, enter +++Lab phone number+++.

1. [] In the **Add a description so you know why it was created** box, enter +++Phone number for lab use+++.

1. [] Under **Location and quantity**, select the **Country or region** menu and then select +++United States+++.

    >[!ALERT] For this lab, only numbers in the United States will be used. Please do not use another Country/Region.

1. [] Select the **Number type** menu and then select **User (subscriber)**.

1. [] In the **Quantity** box, enter +++2+++.

1. [] Under **Search for new numbers**, in the **Search by the location's name or city** box, enter +++Main+++.

1. [] In the resolved location list, hover over **Contoso** and select **Select**.

1. [] In the United States, some locations have multiple area codes. Select the **Area code** menu and then select an available area code.

1. [] Select **Next**.  
If there are no numbers available for your selected location, add another location and try again.

    >[!ALERT] Please do not select more numbers than required to complete the lab.

1. [] Review the **Reserved numbers** and then select **Place order**.

1. [] On the Thank you, your order has been placed page, review the information and then select **Finish**.

1. [] On the Phone number page, on the **Numbers** tab, review the listed number and scroll to the right to see the **Assignment status**.  
If necessary, refresh the browser to update the list.

===

### 4. Assign phone numbers to users

1. [] In the **Numbers** list, select an unassigned number and then, on the menu, select **Edit**.

1. [] In the **Assign/unassign** pane, under **Assigned to** box, enter +++Allan+++.

1. [] In the resolved names, hover over **Allan Deyoung** and select **Assign**.

1. [] Under **Emergency location**, verify **Main office Memphis** is shown.

1. [] Review the remaining information and then select **Apply**.

1. [] In the **You've assigned a phone number to this user** dialog box, select the different device options and when complete, close the dialog box.

1. [] Using the same steps, assign the second user phone number to +++Megan Bowen+++.

===

### 5. Review the assigned phone numbers

1. [] On the Phone numbers page, review the list of assigned numbers.  
    Notice the **Assignment status** column value.  
    You may need to refresh the browser to refresh the updated assigned numbers.

    >[!NOTE] The Assignment status will show as *Pending* until the number has been fully provisioned and updated in Microsoft Teams.

1. [] (OPTIONAL) On your own, you may call the number assigned to Allan Deyoung.  
    If the number shows as *Pending*, your call may not complete as expected.

===

### 6. Configure outbound calling and call forwarding

As an administrator, you can use outbound call controls to restrict the type of audio conferencing and end-user Public Switched Telephone Network (PSTN) calls that can be made by users in your organization.

Outbound call controls can be applied on a per-user basis or on a tenant basis and provide the following two controls to independently restrict each type of outbound call. By default, both controls are set to allow international and domestic outbound calls.

1. [] In the Microsoft Teams admin center, in the left navigation, select **Users** > **Manage users**.

1. [] On the Manage users page, select **Allan Deyoung**.

1. [] In the Allan Deyoung user card, select the **Voice** tab.

1. [] Review forwarding options. Under **Call answering rules** select **Be immediately forwarded**.

1. [] Select the **Call forward type** menu and then review the available options.

1. [] Reset to the default setting. Under **Call answering rules**, select **Ring Allan Deyoung's devices**.

1. [] Select the **Also allow** menu and then select **Simultaneous ring a user**.

1. [] In the **Select a person** box, enter +++Megan+++.

1. [] In the results list, select **Megan Bowen**.

1. [] Select the **If unanswered** menu and then select **Send to voicemail**.

1. [] Select the **Ring for this many seconds before redirecting** menu and then select **30 seconds**.

1. [] Select **Save**.

===

## Exercise 4: Configure and license resource accounts for Auto Attendants and Call Queues

Auto attendants allow you to set up menu options to route calls based on caller input. Menu options for an auto attendant, such as "For Sales, press 1" or "For Services press 2", let an organization provide a series of choices that guide callers to their destination quickly, without relying on a human operator to handle incoming calls.

Call queues are waiting areas for callers. For situations where callers need to reach someone with a particular specialty, such as sales or service rather than a specific person, you can use call queues to connect callers to the group of agents who can assist them. Callers are put on hold until an agent assigned to the queue is available to take their call.

Used together, auto attendants and call queues can easily route callers to the appropriate person or department in your organization.

In Microsoft Teams, a resource account is required for each auto attendant or call queue. Resource accounts may also be assigned telephone numbers. This is how you assign phone numbers to auto attendants and call queues allowing callers from outside Teams to reach the auto attendant or call queue.

Your team needs to perform all necessary tasks to create, license, and order and assign phone numbers to the resource accounts that will be used for the auto attendant and call queue.

>[!KNOWLEDGE] If you want to learn more, go to [**Plan for Teams auto attendants and call queues - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/plan-auto-attendant-call-queue "Plan for Teams auto attendants and call queues - Microsoft Teams")

___

### Tasks

___

<details>
<summary>**Purchase Microsoft Teams Phone Resource Account licenses** (This task may not be needed depending on your license)</summary>

1. [] In Microsoft Edge, go to +++https://admin.microsoft.com+++.  
    If necessary, sign in as +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ with password +++@lab.CloudCredential(M365Calling).AdministrativePassword+++.

1. [] In Microsoft Edge, switch to the Microsoft 365 admin center tab.  
   If necessary, open a new tab and then go to +++https://admin.microsoft.com+++.

1. [] On the Microsoft 365 admin center page, in the left navigation, select **Marketplace** then **All products tab**.

1. [] On the Marketplace page, in the **Search all product categories** box, enter +++Resource Account+++ and then press Enter.

1. [] Under **Add-ons** > **Microsoft Teams Phone Resource Account**, select **Details**.

1. [] On the Microsoft Teams Phone Resource Account page, under **Select license quantity**, set the value to +++5+++ and then select **Buy**.

1. [] On the Checkout page, under **Billed to**, select **Invoice (Pay by check or wire transfer**, and then select **Save**.

1. [] On the right, select **Place order** and wait for the order to complete.

1. [] On the **You're all set** page, select **Go to Admin Home**.
>
</details>

===

#### 1. Create resource accounts for an auto attendant and call queue

1. [] In Microsoft Edge, switch to the Microsoft Teams admin center tab.  
    If necessary, open a new browser tab and then go to +++https://admin.teams.microsoft.com+++.

1. [] In the left navigation, select **Voice** > **Resource accounts**.

1. [] On the Resource accounts page, select **+ Add**.

1. [] In the **Add resource account** pane, in the **Display name** box, enter +++Contoso Main AA+++.

1. [] In the **Username** box, enter +++ContosoMainAA+++.

1. [] Select the **Resource account type** menu and then select **Auto attendant**.

1. [] Review the information and then select **Save**.

1. [] Perform the same steps to create another resource account. This will be a call queue account type using the following information:

    | Setting | Value |
    |---|---|
    | Display name | +++Personnel Management Call Queue+++ |
    | Username | +++PersonnelManagementCQ+++ |
    | Resource account type | Call queue |

===

### 2. License the auto attendant and call queue accounts

1. [] Switch to the Microsoft 365 admin center. If necessary, open a new tab and then go to +++https://admin.microsoft.com+++.

1. [] In the left navigation, select **Users** > **Active users**.

1. [] In the **Active users** list, select **Contoso Main AA**.

1. [] In the **Contoso Main AA** pane, select the **Licenses and apps** tab.

1. [] Under **Licenses**, select the **Microsoft Teams Phone Resource Account** checkbox and then select **Save changes**.

    >[!ALERT] If the license is not listed, you may have to wait 1-2 minutes, refresh the page, and then try again.

1. [] Close the **Contoso Main AA** pane.

1. [] Perform the same steps to license the **Personnel Management Call Queue** resource account.

@lab.Activity(Question13)

===

### 3. Order a phone numbers for the auto attendant

1. [] Switch to Microsoft Teams admin center. If necessary, open a new browser tab and then go to +++https://admin.teams.microsoft.com+++.

1. [] In the left navigation, select **Voice** > **Phone numbers**.

1. [] On the Phone numbers page, under **Numbers**, select **+ Add**.

1. [] In the **Add a name for your order** box, enter +++Lab resource phone number+++.

1. [] In the **Add a description so you know why it was created** box, enter +++Phone number for lab resource accounts+++.

1. [] Under **Location and quantity**, select the **Country or region** menu and then select +++United States+++.

    >[!ALERT] For this lab, only numbers in the United States will be used. Please do not use another Country/Region.

1. [] Select the **Number type** menu and then select **Auto attendant (Toll)**.

1. [] In the **Quantity** box, enter +++1+++.

1. [] Under **Search for new numbers**, in the **Search by city name** box, enter +++Main+++.

1. [] In the resolved location list, hover over **Contoso** and select **Select**.

1. [] In the United States, some locations have multiple area codes. Select the **Area code** menu and then select an available area code.

1. [] Select **Next**.  
If there are no numbers available for your selected location, add another location and try again.

    >[!ALERT] Please do not select more numbers than required to complete the lab.

1. [] Review the **Reserved numbers** and then select **Place order**.

1. [] On the Thank you, your order has been placed page, review the information and then select **Finish**.

1. [] On the Phone number page, on the **Numbers** tab, review the listed number and the **Assignment status**.  
If necessary, refresh the browser to update the list.

1. [] In the following box, enter the phone number that was requested for the auto attendant.  
    This number will be used later in this lab.

    @lab.TextBox(aaphonenumber)

===

### 4. Assign a phone number to the resource account

1. [] In the left navigation, select **Voice** > **Resource accounts**.

1. [] On the Resource accounts page, hover over **Contoso Main AA** and select the checkmark to the left of the Display name.

1. [] On the menu, select **Assign/unassign**.

    >[!ALERT] If the account is still being provisioned, you will need to wait until account setup has completed.  
    > This may take up to 30 minutes to complete.

1. [] In the **Assign/unassign** pane, select the **Phone number type** menu and then select **Calling Plan**.

1. [] Select the **Assigned phone number** menu, select the available phone number and then select **Save**.

1. [] Refresh the browser and then verify the new phone number has been assigned.

===

## Exercise 5: Create a call queue, auto attendant and voice applications policy

The company intends on using call queues and an auto attendant the same way they have the on-premises voice solution configured. You and the team must deploy a call queue and auto attendant in Microsoft Teams and become familiar with some of the features and capabilities of them.

___

### Tasks

___

### 1. Configure call flows for after hours and holidays

Holidays can be used to provide alternate messages and call routing to callers for specific dates and times when departments, call queues or people in your organization will be following different working hours or won't be available. For example, you might create a holiday for New Year's day when your organization may be closed.

1. [] In the Microsoft Teams admin center, in the left navigation, select **Voice** > **Holidays**.

1. [] On the Holidays page, select **+ Add**.

1. [] In the **Add the name of your holiday** box, enter +++Company Holidays+++.

1. [] Under **Dates**, select **+ Add new date**.

1. []  Add two or three dates your company recognizes as holidays and then save your changes.

===

### 2. Create a call queue

A call queue is analogous to a waiting room in a physical building. Callers wait on hold while calls are routed to the agents in the queue. Call queues are commonly used for sales and service functions. However, call queues can be used for any situation where the number of calls exceeds your internal capacity, such as a receptionist in a busy facility.

1. [] In the left navigation, select **Voice** > **Call queues**.

1. [] On the Call queues page, select **+ Add**.

1. [] In the **Add a name for your call queue** box, enter +++Personnel Management+++.

1. [] Under **Resource accounts**, select **Add**.

1. [] In the **Add accounts** pane, enter +++Personnel+++.

1. [] In the resolved names, hover over **Personnel Management** and select **Add**.

    >[!KNOWLEDGE] Notice the **Add resource account** option. A new resource accounts can be created when adding a call queue.

1. [] At the bottom of the **Add accounts** pane, select **Add**.

1. [] Under **Language**, select the menu and then select **English**.  
You may also choose another language that best fits your deployment.

1. [] Review the remaining settings and then select **Next**.

1. [] On the Greetings and music page, review the settings and then select **Next**.  
   This setting will not be configured now.

1. [] On the Call answering page, select **Choose users and groups**.

1. [] Select **Add users**.

1. [] In the **Add users** pane, enter +++Megan+++.

1. [] In the resolved names, hover over **Megan Bowen** and select **Add**.

1. [] Add +++Allan Deyoung+++ using the same steps.

1. [] When complete, in the **Add users** pane, select **Add**.

1. [] In the **Users** list, verify or reorder the users so Megan is first in the list.

1. [] Select **Next** for each of the remaining pages, **Agent selection**, **Call overflow handling**, and **Call timeout handling** and review their settings.

    Please go to [Create a call queue in Microsoft Teams - Microsoft Teams](https://learn.microsoft.com/microsoftteams/create-a-phone-system-call-queue?tabs=general-info "Create a call queue in Microsoft Teams - Microsoft Teams") for more information about each configurable element of the new call queue.

    >[!KNOWLEDGE] More information is available wherever you see the **More information** ![Information icon](informationicon.png "Image of the More information icon"){15} icon.  
    >Selecting the icon will open a description of the item.

1. [] When complete, at the bottom of the page, select **Submit**.

===

### 3. Create a new auto attendant

The primary purpose of an auto attendant is to direct a caller to an appropriate person or department based on the caller's input to the provided menu options. Callers can be directed to specific people within your organization, to call queues where they wait to talk to the next available agent, or to voicemail. Different call routing options can be specified for business hours, off hours, and holidays.

1. [] In the Microsoft Teams admin center in the left navigation, select **Voice** > **Auto attendants**.

1. [] On the Auto attendants page, select **+ Add**.

1. [] In the **Add a name for your auto attendant** box, enter +++Contoso AA+++.

1. [] Under **Operator (optional)**, select the menu and then select **Person in organization**.

1. [] In the **Search by display name** box, enter +++Megan+++ and then, in the resolved names, select **Megan Bowen**.

1. [] Under **Time zone**, select the menu and choose a time zone.

1. [] Under **Language**, select the menu and then select English.
You may also choose another language that best fits your deployment.


1. [] Under **Voice inputs** select the toggle to set it to **On**.

1. [] Select **Next**.

1. [] On the Call flow page, under **Greeting options**, select **Add a greeting message**.

1. [] In the text box, enter +++Welcome to the Contoso Memphis Office+++.

1. [] Under **Call routing options**, select **Play menu options**.

1. [] Under **Set up the greeting and menu options**, select **Add a greeting message**.

1. [] In the text box, enter +++For the operator, please press 0 or say operator and for the Personnel Management press 1 or say Personnel Management+++.

1. [] Under **Set menu options**, select **+ Assign a dial key**.

1. [] Select the **Dial key** menu and then select **0**.

1. [] In the **Voice command** box, enter +++Operator+++.

1. [] Select the **Redirect to** menu and then select **Person in organization**.

1. [] In the destination box, enter +++Megan+++.

1. [] In the resolved names list, select **Megan Bowen**.

1. [] On the menu, select **+ Assign a dial key**.

1. [] For the new dial key, set the **Dial key** to **1**.

1. [] In the **Voice command** box, enter +++Personnel management+++.

1. [] Select the **Redirect to** menu and then select **Voice app**.

1. [] In the **Search by resource account**, enter +++Personnel+++.

1. [] In the resolved names, select **Personnel Management Call Queue**.

1. [] When complete, your menu options should look similar to the following image:

    !IMAGE[autoattendantmenuoptions2.png](autoattendantmenuoptions2.png "Screen image of the Auto Attendant call routing menu options")

1. [] Under **Directory search**, review the default setting and then select **Next**.

1. [] On the Set business hours page, review the default settings and then select **Next**.  
    Business hours will be configured in a later task.

1. [] On the Holiday call settings page, review the default settings and then select **Next**.  
    Additional voice feature settings will be configured in a later task.

1. [] On the Dial scope page, review the default settings and then select **Next**.

1. [] On the Resource accounts page, select **Add**.

1. [] In the **Add accounts** pane, enter +++Contoso+++.

1. [] In the resolved accounts list, hover over **Contoso Main AA** and select **Add**.

1. [] At the bottom of the **Add accounts** pane, select **Add**.

1. [] On the Resource accounts page, select **Submit**.

### 4. Create a custom voice applications policy
Voice applications policies allow you to create and assign voice application policies to authorized users. Voice application policies control what configuration changes an authorized user can make to the auto attendants and call queues they're authorized for.

1. [] In the Microsoft Teams admin center, in the left navigation
, select **Voice** > **Voice applications policies**.
1. [] On the **Voice applications policies** page select **Add**.
1. [] Enter +++After Hours Voice Applications Policy+++ in the name section and enter +++After hours policy for assigned users+++ in the description
1. [] Toggle **After hours greeting** to the on position.
!IMAGE[ah.png](instructions257271/ah.png)
1. [] Toggle **Welcome greeting** and **Shared voicemail greeting for no agents** to the on position.
!IMAGE[wel.png](instructions257271/wel.png)
1. [] Select **Save**
1. [] On the **Voice applications policies** page, select the **After Hours Voice Application Policy** checkbox then select **Assign users**
1. [] In the manage users window enter +++Allan Deyoung+++ in the search box.
1. [] Hover over **Allan Deyoung** select **Add** then select **Apply**
1. [] In the **Assignment will take time to take effect** dialog box, click **Confirm**.


===

## Exercise 6: Working with voice features

Voice and calling policies are used to control voice and calling in Microsoft Teams and the company expects to need different policies for different business segments that are located in different regions. You need to become familiar with some of these policies and dial plan settings that will be used at scale after the pilot has ended and implementation begins.

___

### Tasks

___

<!-- ### 1. Review calling policies

1. [] In the left navigation, select **Voice** > **Calling policies**.

1. [] Review the existing policies and then select **Global (Org-wide default)**.

1. [] Review each of the Global policy settings that are available to your users.

1. [] When finished, select **Cancel**.

=== -->

### 1. Add a caller ID policy

1. [] In the left navigation, select **Voice** > **Caller ID policies**.

1. [] On the Caller ID policies page, select **+ Add**.

1. [] In the **Add a name for your call ID policy** page, enter +++Memphis Main Office+++.

1. [] Select the **Replace the caller ID with** menu and then select **Resource account**.

1. [] Select the **Replace the caller ID with this Resource account** menu, and then select the available Resource account.

1. [] Review the remaining settings and then select **Save**.

@lab.Activity(Question10)

===

### 2. Configure emergency policy settings

1. [] In the left navigation, select **Voice** > **Emergency policies**.

1. [] On the Emergency policies page, select the **Global (Org-wide default)** policy.

1. [] In the **Emergency numbers** section, select **+Add**.

1. [] In the **Emergency numbers** pane, enter the following setting**.

    | **Setting** | **Value** |
    |---|---|
    | Notification mode | Conferenced in but are muted |
    | Users and groups for emergency calls notifications | +++Allan Deyoung+++ |


    >[!HINT] For this lab a single user account is being added, however, we recommend using groups to avoid the possibility of a single user not being available for this critical services. More information can be found at [**Manage emergency calling policies in Microsoft Teams - Microsoft Teams**](https://learn.microsoft.com/MicrosoftTeams/manage-emergency-calling-policies "Manage emergency calling policies in Microsoft Teams - Microsoft Teams")

1. [] Verify your changes and then select **Apply** on the Emergency policies page select **Save**.

1. [] In the **Changes will take time to take effect** dialog box, review the information and then select **Confirm**.

@lab.Activity(Question12)

===

### 3. Set call park policies

Call park and retrieve lets a user place a call on hold. When a call is parked, the service generates a unique code for call retrieval. The user who parked the call or someone else can then use that code with a supported app or device to retrieve the call.

1. [] In the left navigation, select **Voice** > **Call park policies**.

1. [] On the Call park policies page, select the **Global (Org-wide default)** policy.

1. [] To the right of **Call park** select the toggle switch and set it to **On**.

1. [] Review the remaining settings and then select **Save**.

1. [] In the **Changes will take time to take effect** dialog box, review the information and then select **Confirm**.

>[!KNOWLEDGE] If you want to learn more, go to [**Call park and retrieve in Microsoft Teams - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/call-park-and-retrieve "Call park and retrieve in Microsoft Teams - Microsoft Teams")

===

### 4. Configure call flows for after hours and holidays

Business hours can be set for each auto attendant. 
- If business hours aren't set, all days and all hours in the day are considered business hours because a 24/7 schedule is set by default.
- Business hours can be set with breaks in time during the day, and all of the hours that are not set as business hours are considered after-hours.
- You can set different incoming call-handling options and greetings for after-hours.

Depending on how you have configured your auto attendants and call queues, you may only need to specify after-hours call routing for auto attendants with direct phone numbers.

1. [] In the left navigation, select **Voice** > **Auto attendants**.

1. [] Select the **Contoso AA** auto attendant.  

1. [] On the left, select the **Call flow for after hours** tab.

1. [] Configure the business hours using the following settings:

    - Monday - Friday: set the business hours to **8:00 AM** - **5:00 PM**

    >[!ALERT] If the changes are not able to be saved, review the remaining steps, select **Cancel** and continue to the next task. In some cases, the service is still provisioning the newly added services.

1. [] On the left, select the **Call flows during holidays** tab.

1. [] Select **+ Add**.

1. [] In the **Enter a name for the call setting** box, enter +++Contoso holidays+++.

1. [] Select the **Holiday** menu and then select **Company Holidays**.

1. []  Under **Greeting**, select **Add a greeting message**.

1. [] In the box, enter +++The company is closed today in observance of a scheduled company holiday. Please see our website for normal business hours. Thank you.+++

1. [] Review the changes and then select **Save**.

1. [] On the Holiday call settings page, select **Submit**.


===

### 5. Create a dial-plan

1. [] In the Microsoft Teams admin center, in the left navigation, select **Voice** > **Dial plans**.

1. [] On the Dial plans page, select **+ Add**.

1. [] In the **Add a name for your dial plan** box, enter +++External calling+++.

1. [] Under **Normalization rules**, select **Add**.

1. [] In the **Add new rule** pane, in the **Name** box, enter +++Main office outbound normalization rule+++.

1. [] Under **Rule creation mode**, select **Advanced**.

1. [] Review the advanced settings and notice the use of regular expressions (RegEx).

1. [] Select **Basic**.

1. [] Under **If all select conditions match**, select the **The number dialed begins with** checkbox.

1. [] In the **The number dialed begins with** box, enter +++9+++.

1. [] Under **Then do this**, select the **Remove this many digits from the start of the number** checkbox.

1. [] In the **Remove this many digits from the start of the number** box, enter +++1+++.

1. [] Select the **Add this number to the beginning** checkbox.

1. [] In the **Add this number to the beginning** box, enter ++++1+++

1. [] Under **Test this rule**, in the **Enter a phone number to test** box, enter +++94255552345+++ and then select **Test**.

1. [] Review the result and verify it shows **+14255552345**.

1. [] Select **Save**.

1. [] On the External Calling dial plan page, select **Save**.

>[!KNOWLEDGE] Teams traverses the list of normalization rules from the top down and uses the first rule that matches the dialed number. If you set up a dial plan so that a dialed number can match more than one normalization rule, make sure the more restrictive rules are sorted above the less restrictive ones. If you set up a dial plan that normalizes a dialed number without a "+", the calling service will attempt to normalize the number again using Tenant and regional dial plan rules. To avoid double normalization, it's recommended that all normalization rules result in numbers starting with a "+". Direct Routing customers can use trunk translation rules to remove the "+" if required. (E.164)
>
>If you want to learn more, go to [**Create and manage dial plans - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/create-and-manage-dial-plans "Create and manage dial plans - Microsoft Teams")

===

### 6. Configure Shared line appearance in Microsoft Teams

Shared line appearance lets a user choose a delegate to answer or handle calls on their behalf. This feature is helpful if a user has an administrative assistant who regularly handles the user's calls. In the context of shared line appearance, a manager is someone who authorizes a delegate to make or receive calls on their behalf. A delegate can make or receive calls on behalf of the delegator.

As the administrator, you can change call forwarding and delegation settings for your users. You might want to change these settings, for example, if:

- A user is out on sick leave, and you need to ensure that incoming calls to the user are forwarded to a colleague.
- You need to inspect the call forward settings for all users in a department and potentially correct them as appropriate.
- A new assistant has been employed and you need to add the assistant as a delegate for a group of employees.

1. [] Switch to the Windows terminal window.

    >[!ALERT] <details><summary>Expand the following instructions if you have closed the Windows Terminal window.</summary>
    >1. Create a variable to hold your tenant credentials. In Windows PowerShell, enter the following and then press Enter:
    >
    >   ``` PowerShell-wrap
    >   $cred = Get-Credential
    >   ```
    >
    > 1. In the Windows PowerShell credential request window, enter the following credentials and then select **OK**:
    >
    >       |   |   |
    >       |---|---|
    >       | **User name** | +++@lab.CloudCredential(M365Calling).AdministrativeUsername+++ |
    >       | **Password** | +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ |
    ></details>

1. [] Connect to Microsoft Teams. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Connect-MicrosoftTeams -Credential $cred
    ```

1. [] Add Allan Deyoung as a delegate of Megan Bowen with permissions to make and receive call on behalf of Megan. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    New-CsUserCallingDelegate -Identity MeganB@@lab.CloudCredential(M365Calling).TenantName -Delegate AllanD@@lab.CloudCredential(M365Calling).TenantName -MakeCalls $true -ReceiveCalls $true -ManageSettings $true
    ```

    There is no output from the command.
    
@lab.Activity(Automated4)

    >[!HINT] You may also try [**Share a phone line with a delegate**](https://support.microsoft.com/office/share-a-phone-line-with-a-delegate-16307929-a51f-43fc-8323-3b1bf115e5a8 "Share a phone line with a delegate")


>[!KNOWLEDGE] If you want to learn more, go to [**Shared line appearance in Microsoft Teams - Microsoft Teams**](https://learn.microsoft.com/microsoftteams/shared-line-appearance "Shared line appearance in Microsoft Teams")
>For additional information to [**Share a phone line with a delegate**]

===

### 7. Routing rules for unassigned numbers

As an administrator, you can route calls to unassigned numbers in your organization. For example, you might want to re-route calls to unassigned numbers as follows:

- Re-route all calls to a given unassigned number to a custom announcement.
- Re route all calls to a given unassigned number to the main switchboard.

You can re-route calls to unassigned numbers to a user, to a resource account associated with an auto attendant or a call queue, or to an announcement service that will play a custom audio file to the caller.

1. [] In the Microsoft Teams admin center, in the left navigation, select **Voice** > **Phone numbers**.

1. [] On the Phone numbers page, select the **Routing rules** tab.

1. [] Review the information and then select **Add a new rule**.

1. [] In the **Add a routing rule** pane, in the **Rule name** box, enter +++Test routing rule+++.

1. [] In the **Evaluation order** box, enter +++1+++.

1. [] Select the **Select a rule** menu, review the available options, and then select **Single number**.

    >[!NOTE] For this lab, we do not have any unassigned numbers to test and will use a non-routable phone number.

1. [] Under **1: If all select conditions match**, in the **Enter a number** box, enter ++++12065550001+++.

1. [] Under **2: Then do this**, select the **Routing options** menu and then select **Voice application**.

1. [] In the **Assign to** box, enter +++Contoso+++.

1. [] In the resolved names list, hover over **Contoso Main AA** and select **Assign**.

1. [] Select **Save**.

===

### 8. Test unassigned numbers routing

1. [] On the **Routing rules** tab, on the menu, select **Test number**.

1. [] In the **Test a phone number** dialog box, verify that **Enter a number** is selected.

1. [] In the **Enter a number** box, enter ++++12065550001+++ and then select **Test**.

1. [] Review the output and verify that the unassigned number has matched the routing rule created earlier.

1. [] Close the **Test a phone number** dialog box.

===

## Exercise 7: Using voice enabled channels (Collaborative Calling)

Voice enabled channels connects a call queue to a channel in a team. All team members will be agents in that queue and can easily sign-in and sign-out from the channel, they can also see other agents signed in.

This feature is being considered by the company and you must be prepared to demonstrate the configuration and some of the capabilities.

___

### Tasks

___

### 1. Create a new channel named HR Call Queue in the Contoso team

1. [] In the Microsoft Teams admin center in the left navigation, select **Teams** > **Manage teams**.

1. [] On the Manage teams page, select the **Contoso** team.

1. [] On the Contoso page, select the **Channels** tab.

1. [] On the menu, select **+ Add**.

1. [] In the **Add** pane, in the **Name** box, enter +++HR Call Queue+++.

1. [] Verify **Type** is set to **Standard** and then select **Apply**.

===

### 2. Configure the Personnel Management call queue to use the new HR Call Queue channel

1. [] In the left navigation, select **Voice** > **Call queues**.

1. [] On the Call queues page, select **Personnel Management**.

1. [] In the left navigation, select **Call answering**.

1. [] On the Call answering page, select **Choose a team**.

1. [] Select **Add a channel**.

1. [] In the **Add a channel** pane, enter +++Contoso+++.

1. [] In the results, hover over **Contoso** and select **Add**.

1. [] Select the **Select the channel** menu and then select **HR Call Queue**.

1. [] Review the changes and then select **Apply**.

1. [] On the Personnel Management page, at the bottom of the page, select **Submit**.

<!-- 
# Removed after Microsoft review
### 3. Update the auto attendant to route calls

1. [] In the Microsoft Teams admin center, in the left navigation, select **Voice** > **Auto attendants**.

1. [] On the Auto attendants page, select **Contoso AA**.

1. [] On the left, select **Call flow**.

1. [] Under **Call routing options**, select **Redirect call**.  
    For this lab, calls will be sent directly to a call queue.

1. [] Select the **Redirect to** menu and then select **Voice app**.

1. [] In the **Search by resource account** box, enter +++Personnel+++.

1. [] In the results, select **Personnel Management Call Queue**.

1. [] Select **Submit**. 
-->

===

### 3. Review user setting and test the voice-enabled channel (Optional)

1. [] If not already open in the CLIENT01 virtual machine, in the **Start menu** select **Microsoft Teams (work or school)**.

1. [] In the **Sign in** box, enter +++AllanD@@lab.CloudCredential(M365Calling).TenantName+++ and then select **Next**.

1. [] In the **Password** box, enter +++@lab.CloudCredential(M365Calling).AdministrativePassword+++ and then select **Sign in**.

1. [] In the Stay signed in to all your apps window, select **No, sign in to this app only**.

1. [] Close the introduction wizard and any other notifications.

1. [] In the Contoso team, select the **HR Call Queue** channel.

1. [] Using your mobile phone, call **@lab.Variable(aaphonenumber)**.

1. [] Enter **1** during the menu options and verify the call is routed to the HR Call Queue channel.

1. [] Verify you receive a call notification and then hang up.

1. [] Within the Teams application you can also review how the configuration changes you (Admin) made for Allan are different from when you started.

===

## Exercise 8: Reviewing Operator Connect

Operator Connect is another option for providing Public Switched Telephone Network (PSTN) connectivity with Teams and Phone System.

Operator Connect might be the right solution for your organization if:

- Microsoft Calling Plan isn't available in your geographic location.
- Your preferred operator is a participant in the Microsoft Operator Connect Program.
- You want to find a new operator to enable calling in Teams.

To ensure your team has looked at other calling solutions, you also want to review other options that may work for the company.

___

### Tasks

___

### 1. Review Operator Connect offer details

With Operator Connect, if your existing operator is a participant in the Microsoft Operator Connect Program, they can manage the service for bringing PSTN calling to Teams. The Operator Connect program provides the following benefits:

- Leverage existing contracts or find a new operator. You keep your preferred operator and contracts or choose a new one from a selection of participating operators to meet your business needs.
- Operator-managed infrastructure. Your operator manages the PSTN calling services and Session Border Controllers (SBCs), allowing you to save on hardware purchase and management.
- Faster, easier deployment. You can quickly connect to your operator and assign phone numbers to users -- all from the Teams admin center.
- Enhanced support and reliability. Operators provide technical support and shared service level agreements to improve support service, while direct peering powered by Azure creates a one-to-one network connection for enhanced reliability.

1. [] In CLIENT01, switch to Microsoft Edge and the Microsoft Teams admin center +++https://admin.teams.microsoft.com+++.

1. [] In the left navigation, select **Voice** > **Operator Connect**.

1. [] On the Operator Connect page, select the **All operators** tab.

1. [] Select the **Regions** menu and review the listed countries.  
    You may choose to clear the **All regions** checkbox and then select a single country or leave the default selection for this task.

1. [] In one of the displayed operator's tile, select **Offer details**.

1. [] In the new browser tab, review the operator offer.

1. [] When complete, close the tab and return to the **Operator Connect** tab.

1. [] In the displayed operators, select the logo for one of the operators.

1. [] Review the operator's page and then select **Cancel**.

@lab.Activity(Question11)

===

## Clean up

During this lab you requested and were provided several phone numbers. Use the following steps to release those numbers back to the service so they can be used by others.

1. [] Switch to the Windows Terminal window.

1. [] Remove all phone numbers assigned to Megan Bowen. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Remove-CsPhoneNumberAssignment -Identity MeganB@@lab.CloudCredential(M365Calling).TenantName -RemoveAll
    ```

1. [] Remove all phone numbers assigned to Allan Deyoung. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Remove-CsPhoneNumberAssignment -Identity AllanD@@lab.CloudCredential(M365Calling).TenantName -RemoveAll
    ```

1. [] Remove all phone numbers assigned to the Contoso Main AA resource account. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    Remove-CsPhoneNumberAssignment -Identity ContosoMainAA@@lab.CloudCredential(M365Calling).TenantName -RemoveAll
    ```

1. [] Release the phone numbers from the tenant back to the service. In Windows PowerShell, enter the following and then press Enter:

    ``` PowerShell-wrap
    (Get-CsPhoneNumberAssignment | Where-Object {$_.PstnAssignmentStatus -eq "Unassigned"}).TelephoneNumber | foreach {Remove-CsOnlineTelephoneNumber -TelephoneNumber $_}
    ```

>[!ALERT] When you **End** or when your lab expires, an automated action in the lab platform will remove any assigned public telephone number and release the number(s) back to Microsoft.

===
# Survey

## Please take a moment to answer these questions. Providing answers to these questions will complete this lab. Your answers will help us understand the effectiveness of the lab and help to improve these labs as a whole.

@lab.ActivityGroup(completionsurvey)

===

# Congratulations!

You've completed the **Managing Teams policies and voice settings** lab.
