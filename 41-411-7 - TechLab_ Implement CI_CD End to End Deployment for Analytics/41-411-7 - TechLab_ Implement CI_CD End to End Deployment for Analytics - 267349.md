# Survey

## This lab is a technical deep dive on **TechLab: Implement CI/CD End to End Deployment for Analytics**. Before we begin, please take a moment to consider your experience and comfort level with this topic and answer these questions.  

@lab.ActivityGroup(initialsurvey)

===

@lab.Title
##Understanding Key Concepts of CI/CD 

!IMAGE[n07wphnb.jpg](instructions265985/n07wphnb.jpg)

### What is CI?

Continuous Integration (CI) is a software development practice where developers frequently integrate their code changes into a shared repository. Each integration is automatically verified by running automated builds and tests, allowing teams to detect problems early. The main goals of CI are to improve software quality and reduce the time it takes to deliver new software updates.

Key Components of CI:
- **Version control system**: Centralized repository where code changes are committed. Examples include Git, SVN, and Mercurial.
- **Automated builds**: Each code commit triggers an automated build process to compile the code and create executable artifacts.
- **Automated testing**: Automated tests are run as part of the build process to verify that the code changes don't introduce new bugs.
- **Continuous feedback**: Developers receive immediate feedback on the success or failure of their changes, enabling quick resolution of issues.

Benefits of CI:
- **Early detection of errors**: Problems are identified and fixed earlier in the development cycle.
- **Reduced integration problems**: Frequent integration reduces the complexity of merging code changes.
- **Improved code quality**: Automated tests help maintain a high standard of code quality.
- **Faster development**: Automated processes save time, allowing developers to focus on writing code.

### What is CD?

Continuous Delivery (CD) is a software development practice that builds on CI by automatically deploying all code changes to a staging environment after they pass the automated tests. CD ensures that the software can be reliably released at any time and that deployment is automated, reducing the risks associated with manual deployments.

Key Components of CD:
- **Automated deployment**: Code changes that pass automated tests are automatically deployed to staging environments.
- **Staging environment**: An environment that closely resembles the production environment where final testing occurs before production release.
- **Release automation**: Deployment processes are automated to ensure consistency and reliability.
- **Configuration management**: Management of software configuration files and environment-specific parameters to ensure correct deployment.

Benefits of CD:
- **Reduced deployment risk**: Automation reduces the likelihood of human errors during deployment.
- **Faster time to market**: Automating the deployment process accelerates the release cycle.
- **Higher quality releases**: Automated tests and deployments ensure that only thoroughly tested code reaches production.
- **Improved collaboration**: Teams can work more effectively by integrating and delivering code frequently.

### Azure Data Factory CI/CD Lifecycle

In Azure Data Factory (ADF), the CI/CD process involves moving Data Factory pipelines from one environment to another (development, test, production). Azure Resource Manager templates are used to store the configuration of various ADF entities (pipelines, datasets, data flows, and so on). There are two main methods to promote a data factory to another environment:
- **Automated deployment**: Using Data Factory's integration with Azure Pipelines.
- **Manual upload**: Using Data Factory UX integration with Azure Resource Manager.

#### CI/CD lifecycle steps in ADF:
1. **Development environment**:
   - A development data factory is created and configured with Azure Repos Git.
   - Developers create feature branches for changes and debug their pipeline runs.
2. **Pull request and review**:
   - Developers create a pull request to merge changes into the main branch.
   - Peer reviews ensure code quality before merging.
3. **Publishing changes**:
   - Approved changes are merged and published to the development factory.
4. **Deploying to test environment**:
   - Changes are deployed to a test (UAT) factory using Azure Pipelines.
   - Resource Manager template parameters are used for appropriate configuration.
5. **Deploying to production environment**:
   - After verification in the test environment, changes are deployed to the production factory via the next task in the pipelines release.

**Important Notes**:
- Only the development factory is associated with a Git repository.
- Test and production factories are updated only via CI/CD pipelines or Resource Manager templates.

**Architecture diagram of CI & CD for ADF:**
!IMAGE[fz6nnll4.jpg](instructions265985/fz6nnll4.jpg)

### Azure Databricks the CI/CD Lifecycle

Azure Databricks also follows a CI/CD lifecycle, focusing on continuous integration and delivery of notebooks, jobs, and other Databricks resources. The process involves similar steps to ADF but tailored for Databricks.

#### CI/CD Lifecycle Steps in Azure Databricks:
1. **Development environment**:
   - Development is done in a Git-integrated Databricks workspace.
   - Notebooks and other resources are version-controlled.
2. **Pull request and review**:
   - Changes are made in feature branches and reviewed via pull requests.
3. **Automated testing**:
   - Automated tests are run to ensure changes don't break functionality.
4. **Deployment to staging**:
   - Approved changes are deployed to a staging environment for further testing.
5. **Deployment to production**:
   - After successful staging tests, changes are deployed to the production environment using CI/CD pipelines.

**Architecture diagram of CI & CD for ADB:**
!IMAGE[9q61lo4n.jpg](instructions267340/9q61lo4n.jpg)

===

## Exercise 2: Set up CI for Azure Data Factory using Azure DevOps (30 minutes)
___

# Before you begin

>[!hint] **Auto Text Typing Supported**  
>  
> Select the green type text icon !IMAGE[4bxkzh41.jpg](instructions266048/4bxkzh41.jpg) in the **Instructions** pane to type directly into your virtual machine. Input commands, responses, and code snippets effortlessly within your VM interface.

>[!hint] **Mark Each Step as Complete**  
>  
>Many of the tasks in this lab include a large number of steps. To the left of each step you will see a check box. Select the check box next to a step after you complete the step. This will help ensure that you do not miss a step.

>[!hint] **Saving tokens and other information**  
> 
>In several steps you will be asked to copy some information (GUIDs or bearer tokens for example) and then paste the information into a text field in the instructions. Be sure to click outside of the text field after you paste your data. This ensures that the data is saved for use later in the lab.


===
### Task 1: Create a repository in Azure DevOps and configure a reviewer policy

1. Sign in to @lab.VirtualMachine(W-11CL-1).SelectLink by using the following credentials:

    | | |
    |:--|:--|
    | Username | +++@lab.VirtualMachine(W-11CL-1).Username+++ |
    | Password | +++@lab.VirtualMachine(W-11CL-1).Password+++ |

    >[!hint] You can find the credentials for this lab on the **Resources** tab. 


1. Open a web browser and navigate to +++https://go.microsoft.com/fwlink/?LinkId=2014579&campaign=acom~azure~pipelines~pricing~hero&projectVisibility=Everyone&githubsi=true&clcid=0x409+++.

    !IMAGE[hieay3u6.jpg](instructions267340/hieay3u6.jpg)


1. Sign in with by using the following credentials.

    | **Username** | +++@lab.CloudPortalCredential(User1).Username+++ |
    |:-------------|:-------------------------------------------------|
    | **Password** | +++@lab.CloudPortalCredential(User1).Password+++ |


1. In the Azure DevOps dialog, select **Continue**.

    !IMAGE[qzg6nh39.jpg](instructions265985/qzg6nh39.jpg)   

1. If the Azure DevOps Almost done... dialog displays, change the value for the **We'll host your projects in** drop-down to a country/region that is geographically closest to you.

	 !IMAGE[h4v8wriu.jpg](instructions267349/h4v8wriu.jpg)

1. In the upper left-hand corner of the page, select **Azure Devops**.

	!IMAGE[gnu7pqsb.jpg](instructions267349/gnu7pqsb.jpg)

1. In the lower left of the navigation pane, select **Organization settings**.
	
	!IMAGE[obwczrs2.jpg](instructions267349/obwczrs2.jpg)

