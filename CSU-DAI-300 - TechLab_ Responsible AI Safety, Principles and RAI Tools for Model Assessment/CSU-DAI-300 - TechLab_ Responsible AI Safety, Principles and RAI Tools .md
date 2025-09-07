# Survey

@lab.ActivityGroup(initialsurvey)

===
## Welcome to the Responsible AI lab environment. 

In this session you will gain a foundational understanding of responsible AI principles through theory and practical exercises, including hands-on evaluations and red-teaming scenarios. 

### Lab Setup Guide
1. Navigate to your Desktop.
2. Clone the RAI Lab repository. 
    1. Go to the start menu and open Git CMD.
    2. In terminal window type `cd Desktop`. This will navigate to the desktop.
    3. Enter the command: `git clone https://github.com/Tech-Connect-RAI/LAB112.git` 

    	>[!NOTE] *This will download the lab files from a remote location and create a local copy on your machine.*
    
    4. Close the terminal and go to the desktop. 
    5. Open the Visual Studio Code application on your desktop. When the program opens, perform the following:
        
        1. Click on the "File" menu located at the top-left corner of the window.
        2. From the dropdown menu, select "Open Folder..."
        3. A file explorer window will appear. Navigate to the Desktop. 
        4. Select the **LAB112**, and click the "Select Folder" button.

3. Next, configure the environment variables.

    **Configure the environment variables**
    1. **Open the .env File:**
        - Open the **.env** file. We will populate it with the necessary data from Azure.
    2. **Sign In to Azure Portal:**
        - Navigate to +++https://portal.azure.com/+++ and sign in using the Azure account credentials provided in the lab instructions.
    3. **Navigate to the Azure OpenAI Resource:**
        -  On the left side of the page, you will see a navigation menu. Click the menu button. 
        - In this menu, look for the **All resources** link. Click on **All resources** from the droplist to access the page listing all your resources.
        - Then, use the search bar to filter for +++Azure OpenAI+++, and click on the Azure OpenAI resource that's displayed.

    4. **Access the Keys:**
        - On the resource's overview page, click on **Manage keys**.
        - This will redirect you to a page containing the required API keys and endpoint for the lab.

    5. **Copy and Paste API Key & Endpoint:**
        - Copy the value of **Key 1**.
        - Open the **.env** file and paste this value next to **AZURE_OPENAI_API_KEY**.
        - Copy the value of **Endpoint**.
        - In the **.env** file, paste this value next to **AZURE_OPENAI_ENDPOINT**.
    
        >[!NOTE] Ensure that environment variables are added in the format KEY=VALUE. For example:
        ```bash
        AZURE_OPENAI_ENDPOINT=https://aoai123.openai.azure.com/
        AZURE_OPENAI_API_KEY=1234567890
        ```

    7. Save the **.env** file:

<br>
<hr>
### Let's begin the lab!

#### Lab 1: Evaluate bias and fairness in AI systems 
**Objective:** 
This session introduces articipants to various methods to assess and mitigate bias and fairness issues in machine learning models using Fairlearn in Azure Machine Learning. By the end of this session, you will understand how to apply RAI tools to proactively evaluate bias and fairness in AI systems.

**Key topics**
- Load and preprocess a dataset.
- Train a simple machine learning model.
- Fairness assessment of unmitigated model
- Define fairness metrics based on the anticipated harms
- Evaluate the fairness of the model.
- Mitigating Unfairness in ML models

#### Open the **Lab 01** folder to start this lab.

**Step-by-Step Guidance**
1. Load the dataset containing credit loan outcomes.
2. Preprocess the data by handling missing values, encoding categorical variables, and splitting the data into training and testing sets.
3. Train a fairness-unaware model to predict loan defaults using the training data.
4. Evaluate the model's performance using standard metrics such as accuracy, precision, recall, and ROC-AUC.
5. Use the Fairlearn toolkit to assess the fairness of the model.
6. Calculate fairness metrics such as equalized odds difference, false negative rate, false positive rate, and selection rate.
7. Identify any disparities in the model's performance across different demographic groups.
8. Apply fairness mitigation techniques using the Fairlearn toolkit.
9. Implement methods such as ThresholdOptimizer and ExponentiatedGradient to reduce unfairness.
10. Re-evaluate the model's performance and fairness metrics after applying mitigation techniques.
11. Compare the performance and fairness metrics of the original and mitigated models.
12. Reflect on the ethical considerations and importance of fairness in credit loan decisions.

**Example Notebook:**
Please refer to the notebook provided in the Lab Environment Folder for detailed code, analysis, and observations.


<br>
#### Lab 2: Red teaming GenAI applications for RAI
**Objective:** This session introduces participants to various red teaming activities to identify potential weaknesses in AI systems. By the end of this session, you will understand the importance of Red Teaming for RAI and various tools and exercises to evaluate GenAI applications.

**Key topics**
- Understand the fundamentals of red teaming and its importance in AI safety and security.
- Identify potential biases and vulnerabilities in AI systems.
- Improve the robustness and reliability of AI models through rigorous testing.

Open the **Lab 02** folder to start this lab.

===
@lab.ActivityGroup(completionsurvey)
---

>[!note] Please select **Submit your responses**, then select **Next** to proceed.

=== 
Congratulations!

You've successfully completed the lab. Select **End** to mark the lab as **Complete**.