1. In the Organization Settings pane, in the **Pipelines** section, select **Settings**.

	!IMAGE[w5myjs4b.jpg](instructions267349/w5myjs4b.jpg)

1. in the General section, locate the **Disable creation of classic build pipelines** and **Disable creation of classic release pipelines** settings.

	!IMAGE[qjshfeja.jpg](instructions267349/qjshfeja.jpg)
	
1. Set the value of **Disable creation of classic build pipelines** and **Disable creation of classic release pipelines** to **Off**.

	!IMAGE[ttfofylm.jpg](instructions267349/ttfofylm.jpg)

1. In the upper left-hand corner of the page, select **Azure Devops**.
       
1. At the top right of the page, select **+ New project**.

	!IMAGE[9fv41ra2.jpg](instructions267349/9fv41ra2.jpg)

	>[!Alert] The Create a project page may open automatically without you selecting **New project**.

1. In the Create new project dialog, enter +++CICD Demo+++ in the Project name field and select **Create**. The project page opens.

    !IMAGE[me9epc5l.jpg](instructions267349/me9epc5l.jpg)

1. Review the options in the left navigation pane. If you see the **Repos** option, skip to step 18. If you do not see the **Repos** option, continue with step 15.

1. At the bottom of the left navigation pane, select **Project settings**. 

	!IMAGE[ms3g2opv.jpg](instructions267349/ms3g2opv.jpg)

1. In the Azure DevOps services section of Project Settings, change the value for Repos from **Off** to **On**.

	!IMAGE[8s0zbupr.jpg](instructions267349/8s0zbupr.jpg)

1. Select the **Click to go back** button on the browser menu to return to the project page.

	!IMAGE[0hx6th99.jpg](instructions267349/0hx6th99.jpg)

1. In the left navigation pane for the project, select **Repos**.

	!IMAGE[uvbjrr2v.jpg](instructions267349/uvbjrr2v.jpg)



1. In the **Initialize main branch with a README or gitignore** section, select **Initialize**.

	!IMAGE[2w0iyac4.jpg](instructions267349/2w0iyac4.jpg)

===
### Task 2: Generate a Personal Access Token (PAT):

1. In in the upper-right corner of the page, select **User settings**.

	!IMAGE[99wy4p1l.jpg](instructions267349/99wy4p1l.jpg)
    
1. Select **Personal access tokens**.

    !IMAGE[2qyo1eiu.jpg](instructions267349/2qyo1eiu.jpg)

1. On the Personal Access Tokens page, select **+ New token**. 

	!IMAGE[uuok9brm.jpg](instructions267349/uuok9brm.jpg)

1. In the Create a new personal access token dialog, enter +++RepoAccessToken+++ in the Name field.

1. In the **Scopes** section, select **Full access**.

	!IMAGE[nfiurows.jpg](instructions267349/nfiurows.jpg)	

1. Select **Create**.

	!IMAGE[8p632vrd.jpg](instructions267349/8p632vrd.jpg)

1. On the **Success** page, select the **Copy** icon next to the token value to copy the value to the Windows clipboard. Paste the token value into the following text field. 

    @lab.TextBox(PAT)

    `My Access Token is @lab.Variable(PAT)`

    >[!note] be sure to move the mouse cursor outside of the text field after you paste the value. This ensures that the token is stored for later use.

1. Select **Close**.    

===
### Task 3: Configure Azure Data Factory to use Azure Repos Git

1. Open a new browser window and go to to +++portal.azure.com+++.

1. In the search box, enter +++adfv2dev@lab.LabInstance.Id+++ and then select the Data Factory resource.

	!IMAGE[9u8rmirn.jpg](instructions267349/9u8rmirn.jpg)

1. On the **adfv2dev@lab.LabInstance.Id** Overview page, select **Launch Studio**.

	!IMAGE[ua3bmkeq.jpg](instructions267349/ua3bmkeq.jpg)

1. In Studio, in the left navigation pane, select **Manage**. 

	!IMAGE[5vtudsvn.jpg](instructions267349/5vtudsvn.jpg)

1. In the **Source control** section, select **Git configuration**.

    !IMAGE[j6t42j4n.jpg](instructions267349/j6t42j4n.jpg)

1. On the Configure a repository page, select **Configure**. 

	!IMAGE[2lz1hkzw.jpg](instructions267349/2lz1hkzw.jpg)

1. In the Congigure a repository dialog, select **Azure DevOps Git** and then select **Continue**.

	!IMAGE[fhj602o1.jpg](instructions267349/fhj602o1.jpg)

1. Configure the repository by using the values in the following table. Leave all other fields at default values. 

	>[!note] For the Collaboration branch field, you will first have to select **+ Create new** enter the name +++main+++, and then select **Create**.

    | Field                    | Value                       |
    |--------------------------|-----------------------------|
    | Azure DevOps organization name | **User1-@lab.LabInstance.Id** |
    | Project name             | **CICD Demo**                  |
    | Repository name          | **CICD DEMO**                   |
    | Collaboration branch     | **main**                        |
    | Publish branch           | **adf_publish**                 |
    | Root folder              | **/**                           |

1. Select **Apply**.

	!IMAGE[o3ltt15i.jpg](instructions267349/o3ltt15i.jpg)

1. Leave the Configure a repository page open. You will return to the page later in the lab.

	!IMAGE[8pfwdsc7.jpg](instructions267349/8pfwdsc7.jpg)
===
### Task 4: Create a feature branch

1. Open a new browser window and go to +++https://dev.azure.com+++.

1. In the Project pane, select **CICD Demo**.

	!IMAGE[7czm5swq.jpg](instructions267349/7czm5swq.jpg)

1. In the left navigation pane, select **Repos** and then select **Branches**.

	!IMAGE[zuddxjmh.jpg](instructions267349/zuddxjmh.jpg)

1. On the Branches page, select **New branch**.

	!IMAGE[f8yseo6k.jpg](instructions267349/f8yseo6k.jpg)

1. Enter +++feature/issue-1234+++ in the **Name** field and then select **Create**.

	!IMAGE[cf6o6fs4.jpg](instructions267349/cf6o6fs4.jpg)

===
### Task 5: Develop and debug the pipeline

1. Return to the **Azure Data Factory Studio** Configure a repository browser window. In the left navigation pane, select **Author**.

	!IMAGE[7ar4d4gj.jpg](instructions267349/7ar4d4gj.jpg)	 

1. In the upper-left corner of the page, select **main branch**.

    !IMAGE[a7c11y3f.jpg](instructions267349/a7c11y3f.jpg)

1. Select **feature/issue-1234** to switch to the feature/issue-1234 branch.

	!IMAGE[izgbhfeg.jpg](instructions267349/izgbhfeg.jpg)

1. Select the **+** button and then select **Pipeline** twice.

    !IMAGE[vips9m7o.jpg](instructions267349/vips9m7o.jpg)

1. In the Properties pane for the new pipeline, enter +++CopyPipeline+++ in the Name field.

	!IMAGE[m6wrt56v.jpg](instructions267349/m6wrt56v.jpg)	

1. In the **Activities** pane, expand the Move and transform node. 

	!IMAGE[wy6tv335.jpg](instructions267349/wy6tv335.jpg)
    
1.  Drag the **Copy data** activity to the pipeline canvas. 

    !IMAGE[18zhvah3.jpg](instructions265985/18zhvah3.jpg)

>[!note]Do not close the CopyPipeline page. You will configure the Copy data activity in the next task.

=== 
### Task 6: Configure the source for the Copy data activity

1. Select the **Copy data** activity and then select **Source**.

	!IMAGE[0jr2ug9b.jpg](instructions267349/0jr2ug9b.jpg)

1. Select **+ New**.

    !IMAGE[8fo5w4jw.jpg](instructions267349/8fo5w4jw.jpg)

1. In the New dataset dialog, select **Azure Blob Storage** and then select **Continue**.

	!IMAGE[oe3xb6u2.jpg](instructions267349/oe3xb6u2.jpg)

1. In the **Select format** dialog, select **DelimitedText** and then select **Continue**.

	!IMAGE[6qqzeu7l.jpg](instructions267349/6qqzeu7l.jpg)

1. In the Set properties dialog enter +++csvdata+++ in the Name field.

1. In the **Linked service**, select **+ New**.

	!IMAGE[3pexu3pa.jpg](instructions267349/3pexu3pa.jpg)

1. Configure the new linked service by using the values in the following table. Leave all other fields at default values. 

    | Field                    | Value                                     |
    |--------------------------|-------------------------------------------|
    | Name                     | +++devblobstorage+++                      |
    | Description              | +++Linked service to the dev blob storage+++ |
    | Authentication type      | Account Key                               |
    | Toggle                   | Azure Key Vault                           |

	!IMAGE[u4rbehfe.jpg](instructions267349/u4rbehfe.jpg)

1. Select **AKV linked service** and then select **+ New**.

1. In the New linked service | Azure Key Vault dialog, enter +++KVserviceDev+++ in the Name field.

1. Configure the key vault by using the values in the following table. Leave all other fields at default values.

    | Field                    | Value                                     |
    |--------------------------|-------------------------------------------|
    | Azure Subscription       | Select the existing subscription from the drop-down list      |
    | Azure key vault name     |**devkeys@lab.LabInstance.Id**                 |
    | Azure key vault name     |+++Dev Key Vault+++                 |
    | Authentication method    | **System assigned Managed Identity**            |

	>[!Note]Leave the New linked service | Azure Key Vault dialog open. You will return to the dialog in an upcoming step. 

1. Open a new browser window and go to +++portal.azure.com+++.

1. In the menu bar for the Azure home page, select **Cloud Shell**.

	!IMAGE[i86092mx.jpg](instructions267349/i86092mx.jpg)

1. Select **PowerShell**.

    !IMAGE[pffecw47.jpg](instructions267349/pffecw47.jpg)

1. In the Getting started dialog, in the Subscription field, select the existing subscription and select **Apply**. Wait for the Cloud Shell to initialize.

    !IMAGE[iswcfbva.jpg](instructions267349/iswcfbva.jpg)

1. Enter the following command and the command prompt and press the Enter key. These commands grant access to Azure Key Vault.

	>[!Note] After you select the Copy icon in the instructions, in the PowerShell window, right-click and select **Paste** to add commands.

    >[!Alert] You may need to press the Enter key several times after you paste the code to ensure that all commands run. </br></br>If any command in steps 16-19 fails due to Azure connectivity issues, run the following command to reauthenticate your session for interactive mode. Then, repeat steps 16-19.
    > ```
    > Connect-AzAccount -UseDeviceAuthentication
    > ```
   

    ```
    # Define the Key Vault name and your user principal ID
    $keyVaultName = "devkeys@lab.LabInstance.Id"
    $userPrincipalId = (Get-AzADUser -UserPrincipalName "@lab.CloudPortalCredential(User1).Username").Id

    # Grant access to the Key Vault
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToSecrets set,delete,get,list
	```

1. Enter the following command and the command prompt and press the Enter key. These commands create a secret.
	```
    $connectionString = (Get-AzStorageAccount -ResourceGroupName "RG1Dev" -Name "sadev@lab.LabInstance.Id").Context.ConnectionString
    
    $secretValue = ConvertTo-SecureString "$connectionString" -AsPlainText -Force
    Set-AzKeyVaultSecret -VaultName "devkeys@lab.LabInstance.Id" -Name "DevConnectionString" -SecretValue $secretValue
	```

1. Enter the following command and the command prompt and press the Enter key. These commands configure the production key vault.
	```
    #For Prod
    # Define the Key Vault name and your user principal ID
    $keyVaultName = "prodkeys@lab.LabInstance.Id"
    $userPrincipalId = (Get-AzADUser -UserPrincipalName "@lab.CloudPortalCredential(User1).Username").Id

    # Grant access to the Key Vault
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToSecrets set,delete,get,list
	```
1. Enter the following command and the command prompt and press the Enter key. These commands configure the production key vault.

	```
    $connectionString = (Get-AzStorageAccount -ResourceGroupName "RG1Prod" -Name "saprod@lab.LabInstance.Id").Context.ConnectionString
    
    $secretValue = ConvertTo-SecureString "$connectionString" -AsPlainText -Force
    Set-AzKeyVaultSecret -VaultName "prodkeys@lab.LabInstance.Id" -Name "ProdConnectionString" -SecretValue $secretValue
	```

1. Close the Azure portal page and return to the browser window which displays the **New linked service | Azure Blob Storage** pane.

1. Select **Create**. The New linked service pane remains open but the context switches to Azure Blob Service.

	!IMAGE[v76oozr8.jpg](instructions267349/v76oozr8.jpg)

1. In the **New linked service** pane, select the **Secret name** drop-down list and then select **DevConnectionString**.

	!IMAGE[iry4hic2.jpg](instructions267349/iry4hic2.jpg)

1. Select **Create**. The Set properties pane displays.

>[!Note] Leave the Set properties pane open. You will return to the pane in an upcoming step.

1. Open a new tab and navigate to +++portal.azure.com+++.

1. Select **Resource Groups**, then select **RG1Dev**.

	!IMAGE[9uxx9lef.jpg](instructions267349/9uxx9lef.jpg)

1. On the **RG1Dev** resource group, select **sadev@lab.LabInstance.Id**.

	!IMAGE[p3r2ba0b.jpg](instructions267349/p3r2ba0b.jpg)

1. On the **sadev@lab.LabInstance.Id** blade, select **Storage Browser**.

	!IMAGE[zxsgzgzp.jpg](instructions267349/zxsgzgzp.jpg)

1. Select **Blob containers** and then select **devshare@lab.LabInstance.Id**.

	!IMAGE[h6hq0g7h.jpg](instructions267349/h6hq0g7h.jpg)

1. On the menu bar for Storage browser, select **Upload**.

	!IMAGE[og1mmnfq.jpg](instructions267349/og1mmnfq.jpg)


1. Select **Browse for files**.

	!IMAGE[2qu9jav9.jpg](instructions267349/2qu9jav9.jpg)

1. Select the **Browse for files** link. In the File Explorer popup, enter +++C:\Users\LabAdmin\Desktop\CICD Demo.csv+++ and select **Open**.

1. Select **Upload**. 

1. Select **Check Your Work** to ensure that you have correctly completed the earlier steps.

	@lab.Activity(Automated6)

1. Close the Azure portal page.

1. Return to the Data Factory | Set properties browser window.

	!IMAGE[24tso96t.jpg](instructions267349/24tso96t.jpg) 

1. In the File Path section, select **Browse**.

	!IMAGE[pp7q2154.jpg](instructions267349/pp7q2154.jpg)

1. Select **devshare@lab.LabInstance.Id**. Select **CICD Demo.csv** and then select **OK** twice.

	!IMAGE[ao9rt569.jpg](instructions267349/ao9rt569.jpg)

>[!Note] Leave the CopyPipeline page open. You will continue configuring the Copy data activity in the next task.

===
##Task 7: Configure the sink for the Copy data activity

1. In the Details pane for the Copy data activity, select **Sink**.

1. On the **Sink** tab, select **+ New** to create a new data sink.

1. Choose **Azure Blob Storage** and then select **Continue**.

1. On **Select format**, choose **DelimitedText** and then select **Continue**.

1. In the name field, enter +++SyncData+++.

1. Select **Linked service** and then select **+ New**.

1. Configure the linked service by using the values in the following table.

    | Field                    | Value                                     |
    |--------------------------|-------------------------------------------|
    | Name                     | +++prodblobstorage+++                      |
    | Description              | +++Linked service to the dev blob storage+++ |
    | Authentication type      | Account Key                               |
    | Toggle                   | Azure Key Vault                           |

1. Under **AKV linked service**, select **+ New**.

	!IMAGE[4uzro6v2.jpg](instructions267349/4uzro6v2.jpg)

1. In the **Name** field, enter +++KVserviceProd+++

1. On the **Edit linked service** page, enter the following information.

    | Field                    | Value                                     |
    |--------------------------|-------------------------------------------|
    | Azure Subscription       | Select the existing subscription from the drop-down list        |
    | Azure key vault name     | **prodkeys@lab.LabInstance.Id**                  |
    | Azure key vault name     |+++Prod Key Vault+++                 |
    | Authentication method    | **System assigned Managed Identity**            |

	!IMAGE[jcqemkhe.jpg](instructions267349/jcqemkhe.jpg)

1. Select **Create**.

	>[!Note] Leave the New linked service | Azure Blob Storage pane open. You will return to the pane in an upcoming step.

1. In a new tab, navigate out to +++portal.azure.com+++.

1. Select **Resource groups**, then select **RG1Prod**.

	!IMAGE[75aveuec.jpg](instructions267349/75aveuec.jpg)

1. Select **prodkeys@lab.LabInstance.Id**, then select **Access policies**.

	!IMAGE[o0ge7wvd.jpg](instructions267349/o0ge7wvd.jpg)

1. On the **Access policies** page, select **Create**.

    !IMAGE[0v6lxovj.jpg](instructions267349/0v6lxovj.jpg)

1. In the **Configure from a template** drop-down list, select **Key, Secret, and Certificate Management**, then select **Next**.

    !IMAGE[cqp0fr56.jpg](instructions267349/cqp0fr56.jpg)

1. In the **Principal name** field, enter +++adfv2dev@lab.LabInstance.Id+++. In the list of search results, select **adfv2dev@lab.LabInstance.Id** and then select **Next**.

	!IMAGE[4guf94rn.jpg](instructions267349/4guf94rn.jpg)

1. Select **Next** again and then select **Create**. 

1. Close the Azure portal page.

1. Return to the Data Factory | New linked service browser window.

	!IMAGE[q8h2qxgw.jpg](instructions267349/q8h2qxgw.jpg)

1. Select the **Refresh** button next to the **Secret name** field. 

    !IMAGE[hzsw0f8g.jpg](instructions267349/hzsw0f8g.jpg)

1. Select **Secret name**  and then select **ProdConnectionString**.

	!IMAGE[r02tnvg0.jpg](instructions267349/r02tnvg0.jpg)

1. Select **Create**.

    !IMAGE[fpd4sz17.jpg](instructions267349/fpd4sz17.jpg)

1. On the Set properties page in the File Path section, select **Browse**.

	!IMAGE[vg3y3z79.jpg](instructions267349/vg3y3z79.jpg)

1. Select **prodshare@lab.LabInstance.Id** and then select **OK** twice.

1. On the menu bar for CopyPipeline, select **Save**. In the Save dialog, select **OK**.

	!IMAGE[pwu4lht6.jpg](instructions267349/pwu4lht6.jpg)

===
### Task 8: Create a Pull Request (PR) from the feature branch to the main branch

Now that you have developed your pipeline, it's time to create a Pull Request (PR) to merge your changes into the main branch.

1. Open a web browser and navigate to +++https://dev.azure.com/+++.

1. Select the **CICD Demo** project.

1. Navigate to **Repos** and then select **Branches**.

1. Select the **feature/issue-1234** branch.

	!IMAGE[3plrxie6.jpg](instructions267349/3plrxie6.jpg)

1. Select **Create a Pull Request:**

    !IMAGE[ilkfi6wz.jpg](instructions267349/ilkfi6wz.jpg)


1. Enter +++ADF pipeline changes+++ in the Title field. Enter +++@lab.CloudPortalCredential(User1).Username+++ in the Reviewers field. 

1. Select **Create** to create the pull request.

===
### Task 8: Review, approve, and merge the pull request into the main branch

The pull request must be reviewed and approved before merging the pull request the main branch.

1. In the **Repos** section of the left navigation pane for the project, select **Pull requests**.

	!IMAGE[3o2h2v43.jpg](instructions267349/3o2h2v43.jpg)

1. Select **ADF pipeline changes**.

1. Review the changes in the commit. Once you're satisfied with the changes, select **Approve** and then select **Complete**.

	!IMAGE[lmb3wus5.jpg](instructions267349/lmb3wus5.jpg)

1. On the Complete pull request pane, select **Complete merge**.

	!IMAGE[rqcnkcl6.jpg](instructions267349/rqcnkcl6.jpg)
===
### Task 9 - Publish the main branch in Azure Data Factory

1. Return to the Data Factory browser window. 

1. At the top left of the page, ensure that **main branch** is selected. Switch to **main branch if necessary**.

    !IMAGE[nmgs9v2q.jpg](instructions267349/nmgs9v2q.jpg)

1. Select **Publish** near the top, then select **OK**. Wait for the publish process to complete.

    !IMAGE[u37uxp20.jpg](instructions267349/u37uxp20.jpg)


===

## Exercise 3: Set up CD for Azure Data Factory by using Azure DevOps (10 minutes)
___

In this exercise, you will configure continuous deployment for the pipeline.
===
### Task 1 - Create Dev and Prod variable groups 


1. Open a web browser and navigate to +++https://dev.azure.com/+++.

1. Select your **CICD Demo** project.



1. In your Azure DevOps project, select **Pipelines** from the left-hand menu.

	!IMAGE[z9whppns.jpg](instructions267349/z9whppns.jpg)

1. Select **Library** and then select **+ Variable group**.

	!IMAGE[mbdilowg.jpg](instructions267349/mbdilowg.jpg)

1. Name the variable group +++Dev+++.

1. Add the following variables and values for the development environment:

    | **Variable**         | **Value**                       |
    |-----------------------|---------------------------------|
    | `KeyVaultName`        | `devkeys@lab.LabInstance.Id`    |
    | `StorageAccountName`  | `sadev@lab.LabInstance.Id`      |

	!IMAGE[pluawiea.jpg](instructions267349/pluawiea.jpg)

1. On the menu bar for the variable group, select **Save**.

	!IMAGE[0cwfseuy.jpg](instructions267349/0cwfseuy.jpg)

1. At the top left of the page, select **Library**.

	!IMAGE[lch3soev.jpg](instructions267349/lch3soev.jpg)

1. Select **+ variable group**.

1. Name the variable group +++Prod+++.

1. Add the following variables and values for the production environment:

    | **variable**         | **Value**                       |
    |-----------------------|---------------------------------|
    | `KeyVaultName`        | `prodkeys@lab.LabInstance.Id`    |
    | `StorageAccountName`  | `saprod@lab.LabInstance.Id`      |


1. Select **Save**.
===
### Task 2: Create a stage to build an Azure Data Factory ARM template

1. On the Azure DevOps page, in the left navigation pane, select **Pipelines**.

1. Select **Pipelines** and then select **Create pipeline**.

1. Select **Use the classic editor**.

	!IMAGE[mfk9sly3.jpg](instructions267349/mfk9sly3.jpg)

1. In the Select a source pane, select **Default branch for manual and scheduled builds** and then select **adf_publish**.

1. Select **Continue**.

    !IMAGE[x9gaxeay.jpg](instructions267349/x9gaxeay.jpg)

>[!note] Leave the Select a template pane open. You will continue configuration in an upcoming step.

===
### Task 3: Create a stage to deploy to Dev

1. At the top of the **Select a template** screen, select **Empty Job**.

	!IMAGE[j8asxcs2.jpg](instructions267349/j8asxcs2.jpg)

1. In the list of tasks, select **Agent Job 1** and then select **+**.

	!IMAGE[t19z42vw.jpg](instructions267349/t19z42vw.jpg)

1. In the search box, enter +++ARM template deployment+++. 

    !IMAGE[xqb9vxn9.jpg](instructions267349/xqb9vxn9.jpg)

1. Select **ARM template deployment** and then select **Add**.

	!IMAGE[jq95ublc.jpg](instructions267349/jq95ublc.jpg)

1. In the list of tasks, select **ARM Template deployment: Resource group scope**.

	!IMAGE[mlk8tndv.jpg](instructions267349/mlk8tndv.jpg)

1. In the Azure Resource Manager connection drop-down list, select **@lab.CloudSubscription.name** and then select **Authorize**.

	!IMAGE[2qb7pbni.jpg](instructions267349/2qb7pbni.jpg)

1. In the **Subscription** drop-down list, select **@lab.CloudSubscription.name**.

	>[!note] If you do not see the **@lab.CloudSubscription.name** option when you select **Subscription**, refresh the Azure Resource Manager connection drop-down list and the Subscription drop-down list.

	!IMAGE[j3qnpzg3.jpg](instructions267349/j3qnpzg3.jpg)	

1. Select **Resource Group** and then select **RG1Dev**.

1. Select location and then select **Central US**.

1. Select the ellipses next to the **Template** dropdown menu. Expand the folder tree and select **ARMTemplateForFactory.json**. Select **OK**.

    !IMAGE[4pmn4eez.jpg](instructions265985/4pmn4eez.jpg)

1. Select the ellipses by the **Template Parameters** dropdown menu. Expand the folder tree and select **ARMTemplateParametersForFactory.json**. Select **OK**.

    !IMAGE[3kr4bnan.jpg](instructions265985/3kr4bnan.jpg)


1. On the pipeline page, select **Triggers**.

	!IMAGE[e22vefw3.jpg](instructions267349/e22vefw3.jpg)

1. Select the checkbox next to **Enable continuous integration**. 

    !IMAGE[l8ruq0bq.jpg](instructions265985/l8ruq0bq.jpg)

1. Select **Save & Queue**. Select **Save & queue** again and then select **Save and run**.

1. In the **Jobs** section, select **Agent job 1**. The deployment will start.

	!IMAGE[otvgfmw1.jpg](instructions267349/otvgfmw1.jpg)

1. Wait for the deployment to be completed successfully before moving on to the next task. 

>[!Note] It may take 1-3 minutes for the job to complete.


===
### Task 4: Create a stage to deploy to Prod

1. On the Azure DevOps page, in the left navigation pane, select **Pipelines**, select **Releases** and then select **New pipeline**.

	!IMAGE[g00ra1ci.jpg](instructions267349/g00ra1ci.jpg)

1. In the Select a template pane, select **Empty job**.

	!IMAGE[j9a2k7ek.jpg](instructions267349/j9a2k7ek.jpg)

1. On the pipeline design canvas, in the **Artifacts** section, select **+ Add**.

    !IMAGE[c142ss9h.jpg](instructions267349/c142ss9h.jpg)

1. In the **Add an artifact** panel, select **Source (build pipeline)**. Select **CI CD Demo** and then select **Add**. 

    !IMAGE[s4hw7cl2.jpg](instructions267349/s4hw7cl2.jpg)

1. On the pipeline design canvas, in the **Artifacts** activity, select **Continuous deployment trigger** (⚡).

    !IMAGE[4onwdd76.jpg](instructions267349/4onwdd76.jpg)

1. Enable the continuous deployment trigger by setting the slider to **Enabled**.

	!IMAGE[klca2uiu.jpg](instructions267349/klca2uiu.jpg)

1. Close the **Continuous deployment trigger** dialog.


1. On the pipeline design canvas, select the **Stage  1** box.

1. Enter +++prod+++ in the **Stage name** field and then close the dialog. 

1. At the top of the page, select **Save**, then select **OK**.

	!IMAGE[5sjdphqi.jpg](instructions267349/5sjdphqi.jpg)
===

## Exercise 4: Validation/Deployment for ADF Prod
___

In this exercise you will complete the deployment.
===
### Task 1 - Monitor the deployment process and check for any errors or issues

1. In a new tab, navigate to +++adf.azure.com+++.

1. In the Select an existing data factory dialog, in the Name field, select **adfv2dev@lab.LabInstance.Id** factory and select **Continue**.

    !IMAGE[s685oxfq.jpg](instructions267349/s685oxfq.jpg)

1. Expand **Pipelines** and select **CopyPipeline**.

    !IMAGE[47jypqcz.jpg](instructions267349/47jypqcz.jpg)

1. On the design canvase, select **Copy data 1**. 

	!IMAGE[eidufb15.jpg](instructions267349/eidufb15.jpg)

1. On the information pane below, change the value in the **Name** field from **Copy data 1** to +++CopyData+++.

    !IMAGE[aoxkjk1a.jpg](instructions267349/aoxkjk1a.jpg)

1. At the top of the page, select **Save all** then select **OK**.

    !IMAGE[34fanaai.jpg](instructions267349/34fanaai.jpg)

1. On the menu bar for the pipeline, select **Publish**. Select **Publish** and then select **OK**.

1. Open a new browser window and go to +++dev.azure.com+++, and browse back to the project folder.

1. In the project pane, select the **CICD Demo** project.

1. In the navigation menu, select **Pipelines**, then select the **Runs** tab.

	!IMAGE[a0q943qc.jpg](instructions267349/a0q943qc.jpg)

1. Watch the deployment to ensure it's successful.

    !IMAGE[w23vvcuu.jpg](instructions265985/w23vvcuu.jpg)




===

## Exercise 5: Continuous Integration configuration (Databricks) - approx. 25 min
===
### Task 1: Set up Azure DevOps project and Initialize and clone Git repository
1. Open a web browser and go to +++https://dev.azure.com/+++.

1. Select **+ New Project**.

1. Enter `CICD Databricks Demo` for the project name.

1. Select **Create**.

1. Review the options in the left navigation pane. If you see the **Repos** option, skip to step 9. If you do not see the **Repos** option, continue with step 6.

1. At the bottom of the left navigation pane, select **Project settings**. 

	!IMAGE[ms3g2opv.jpg](instructions267349/ms3g2opv.jpg)

1. In the Azure DevOps services section of Project Settings, change the value for Repos from **Off** to **On**.

	!IMAGE[8s0zbupr.jpg](instructions267349/8s0zbupr.jpg)

1. Select the **Click to go back** button on the browser menu to return to the project page.

	!IMAGE[0hx6th99.jpg](instructions267349/0hx6th99.jpg)

1. In your Azure DevOps project, navigate to **Repos**.

1. At the bottom of the page, select **Initialize**.

    !IMAGE[li9fkiwk.jpg](instructions265985/li9fkiwk.jpg)    
    
1. Select the **Clone** button in the upper-right corner of the page. 

	!IMAGE[vavvn7as.jpg](instructions267349/vavvn7as.jpg)

1. In the **Clone Repository** pane, select **Copy** to save the URL to the Windows clipboard. 
	!IMAGE[qmdpfs8k.jpg](instructions267349/qmdpfs8k.jpg)

1. Paste the URL into the following text field. 

	@lab.TextBox(RepoURL)
	
>[!note] Leave the Clone Repository pane open. You will return to the pane in a later task.

===
### Task 2: Configure Databricks workspace and repository
1. In a new tab, navigate to +++portal.azure.com+++.

1. In the resource search box, enter and select `databricksdev@lab.LabInstance.Id`.

	!IMAGE[4neldavh.jpg](instructions267349/4neldavh.jpg)

1. Select the **Launch Workspace** button.

	!IMAGE[1nlm5jdd.jpg](instructions267349/1nlm5jdd.jpg)

1. If a Sign in page displays, select **Sign in with Microsoft Entra ID**.

1. In the left navigation pane, select **Workspace**. 

1. In the center pane of the Databricks window, select **Workspace** and then select **Repos**.

	!IMAGE[il6097tb.jpg](instructions267349/il6097tb.jpg)

1. Select **Create Git Folder**.

    !IMAGE[58hnm04d.jpg](instructions267349/58hnm04d.jpg)

1. In the Create Git folder dialog, Git repository URL field, enter +++@lab.Variable(RepoURL)+++ and then select **Create Git folder**. The folder displays. 

1. A README.md displays in the list of resources.

    !IMAGE[kfyezcv4.jpg](instructions265985/kfyezcv4.jpg)

1. In the center pane of the Databricks window, select **Workspace** select **Users**.

1. Expand the **@lab.CloudPortalCredential(User1).Username** folder. Right-click the **CICD Databricks Demo** folder and select **Rename**.

    !IMAGE[zush7f6s.jpg](instructions267349/zush7f6s.jpg)

1. In the name field, enter +++CICD Databricks Demo+++ and select **OK**.

1. In the center pane of the Databricks window, select **Workspace** and then select **Shared**. 

1. Right-click the **Shared** folder and select **Import**. 

    !IMAGE[opero1if.jpg](instructions267349/opero1if.jpg)

1. In the Import from section, select **URL**. 

1. In the **URL** field, enter `https://github.com/databricks/notebook_gallery/blob/main/Notebook%202.0%20Feature%20Gallery.py` and then select **Import**.

    !IMAGE[dasw1ne3.jpg](instructions267349/dasw1ne3.jpg)

1. On the left navigation pane, select **Workspace**. 

1. In the center pane of the Databricks window, select **Workspace** and then select **Shared**. 

	!!IMAGE[4w8yk5yn.jpg](instructions267349/4w8yk5yn.jpg)

1. In the right pane, right-click **Notebook 2.0 Feature Gallery** and select **Move**.

    !IMAGE[9jzd3anr.jpg](instructions267349/9jzd3anr.jpg)

1. Select **Workspace**. The subfolders in the Workspace folder display.

	!IMAGE[kdha4ngh.jpg](instructions267349/kdha4ngh.jpg)
    

1. Select **Users** and then select **@lab.CloudPortalCredential(User1).Username**. 

	!IMAGE[dowgklbt.jpg](instructions267349/dowgklbt.jpg)
	

1. Select **CICD Databricks Demo** and then select **Move**.

    !IMAGE[3xn44gz1.jpg](instructions267349/3xn44gz1.jpg)

1. In the **Change who has access** dialog, select **Confirm**.

1. In the center pane of the Databricks window, select **Workspace** and then select **Users**.

1. Select **@lab.CloudPortalCredential(User1).Username**. Right-click the **CICD Databricks Demo** folder and select **Git** from the context menu. 

    !IMAGE[ewy6l26o.jpg](instructions267349/ewy6l26o.jpg)

1. In the CICD Databricks demo dialog, in the **Commit message** field, enter +++Adding an example Notebook to demonstrate CICD+++ and then select **Commit & Push**.

    !IMAGE[j1torns3.jpg](instructions265985/j1torns3.jpg)

1. Close the dialog.

===
### Task 3: Manage Databricks tokens and Data Factory integration

1. In the upper-right corner of the Databricks page, select your account profile picture and then select **Settings**.

    !IMAGE[fholi78r.jpg](instructions267349/fholi78r.jpg)

1. In the **Settings** pane, in the **User** section, select **Developer**.

    !IMAGE[d9oisn7u.jpg](instructions267349/d9oisn7u.jpg)

1. In the Developer pane, in the **Access tokens** row, select **Manage**, and then select the **Generate new token** button. 

	!IMAGE[7hbaq0ub.jpg](instructions267349/7hbaq0ub.jpg)

1. In the comment field, enter +++Databricks access token (Dev)+++ and then select **Generate**.

	!IMAGE[43yzlo4o.jpg](instructions267349/43yzlo4o.jpg)

1. Copy the token to the Windows Clipboard and then paste the token into the following text field.

	!IMAGE[amb63t0r.jpg](instructions267349/amb63t0r.jpg)

    @lab.TextBox(dbri)

    `My Dev Databricks token is @lab.Variable(dbri)`

1. Select **Done**.

1. In a new tab, navigate to +++portal.azure.com+++.

1. Enter +++adfv2dev@lab.LabInstance.Id+++ into the search box and select data factory v2 resource.

1. Under **Azure Data Factory Studio**, select **Launch Studio**. 

1. In the left navigation pane, select **Manage** and then select **Linked services**.

    !IMAGE[pzo2j9ha.jpg](instructions267349/pzo2j9ha.jpg)

1. On the **Linked services** blade, select **+ New**.

1. On the **New linked service** blade, change from the **Data store** tab to the **Compute** tab. 

	!IMAGE[nuh59lty.jpg](instructions267349/nuh59lty.jpg)

1. From the **Compute** tab, select **Azure Databricks**, then select **Continue**.

    !IMAGE[pybxlvv8.jpg](instructions267349/pybxlvv8.jpg)

1. Configure  the service by using the following values. Leave all other settings at default values.


    | Attribute              | Value                                                               |
    |------------------------|---------------------------------------------------------------------|
    | Name                   | +++DevDatabricksLS+++                                               |
    | Azure Subscription     | **@lab.CloudSubscription.Name**                                       |
    | Databricks workspace   | **databricksdev@lab.LabInstance.Id**                                  |
    | Access token           | +++@lab.Variable(dbri)+++                                           |
    | Cluster version        | **10.4 LTS (includes Apache Spark 3.2.1, Scala 2.12)**              |
    | Cluster node type      | **Standard_DS3_v2**                                                 |

	!IMAGE[ucjpdp5o.jpg](instructions267349/ucjpdp5o.jpg)

    >[!ALERT] If the Cluster version drop-dwon list fails to load, confirm that you have properly pasted the access token.


1. At the bottom of the panel, select **Test connection**. 

	!IMAGE[i4czjgas.jpg](instructions267349/i4czjgas.jpg)

1. When your connection successfully passes, select the **Create** button. 

    
===
### Task 4: Develop and deploy Databricks pipelines


1. On the Data Factory page, select **Author**.

	!IMAGE[6j520m3p.jpg](instructions267349/6j520m3p.jpg)

1. In the **Factory Resources** pane, select **+** and then select **Pipeline** twice.

    !IMAGE[al6clz4j.jpg](instructions267349/al6clz4j.jpg)

1. In the list of available activities for the pipeline, select ** Databricks** and then drag the **Notebook** element into the pipeline designer.

    !IMAGE[bnu8c8q5.jpg](instructions267349/bnu8c8q5.jpg)

1. In the Details pane that appears below the design canvas, select **Azure Databricks**.

	!IMAGE[tgdpdfkg.jpg](instructions267349/tgdpdfkg.jpg)


1. Select **Databricks linked service** and then select **DevDatabricksLS**.

    !IMAGE[9h4cqtus.jpg](instructions267349/9h4cqtus.jpg)

1. In the Details pane that appears below the design canvas, select **Settings** and then select **Browse**.

    !IMAGE[ou6m00uh.jpg](instructions267349/ou6m00uh.jpg)

1. In the Browse pane, select **Users** and then select **@lab.CloudPortalCredential(User1).Username**.

	!IMAGE[axyrj7wj.jpg](instructions267349/axyrj7wj.jpg)


1. Select **CICD Databricks Demo** and then select **Notebook 2.0 Feature Gallery**. Select **OK**.

	!IMAGE[es87fpof.jpg](instructions267349/es87fpof.jpg)

1. In the Details pane , select **Azure Databricks** tab and then select **Test connection**.

    !IMAGE[phl41flm.jpg](instructions267349/phl41flm.jpg)

1. Select **Save All** and then select **OK**.  

1. Select **Publish All**, then select **OK**.    

>[!note]The Databricks orchestration  is now complete. The code is in the repo and has been thoroughly tested both manually and through ADF orchestration.



1. Return to the browser window which hosts Databricks. In the left navigation pane, select **Workspace**.  

    !IMAGE[qhi85cud.jpg](instructions267349/qhi85cud.jpg)

1. In the center pane of the Databricks page, select **Home** and then select **CICD Demo**. 

	!IMAGE[13mh7j2e.jpg](instructions267349/13mh7j2e.jpg)

1. In the Details pane at the bottom of the page, select **Notebook 2.0 Feature Gallery**.

	!IMAGE[kg1ch7lj.jpg](instructions267349/kg1ch7lj.jpg)

1. In the address bar for the browser, copy the location portion of the URL. The location information appears after **https://** and before **.azuredatabricks.net**. Paste the location into the following text field.

    !IMAGE[wlws8c2r.jpg](instructions267349/wlws8c2r.jpg)

	>[!note] In the screenshot above, the location portion is adb-170428709809524.4

    @lab.TextBox(nbURL)

    `My Notebook URL is @lab.Variable(nbURL)`

1. In the upper-right corner of the Databricks page, select the account profile picture and then select **Settings**.

1. In the **Settings** pane, in the **User** section, select **Developer**.

1. Find the **Access tokens** row and then select **Manage**.

1. Select **Generate new token**. 

1. In the comment field, enter +++Databricks (Dev) to Azure Dev Ops+++, and then select **Generate**.

	!IMAGE[wa15rusm.jpg](instructions267349/wa15rusm.jpg)

1. Copy the token to the Windows Clipboard and then paste the token into the following text field.

	!IMAGE[79wt9usr.jpg](instructions267349/79wt9usr.jpg)

    @lab.TextBox(dbriado)

    `My Databricks (Dev) to Azure Dev Ops is @lab.Variable(dbriado)`

1. Select **Done**.
===
### Task 5: Configure Azure DevOps pipelines

1. Open a new browser window and go to +++htttps://dev.azure.com/User1-@lab.LabInstance.Id+++.

1. Select the **CICD Databricks Demo** project from the project pane.

1. Select **Pipelines** and then select **Create Pipeline**.

1. Select **Use the classic editor**.

1. In the Select a source pane, select **Continue**. 

	!IMAGE[8qbt8sal.jpg](instructions267349/8qbt8sal.jpg)

1. On the **Select a template** pane, select **Empty job**. 

	!IMAGE[pmc0ctn9.jpg](instructions267349/pmc0ctn9.jpg)

1. In the list of tasks, in the **Agent job 1** task, select the **+** sign.

	!IMAGE[2tf6xvlx.jpg](instructions267349/2tf6xvlx.jpg)


1. In the Add task pane, search for and select +++Publish Build Artifacts+++.

	!IMAGE[ytc6vj5w.jpg](instructions267349/ytc6vj5w.jpg)
    
1. Select **Add**. The Publish Artifact: drop task is added to the list of tasks.

1. Select **Publish Artifact: drop**. The Publish build artifacts pane displays. Configure artifact settings by using the following values. Leave all other settigns at default options.

	!IMAGE[f0h0h440.jpg](instructions267349/f0h0h440.jpg)

    | **Parameter**       | **Value**                           |
    |---------------------|-------------------------------------|
    | Display name        | +++Publish Artifact: Dev Project+++ |
    | Path to publish (Select the **ellipsis**, then expand **CICD Databricks Demo** tree)     | **Notebook 2.0 Feature Gallery.py** |
    | Artifact name       | +++Dev Project+++                   |

	!IMAGE[4z40010b.jpg](instructions267349/4z40010b.jpg)


1. On the menu bar for the pipeline, select **Triggers** and then select **Enable continuous integration**.

	!IMAGE[4vjcvalc.jpg](instructions267349/4vjcvalc.jpg)

1. Select  **Save & queue** and then select **Save & queue** again.

1. Select **Save and run**. Wait for the job to report that it was successful.

    !IMAGE[0u4oc5uf.jpg](instructions267349/0u4oc5uf.jpg)

    !IMAGE[muitmh71.jpg](instructions267349/muitmh71.jpg)

<!-- 13. It appears that our pipeline failed due to the agent. Let's quickly configure a local agent. 

14. Right-click **Project settings** in the lower-left corner of the window, select **Open link in new tab** and then navigate to the newly opened tab. 

15. On the **Project Settings** blade under **Pipelines**, select **Agent pools** and then select **Default**.

16. In the upper-right corner, select **New agent**.

17. Select the **Download** button.

18. When the download completes, right-click the **Start** menu and select **Terminal (Admin)**.

19. Run the following code block in the administrative PowerShell session. 


    ```
    #Install the Agent 
    mkdir agent ; cd agent
    Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win-x64-3.240.1.zip", "$PWD")
    .\config.cmd
    ```

20. At the **Enter server URL** prompt, type +++https://dev.azure.com/User1-@lab.LabInstance.Id+++

21. Select **Enter**.

22. Enter +++@lab.Variable(PAT)+++

23. Select **Enter** twice.

24. Select **Enter** again to save your configuration settings.

25. At the **Enter run agent as service?** prompt, type +++y+++ and select **Enter**.

26. Select **Enter** for every prompt until you're returned to the console. 

27. Start the agent by typing +++.\run.cmd+++.

28. Switch back to the web browser and close the **Personal Access Tokens** tab.  

29. In the Azure DevOps pane, select **Pipelines**. 

30. Select the vertical ellipsis on **CICD Databricks Demo-CI** and choose **edit**.

31. Select **Agent job 1**.

32. Under **Agent selection**, set the **Agent pool** dropdown menu to **Default**.

33. Select  **Save & queue**, then select **Save & queue** to continue.

34. Select **Save and run**. 

35. Notice our success.  -->
===
### Task 6: Deploy and manage releases
1. In the left navigation pane on the Databricks page for the project, select **Pipelines** and then select **Releases**.

	!IMAGE[ovl66b8f.jpg](instructions267349/ovl66b8f.jpg)

1. Select **New pipeline**.

1. In the **Select a template** pane, select **Empty job**.

1. On the design canvas, select **Add an artifact**. 

1. In the Add an artifact pane, select **Source**  and then select **CICD Databricks Demo-CI**. Select **Add**.

	!IMAGE[8xnx06y0.jpg](instructions267349/8xnx06y0.jpg)

1. In the Stages section of the design canvas, select <span style="color:blue;">**1 job, 0 task**</span>. The tasks for this stage display.

	!IMAGE[1xzwp5c8.jpg](instructions267349/1xzwp5c8.jpg)

<!-- 6. Select **Agent job 1**.

7. Under **Agent selection**, set the **Agent pool** dropdown menu to **Default**. -->

1. On the Agent job task, select **+**.

1. Enter +++Databricks+++ into the search box.

1. In the list of search results, select **Databricks Script Deployment Task by Data Thirst** and then select **Get it free**. 

	!IMAGE[hc3u2izn.jpg](instructions267349/hc3u2izn.jpg)

1. Select  **Get it free** again. 

    !IMAGE[ikgrd5px.jpg](instructions267349/ikgrd5px.jpg)

1. Select **Install**.

	!IMAGE[8ikf0dxb.jpg](instructions267349/8ikf0dxb.jpg)

    >[!knowledge] The Databricks Script Deployment Task by Data Thirst is used in this pipeline as one of the options available. While this tool is effective for specific scenarios, it is not the only method nor a recommendation over standard tools like REST APIs or Azure DevOps itself. Consider your project requirements and environment when choosing integration tools.

1. Return to the browser window for the new release pipeline. In the **Add tasks** panel select **Refresh**. 

	!IMAGE[rp39kgg9.jpg](instructions267349/rp39kgg9.jpg)

1. Select **Databricks Deploy Notebooks**, then select **Add**.

	!IMAGE[3sgoxq9b.jpg](instructions267349/3sgoxq9b.jpg)

    >[!hint]If **Databricks Deploy Notebooks** isn't visible, wait a few minutes and refresh the task list again. 


1. In the list of tasks, select **Databricks Notebooks deployment**.

    !IMAGE[5ogb7kvn.jpg](instructions267349/5ogb7kvn.jpg)

1. Configure the task by using the values in the following table. Leave all other settings at default values.

 | **Parameter**       | **Value**                           |
    |---------------------|-------------------------------------|
    | Azure Region       | +++@lab.Variable(nbURL)+++ |
    | Source files path  | +++$(System.DefaultWorkingDirectory)/_CICD Databricks Demo-CI/Dev Project+++ |
	| Databricks bearer token       | +++@lab.Variable(dbriado)+++ |
  
  	!IMAGE[szljgezn.jpg](instructions267349/szljgezn.jpg)

1. In the menu bar for the pipeline, select **pipeline** tab.

	!IMAGE[rlba04qq.jpg](instructions267349/rlba04qq.jpg)

1. In the CICD Databricks Demo artifact, select the **Continuous deployment trigger** (the lightning bolt).

    !IMAGE[0yc31h79.jpg](instructions267349/0yc31h79.jpg)

1. Enable the **Continuous deployment trigger** and then close the panel.    

    !IMAGE[2n5k30ry.jpg](instructions267349/2n5k30ry.jpg)

1. On the menu bar for the pipeline, select **Save** then select **OK**. 

1. At the top right of the pipeline page, select **Create release** and then select **Create**.

    !IMAGE[when5ru1.jpg](instructions267349/when5ru1.jpg)

1. On the message bar that displays at the top of the page, right-click **Release- 1** and open the link in a new tab. You will see that the pipeline is in progress.

    !IMAGE[xrukh4el.jpg](instructions267349/xrukh4el.jpg)

    >[!Note]Dismiss the new features dialog if the dialog displays.

1. Wait for the release to show as successful. 

	>[!note]It may take several minutes for the pipeline to complete.

    !IMAGE[2o5gn3bu.jpg](instructions267349/2o5gn3bu.jpg)

1. Return to the browser window that hosts **Databricks**. In the left navigation pane, select **Workspace**.

1. In the center pane of the Databricks page, select **Workspace** and then select **Shared**. The Notebook should appear in the folder.  

    !IMAGE[y1g28mvu.jpg](instructions265985/y1g28mvu.jpg).


===
### Task 7: Deploy the solution to Production

Retrieve the production bearer token.

1. In a new tab, navigate to +++https://portal.azure.com/+++.

1. In the search field, enter +++databricksprod@lab.LabInstance.Id+++ and then select the resource.

1. Select **Launch Workspace**.

1. In the upper-right corner of the page, select the account profile picture and then select **Settings**.

!IMAGE[8funu9ya.jpg](instructions267349/8funu9ya.jpg)

<!-- 1. In the **Settings** blade, under **Workspace admin**, select **Advanced**.

1. Select **Permission settings** in the **Personal Access Tokens** field.

    !IMAGE[pdherzb5.jpg](instructions265985/pdherzb5.jpg)

1. Select **All workspace users** in the dropdown menu, then select **+ Add**.

    !IMAGE[mvnhbero.jpg](instructions265985/mvnhbero.jpg)

1. Select **Save** and then close the **Permission Settings** window. -->


1. In the **Settings** pane, in the **User** section, select **Developer**.

    !IMAGE[onqnmih2.jpg](instructions267349/onqnmih2.jpg)

1. On the **Access tokens** row, select **Manage**, and then select the **Generate new token** button. 

	

1. In the comment field, enter +++Databricks access token (Prod)+++, and then select **Generate**.

	!IMAGE[fz5t7e7g.jpg](instructions267349/fz5t7e7g.jpg)


1. Copy the token to the Windows Clipboard and then paste the token into the following text field.

	!IMAGE[kyhux2o6.jpg](instructions267349/kyhux2o6.jpg)


    @lab.TextBox(pbri)

    `My Prod Databricks token is @lab.Variable(pbri)`

1. Select **Done**.

1. In the address bar for the browser, copy the region portion of the URL. The region information appears after **https://** and before **.azuredatabricks.net**. Paste the location into the following text field.

    !IMAGE[n2r8wli2.jpg](instructions267349/n2r8wli2.jpg)

	>[!note] In the screenshot above, the location portion is adb-4423478022030240.0


1. Copy the region portion of the production Databricks URL into the following text box.

    !IMAGE[smd5f4fd.jpg](instructions265985/smd5f4fd.jpg)


    - I.E. `adb-247546971524660.0`

    @lab.TextBox(prdnbURL)

    `My Production Notebook URL is @lab.Variable(prdnbURL)`

1. Keep this tab for later as we will return to it soon.

1. In a new tab, navigate to +++https://dev.azure.com+++ and select the **CICD Databricks Demo** project.

1. From the Azure DevOps pane, select **Pipelines**, then select **Releases**.

    !IMAGE[hcf9zjhb.jpg](instructions267349/hcf9zjhb.jpg)

1. Next to **New release pipeline**, select **Edit**.

    !IMAGE[ft6mej2z.jpg](instructions265985/ft6mej2z.jpg)

1. On the design surface, select **Stage 1** and then hover your mouse just below the activity. The Add and Clone buttons display. Select **Clone**.

    !IMAGE[vqeo5pbh.jpg](instructions267349/vqeo5pbh.jpg)

1. Select **Copy of Stage 1** and update the Stage name field to +++Push to Production+++. Close the Stage pane.

	!IMAGE[77bmptzy.jpg](instructions267349/77bmptzy.jpg)

1. On the design canvae, in the **Push to Production** stage, select <span style="color:blue;">**1 job, 1 task**</span>.

1. In the list of tasks, select **Databricks Notebooks deployment**.

1. Configure the task by using the following values. Leave all other settings at default values.

    | **Setting**                   | **Parameter**                 |
    |-------------------------------|-------------------------------|
    | Azure Region                  | +++@lab.Variable(prdnbURL)+++ |
    | **Databricks bearer token**   | +++@lab.Variable(pbri)+++     |

	!IMAGE[blcpexeb.jpg](instructions267349/blcpexeb.jpg)

1. Select **Save**, then select **OK**.

1. At the top right of the page, select **Create release** and then select **Create**.

1. 1. On the message bar that displays at the top of the page, right-click **Release- 2** and open the link in a new tab. You will see that the pipeline is in progress. 

	!IMAGE[bkpyvm9n.jpg](instructions267349/bkpyvm9n.jpg)

1. Watch the progress of the pipeline and wait for the status to show as **Successful**.

    !IMAGE[w9wxqzsz.jpg](instructions265985/w9wxqzsz.jpg)




===


# Survey

## Please take a moment to answer these questions. Providing answers to these questions will complete this lab. Your answers will help us understand the effectiveness of the lab and help to improve these labs as a whole.

@lab.ActivityGroup(completionsurvey)




===

#Congratulations! You have completed the **CSU-DAI-300 - Implement CI/CD End to End Deployment for Analytics** lab!
 
Select **End** to end the lab.