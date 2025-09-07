# Survey

## This lab is a technical deep dive on **TechLab: Using Copilots with Various Fabric Workloads**. Before we begin, please take a moment to consider your experience and comfort level with this topic and answer these questions.  

@lab.ActivityGroup(initialsurvey)

===  
@lab.Title
## Overview
This lab is designed to demonstrate the capabilities of Microsoft Fabric and Copilot, offering a high-performance, cloud-native analytics solution pattern. It aims to consolidate a customer's data landscape, enhancing the speed and efficiency of deriving value from data.

Participants will explore how Northwind utilizes Microsoft Fabric's robust features to ingest diverse data sources, generate new items, and extract actionable insights. The lab will guide you through the process of utilizing Copilot in Azure Fabric in a number of different tools.

The lab will cover various Microsoft Fabric workloads in conjunction with Copilot, including:

- Synapse Data Engineering
- Data Factory
- Synapse Data Warehouse
- Power BI

===

## Set up the lab environment

Create a Workspace

1. [] Sign in to @lab.VirtualMachine(Base VM).SelectLink using the username +++@lab.VirtualMachine(Base VM).Username+++ and +++@lab.VirtualMachine(Base VM).Password+++ for the password.

1. [] Open Microsoft Edge (Edge) and go to +++https://app.powerbi.com/+++.

1. [] In the Enter your email window, in the **Email** field, enter +++@lab.CloudPortalCredential(User1).Username+++ and then select **Submit**.  

    !IMAGE[Power BI - Email.png](instructions263929/Power BI - Email.png)

1. [] In the Enter password window, in the **Password** field, enter +++@lab.CloudPortalCredential(User1).Password+++ and then select **Sign in**.  

1. [] Close any windows or boxes that may display.  

1. [] If prompted with the **Microsoft Fabric (Free) licensed assigned** box, select **OK**.  

    !IMAGE[MS-Fabric-License-assigned.png](instructions263929/MS-Fabric-License-assigned.png)

1. []  On the Power BI Home page, on the left menu, select **Workspaces**.   


1. []  On the **Workspaces** blade, select **+ New Workspace**. 

    ![A green and white text Description
    automatically
    generated](instructions263929/image002.png){width="3.55257874015748in"
    height="1.2918471128608924in"}

1. []  On the **Create a workspace** pane, in the **Name** box, enter +++Northwind@lab.LabInstance.Id+++.   

    !IMAGE[Create-a-workspace-1.png](instructions263929/Create-a-workspace-1.png)

1. []  Select **Advanced**. In the **License mode** section, verify that **Fabric capacity** is selected.  

    ![A screenshot of a
    computer Description automatically
    generated](instructions263929/image004.png){width="5.802892607174103in"
    height="6.917632327209099in"}

1. []  Under **Capacity**, verify that a capacity is listed and then select **Apply**. 

1. [] If the **Introducing task flows (preview)** dialog displays, select **Got it**.  

    !IMAGE[Intro-Task-Flows.png](instructions263929/Intro-Task-Flows.png)

===  

## Exercise 1: Ingest a Dataset into Fabric by using Data Pipelines Copilot

### Task 1: Connect to an Open DataSet on Azure Storage 


1. [] Select the **Northwind@lab.LabInstance.Id** Workspace.

1. [] Select **+ New item**.

1. [] On the **New item** pane, scroll down to the **Store data** section and select **Lakehouse**.

	>[!Note] You can also use the **Filter** field to search for +++Lakehouse+++.

1. []  In the **New lakehouse** box, enter +++Northwind_LH@lab.LabInstance.Id+++ and then select **Create**.       

	>[!Alert] Do not select the **Lakehouse schemas** check box.

    !IMAGE[New-Lakehouse.png](instructions263929/New-Lakehouse.png)

1. [] On the **Get data in your lakehouse**  page, select **New Dataflow Gen2**.        

    !IMAGE[New-Data-Gen2.png](instructions263929/New-Data-Gen2.png)

1. [] In the **New Dataflow Gen2** dialog, in the **Name** field, enter +++Northwind Data+++.

1. [ ]  Clear the **Enable Git Integration, deployment pipelines and Public API Scenarios** check box and then select **Create**.

1. []  On the **Home** tab, select **Get data** and then select **More**.  

    !IMAGE[New-Source-More.png](instructions263929/New-Source-More.png)     

1. []  In the **Search** field, enter +++odata+++ and then select **OData**.          

    ![A screenshot of a
    computer Description automatically
    generated](instructions263929/image013.png){width="4.292265966754155in"
    height="3.1150185914260717in"}

1. []  In the **Connect to data source** window, in the **URL** field, enter +++https://services.odata.org/V4/Northwind/Northwind.svc/+++ and then select **Next**.

    !IMAGE[a2.jpg](instructions267328/a2.jpg)

1. [] On the **Choose Data** window, select the **Orders** table, select **Select related tables**, and then select **Create**.

    >[!alert] If related tables are not found select the following tables:
    >
    >- **Customers**
    >- **Employees**
    >- **Order_Details**
    >- **Orders**
                 
    
	![A screenshot of a computer Description automatically generated](instructions263929/image015.png){width="2.771220472440945in" height="7.459374453193351in"}

1. [] Verify that all four tables are listed in the Query pane.

	!IMAGE[9j2mpzi2.jpg](instructions267328/9j2mpzi2.jpg)

===

### Task 2: Ingest the Dataset by using Data Pipelines  

1. [] Select the **Customers** query. Scroll to the right and examine the Country column. Notice the countries include **Argentina** and **Mexico**.   
    
    ![A table with numbers and a number Description
    automatically
    generated](instructions263929/image017.png){width="2.489931102362205in"
    height="3.7921959755030623in"}

1. [] On the **Home** tab of the ribbon, select **Copilot**.

1. [] In the **Copilot** pane, enter +++Only keep data from records where the country is in South America+++ and then select **Send**. Power Query adds a Filter rows step to the query. The dataset only includes customers from South America.  
    
    !IMAGE[t9bfrf96.jpg](instructions267328/t9bfrf96.jpg)

    >[!Note] Due to the nature of Copilot you may end up with differing results. The filter expression that Power Query applies should resemble the following expression:
    > 
    >Table.SelectRows(#"Navigation 1", each List.Contains({"Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador", "Guyana", "Paraguay", "Peru", "Suriname", "Uruguay", "Venezuela"}, [Country]))


    !IMAGE[b9von41g.jpg](instructions267328/b9von41g.jpg)

1. [] In the **Copilot** pane, select **Undo**.   

    ![A screenshot
    of a message Description automatically
    generated](instructions263929/image019.png){width="2.9483278652668417in"
    height="1.7606627296587927in"}

1. [] In the **Copilot** pane, enter +++What is the total number of customers in each country?+++ and then select **Send**.   

    !IMAGE[tdhind70.jpg](instructions267328/tdhind70.jpg)

    >The filter expression that Power Query applies should resemble the following expression:
    > 
    > The desired Applied Step expression is: Table.Group(#"Navigation 1", {"Country"}, {{"Total Customers", each Table.RowCount(_)}})

    !IMAGE[ApppliedStep002.png](instructions266282/ApppliedStep002.png)

1. [] The query outputs a list displaying the number of customers per country.   

    ![A screenshot of a table
    Description automatically
    generated](instructions263929/image021.png){width="2.333659230096238in"
    height="5.198641732283464in"}

1. [] In the **Copilot** pane, select **Undo**.   

    ![A screenshot of a computer Description
    automatically
    generated](instructions263929/image022.png){width="3.0108366141732286in"
    height="1.6356452318460193in"}

1. [] Select the **Order_Details** query. Then, in the **Copilot** pane, enter +++Only keep orders whose quantities are above the median value+++ and select **Send**.  

    ![A screenshot of a computer Description automatically
    generated](instructions263929/image023.png){width="3.167108486439195in"
    height="1.4897911198600176in"}

1. [] The Quantity Column now displays only values above 20.  

    !IMAGE[Order-Details-Quantity.png](instructions263929/Order-Details-Quantity.png)

1. [] On the **Power Query** toolbar, on the **Home** tab, select **Advanced editor**.   

    ![A screenshot of a
    computer Description automatically
    generated](instructions263929/image024.png){width="2.812892607174103in"
    height="1.1459930008748906in"}

1. [] Review the definition of the formula used in the query.   

    ![A screen shot
    of a computer code Description automatically
    generated](instructions263929/image025.png){width="6.268055555555556in"
    height="1.7444444444444445in"}

1. [] Select **Cancel** to exit the Advanced editor without making changes.

1. [] In the **Copilot** pane, select **Undo** to revert the changes.   

    ![A screenshot of a phone Description
    automatically
    generated](instructions263929/image026.png){width="3.0837642169728783in"
    height="1.791917104111986in"}

1. [] In the **Copilot** pane, enter +++Create a new query with data for official public holidays for Australia in 2024+++ and then select **Send**.   
    
    ![A screenshot of a computer error Description
    automatically
    generated](instructions263929/image027.png){width="3.17752624671916in"
    height="1.5210454943132108in"}

1. [] The Australian public holidays are now listed.   

    ![A screenshot of a computer Description
    automatically
    generated](instructions263929/image028.png){width="2.9900010936132984in"
    height="2.2086417322834646in"}

1. [] In the **Copilot** pane, select **Undo**.

1. [] In the **Copilot** pane, enter +++Create a new query with average monthly temperatures for Spain between 2022 and 2025+++, then select **Send**.   

    !IMAGE[vsitrp1n.jpg](instructions267328/vsitrp1n.jpg)

1. [] A table containing the average monthly temperatures for Spain between 2022 and 2025 is displayed.   

    ![A screenshot of a computer Description
    automatically
    generated](instructions263929/image030.png){width="6.268055555555556in"
    height="1.3354166666666667in"}

1. [] In the **Copilot** pane, select **Undo** to revert the changes.   

    ![A screenshot of a chat Description automatically
    generated](instructions263929/image031.png){width="3.1566907261592303in"
    height="1.7398261154855643in"}

1. [] Select the **Orders** query.

1. [] In the **Copilot** pane, enter +++Create a new query named "Value By Delivery Country" showing the order value aggregated by shipCountry+++, then select **Send**.   

    ![A screenshot of a computer
    Description automatically
    generated](instructions263929/image032.png){width="3.1462718722659666in"
    height="1.5835542432195975in"}

1. [] A table containing the shipCountry and order value aggregates is displayed.   

    ![A screenshot of a computer Description
    automatically
    generated](instructions263929/image033.png){width="6.268055555555556in"
    height="4.070138888888889in"}  

1. [] On the **Power Query** toolbar, on the **Home** tab, select **Advanced editor** to verify the correct formula was used.   
    
    ![A screenshot of a computer Description automatically
    generated](instructions263929/image034.png){width="2.8545647419072617in"
    height="1.1459930008748906in"}

1. [] The value of freight is being used, is that what we want? We need to check what Copilot is doing.   
    
    ![A screen shot of a computer
    Description automatically
    generated](instructions263929/image035.png){width="6.268055555555556in"
    height="1.2395833333333333in"}

1. [] Select **Cancel** to close the Advanced editor, then in the **Copilot** pane, select **Undo** to revert the changes.   

    ![A screenshot of a phone Description automatically
    generated](instructions263929/image036.png){width="3.1983628608923884in"
    height="1.8752613735783028in"}

1. [] Select **Publish** to publish the data. Wait until the publishing process completes before proceeding to Exercise 2. 

    ![A screenshot of a computer Description automatically generated](instructions263929/image037.png){width="2.0106977252843397in"
    height="1.2085017497812773in"}  

    !IMAGE[Published.png](instructions263929/Published.png)

===


## Exercise 2: Move and Transform Dataset to a Data Warehouse with Dataflow Gen 2 Copilot

### Task 1: Create a Data Warehouse

1. [] Select the **Northwind@lab.LabInstance.Id** Workspace.

1. [] Select **+ New item**, search for and select +++**Warehouse**+++.

    !IMAGE[a3.jpg](instructions267328/a3.jpg)

1. [] Name the Warehouse +++Northwind Warehouse+++ and then select **Create**.   

    ![A screenshot of a
    video game Description automatically
    generated](instructions263929/image040.png){width="2.8962379702537184in"
    height="1.8544258530183726in"}

1. [] On the **Build a warehouse** page, select **Sample data**. Wait for the data to load.

    !IMAGE[a4.jpg](instructions267328/a4.jpg)
    
    >[!Note]It will take several minutes for the data warehouse to be created and for the sample data to load.   
    >
    ![A screenshot of a loading page Description automatically generated](instructions263929/image041.png){width="2.96916447944007in"
    height="2.448257874015748in"}

1. [] On the toolbar for the **Northwind Data** warehouse, on the **Home** tab, select **Copilot**.  

    ![](instructions263929/image042.png){width="6.268055555555556in"
    height="0.59375in"}

===

### Task 2: Start using Copilot in a Data Warehouse

1. []  In the **Copilot** pane, select **Get started**.   

    <!-- ![A screenshot of a phone Description
    automatically
    generated](instructions263929/image043.png){width="3.406725721784777in"
    height="8.324078083989502in"} -->

1. []  In the query editor, enter +++What can I do in the Fabric query editor?+++, then select **Send**. 

    ![A white rectangle with black text
    Description automatically
    generated](instructions263929/image045.png){width="3.125436351706037in"
    height="1.2189206036745406in"}

1. [] Review the various tasks you can perform across three main views:  

    - Data View  
    - Query View  
    - Model View

1. [] In the query editor, enter +++Show me the first 10 rows of the trip+++, then select **Send**.  

    ![A screenshot of a computer Description automatically generated](instructions263929/image046.png){width="3.1462718722659666in"
    height="1.6460629921259842in"}

1. [] Select **Insert code** to add the query to a new SQL Query window.

	!IMAGE[h9hwj8fe.jpg](instructions267328/h9hwj8fe.jpg)
	
3. [] Review the plan description and the query.   
    
    !IMAGE[7emnnyqg.jpg](instructions267328/7emnnyqg.jpg)

1. [] On the **SQL query 1** menu, select **Run**.   

    ![A screenshot of a computer Description
    automatically
    generated](instructions263929/image048.png){width="6.268055555555556in". 
    height="0.7895833333333333in"}

1. [] The query results pane should display 10 results similar to the following:   

    ![A screenshot of a computer Description
    automatically
    generated](instructions263929/image049.png){width="6.268055555555556in"
    height="2.0805555555555557in"}

===

### Task 3: Create a new Query from Comments

1. []  On the **Home** tab, select **New SQL Query** > **New SQL Query**.

    !IMAGE[New-SQL-Query.png](instructions263929/New-SQL-Query.png)

1. []  In the **SQL query 2** editor, enter the following text:

    ```
    -- Step-by-step plan:
    -- 1. Identify the time range for morning rush hours (7-9 AM), evening rush hours (5-7 PM), and non-rush hours (all others)
    -- 2. Group the trip data into morning and afternoon groups based on the HourNumber.
    -- 3. Calculate the average duration of each trip within the three groups.

    ```

    1. []  At the end of Line 4, select **Enter**, then wait a moment. Copilot adds the suggested query content on Line 5.  

    !IMAGE[SQL-query-select-Enter.png](instructions263929/SQL-query-select-Enter.png)

1. []  Hover over line 5 and then select **Accept** to accept the inserted content.  

    >[!knowledge] You can also select line 5 and then press the **Tab** key to insert the suggested content.

    !IMAGE[Inserted-Content.png](instructions263929/Inserted-Content.png)

1. []  On the **SQL query 2** menu, select **Run** to execute the query.   

    ![A screenshot of a computer Description automatically generated](instructions263929/image052.png){width="2.0315332458442694in"
    height="0.645923009623797in"}

1. []  Review the results.   

    ![A screenshot of a computer Description automatically generated](instructions263929/image053.png){width="6.268055555555556in"
    height="0.9763888888888889in"}

===

### Task 4: Troubleshoot and Explain a query

1. []  On the toolbar for the warehouse, on the **Home** tab, select **New SQL Query** and then select **New SQL query**.  

    ![A screenshot of a computer Description automatically
    generated](instructions263929/image050.png){width="3.6775962379702536in"
    height="3.135854111986002in"}

1. []  In the query editor, enter the following query:

    ```
    SELECT top 50 *

    FROM Date as D, [Time] AS T

    INNER JOIN [Trip] AS Tr ON Tr.[PickupTimeID] = T.[TimeID]

    Where [D].[DateID] = [Trip].[DateID]
    ```

1. []  Run the query.

    >[!alert] The query contains an error. This is by design.

    ![A screenshot of a computer code Description automatically
    generated](instructions263929/image054.png){width="4.208920603674541in"
    height="1.6460629921259842in"}

1. [] On the toolbar for the query, select **Fix query errors**.  

    !IMAGE[a5.jpg](instructions267328/a5.jpg)

1. [] Notice the query has been modified and corrected. The query that you see may differ from the following screenshot.

    ![A screenshot of a computer code Description automatically
    generated](instructions263929/image056.png){width="6.268055555555556in"
    height="1.3451388888888889in"}

    >[!Note] The following query is representative of what you might see:

    ```
    -- Auto-Fix: The table alias 'Trip' is not defined in the FROM clause.
    -- Auto-Fix: The column 'Trip.DateID' should be 'Tr.DateID' based on the schema.
    SELECT TOP 50 *
    FROM [Northwind Warehouse_].[dbo].[Date] AS D, [Northwind Warehouse_].[dbo].[Time] AS T
    INNER JOIN [Northwind Warehouse_].[dbo].[Trip] AS Tr ON
    Tr.[PickupTimeID] = T.[TimeID]
    WHERE D.[DateID] = Tr.[DateID] -- Auto-Fix: Replaced
    [Trip].[DateID] with Tr.[DateID]
    ```

1. []  Select and highlight the contents of the query, then select **Explain**. Documentation is added to explain the query. 
    
    !IMAGE[a6.jpg](instructions267328/a6.jpg)

    ![A screenshot of a computer Description automatically generated](instructions263929/image058.png){width="6.268055555555556in"
    height="2.785416666666667in"}

===

### Task 5: Build a Query by using Natural Language

1. []  In the **Copilot** pane, enter +++Create a query bringing data together from the geography, weather, date and trip tables showing information for trips on the 20th of September 2013+++, then select **Send**.   

    ![A screenshot of a computer Description automatically generated](instructions263929/image059.png){width="3.1983628608923884in"
    height="1.6252263779527558in"}

1. [] Select **Insert code** to add the query to a new query window.

1. [ ] Examine the query and then select **Run**.   

    ![A screenshot of a computer Description automatically generated](instructions263929/image060.png){width="6.268055555555556in"
    height="1.6159722222222221in"}

    >[!Note]The query results do not include the Year, Month, or Date. The following query is an example of a working query:
    >
    > ```SELECT     [D].[Date] AS TripDate,    [G].[City] AS PickupCity,    [G].[State] AS PickupState,    [W].[AvgTemperatureFahrenheit] AS AverageTemperature,    [W].[PrecipitationInches] AS Precipitation,    [T].[PassengerCount],    [T].[TripDistanceMiles],    [T].[TotalAmount]FROM     [Northwind Warehouse].[dbo].[Trip] AS [T]JOIN     [Northwind Warehouse].[dbo].[Date] AS [D] ON [T].[DateID] = [D].[DateID]JOIN     [Northwind Warehouse].[dbo].[Geography] AS [G] ON [T].[PickupGeographyID] = [G].[GeographyID]JOIN     [Northwind Warehouse].[dbo].[Weather] AS [W] ON [D].[DateID] = [W].[DateID]WHERE     [D].[Date] = '2013-09-20';```

1. [] In the **Copilot** pane, enter +++Make sure to display the year, month and date in the results+++, then select **Send**.   

    ![A screenshot of a computer screen Description automatically generated](instructions263929/image061.png){width="3.1045997375328085in"
    height="1.2293383639545057in"}

    >[!Note]Due to the nature of Copilot, results and action may differ. Changes may be made in the current query or a new query maybe created. You may see Copilot will generate a new SQL query that is only partially written.  

    ![A screenshot of a computer program Description automatically generated](instructions263929/image062.png){width="6.268055555555556in"
    height="3.270138888888889in"}

1. [] Enter the following lines of code into the previous query after the keyword **Select**.   

    ![A screenshot of a computer Description automatically generated](instructions263929/image063.png){width="6.268055555555556in"
    height="1.882638888888889in"}

    ```
    [D1].[Year] AS [Year],
    [D1].[Month] AS [Month],
    [D1].[Date] AS [Date],
    ```

1. [] Run the query and notice the new fields that appear. 

    ![A screenshot of a computer Description automatically generated](instructions263929/image064.png){width="6.268055555555556in"
    height="2.5347222222222223in"}

===

## Exercise 3: Move Dataset(s) to Lakehouse via Notebook and Copilot

### Task 1: Prepare Copilot

1. []  Open the **Northwind_LH@lab.LabInstance.Id** lakehouse.

1. []  On the **Ribbon**, select **Open Notebook** and then select **New Notebook**.  

    ![A screenshot of a computer Description automatically
    generated](instructions263929/image065.png){width="6.268055555555556in"
    height="1.1715277777777777in"}

1. []  Rename the new Notebook +++cp_Note+++. 

    ![A screenshot of a computer Description automatically generated](instructions263929/image066.png){width="3.5317432195975504in"
    height="2.073206474190726in"}

1. [] On the menu bar, select **Connect** and then select **New standard session**.

	!IMAGE[v14wxq58.jpg](instructions267328/v14wxq58.jpg)

1. [] On the **Home** tab of the ribbon, select **Copilot**.  

    ![](instructions263929/image067.png){width="6.268055555555556in"height="0.7333333333333333in"}

1. []  On the **Copilot** pane, select **Get Started**. 

    ![A screenshot of a cell phone Description automatically generated](instructions263929/image068.png){width="3.6255063429571304in"
    height="5.709130577427821in"}

===

### Task 2: Use Copilot for code Generation

1. []  In the **Copilot** pane, enter +++Load Customers into a DataFrame+++, then select **Send**.   

    ![A screenshot of a computer error Description automatically generated](instructions263929/image071.png){width="3.2608716097987753in"
    height="1.8752613735783028in"}

1. [] Select **Insert Code** to insert the code into a cell.

    !IMAGE[a30.jpg](instructions266282/a30.jpg)

    >[!Alert] In some cases the **Insert code** button does not appear. If this happens, copy the code in the Copilot pane and then paste the code at line 3 in the notebook cell.

    >[!Note] Due to the nature of Copilot, the object may be named **df** or **result**, or some other value. Copilot may generate only the code required to load the DataFrame. Copilot may also add code to show the schema for the DataFrame and to show the data that it loaded.



    !IMAGE[nx9poz1n.jpg](instructions267328/nx9poz1n.jpg)

1. []  Select **Run cell** to execute the query. If Copilot did not add code to display the data, perform steps 4 and 5.

    !IMAGE[e3fa6u0u.jpg](instructions267328/e3fa6u0u.jpg)

1. []  To see the results, in the **Copilot** pane, enter +++show the result as table+++, then select **Send**.  
    
    ![A screenshot of a computer Description automatically
    generated](instructions263929/image073.png){width="3.1879451006124233in"
    height="1.5731364829396326in"}

1. []  Select **Insert Code** to enter the following new code into a cell, then select **Run cell**.   

    ![A screenshot of a computer program Description automatically generated](instructions266282/dfShow.png){width="2.0106977252843397in"
    height="1.510627734033246in"}

    <!-- 6.  Run the new cell by choosing the run icon next to the cell.  

    ![](instructions263929/image075.png){width="6.268055555555556in" height="0.725in"} -->

===

### Task 3: Use Copilot for code completion and to debug

1. []  At the bottom of the Notebook, select **+ Code**.  

	>[!Alert] You may need to hover over the existing code cell to see the **+ Code** option.

    ![A white background with a black and red line Description automatically generated with medium confidence](instructions263929/image076.png){width="6.268055555555556in"
    height="1.073611111111111in"}

1. [] In the new code cell, enter +++%%chat+++ on Line 1. Then, enter +++What are some visuals that may explain this dataset better?+++ on Line 2 and select **Run cell**.   
    
    ![A screenshot of a computer Description automatically
    generated](instructions263929/image077.png){width="6.268055555555556in"
    height="2.275in"}

    >[!Note]Review the type of visuals recommended.

1. []  Select **+ Code**.

1. []  In the new code cell, enter +++%%code+++ on Line 1. Then, enter+++Create a world cloud of Company Names+++ on Line 2 and select **Run cell**.   

    ![A screenshot of a computer program Description automatically generated](instructions263929/image078.png){width="6.268055555555556in"
    height="2.7680555555555557in"}

    >[!Alert]Fabric creates a ready to run code cell ready to be run. When you run the cell an error displays. This is expected. You will resolve the error in the following steps."

1. []  Select **+ Code**.  

1. [] On the Line 1, enter +++%%chat+++. On the Line 2, enter +++What does this error mean ?+++. Then select **Run cell**.   
    
    ![A screenshot of a computer Description automatically generated](instructions263929/image079.png){width="6.268055555555556in"
    height="1.0465277777777777in"}

1. []  Review the result.   

    ![A close-up of a text Description automatically enerated](instructions263929/image080.png){width="6.268055555555556in"
    height="1.3041666666666667in"}

    >[!Note] Due to the nature of Copilot, there may be variances in the exact message. The following steps will solve the issue.

1. []  Select **+ Code**.  

1. [] On Line 1, enter +++pip install wordcloud+++ and then select **Run cell**. 
    
    ![](instructions263929/image081.png){width="6.268055555555556in"
    height="0.5451388888888888in"}

1. [] Rerun the cell that attempts to make the WordCloud.   

    ![A screenshot of a computer code Description automatically generated](instructions263929/image082.png){width="6.268055555555556in"
    height="1.625in"}

    >[!Alert]There will be an error. Line 5 is looking for a dataframe called **df**, while ours is called **result**.

1. [] Enter the following code to update the cell:  

    ```
    from wordcloud import WordCloud
    import matplotlib.pyplot as plt
    result = spark.sql(f"SELECT * FROM northwind_lh1.Customers LIMIT 10")
    company_names_text = ''.join(result.select('CompanyName').rdd.flatMap(lambda x: x).collect())

    wordcloud = WordCloud(width=800, height=400, background_color='white').generate(company_names_text)

    plt.figure(figsize=(10, 6))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis("off")
    plt.show()
    ```

1. [] Select **Run cell**.

   ![A screenshot of a computer Description automatically generated](instructions263929/image083.png){width="6.268055555555556in" height="3.5340277777777778in"}

    >[!Note]The comments are no longer in the cell.

===

### Task 4: Use Copilot for code documentation

1. [] At the top of the cell that creates the word cloud, enter +++%%add_comments+++ and then select **Run cell**.  

    !IMAGE[8nrffogk.jpg](instructions267328/8nrffogk.jpg)

    >[!Note]The comments have returned.

1. [] Select **+ Code**.  

1. [] On Line 1, enter +++pandas_df.describe()+++ and then select **Run cell**. This code provides descriptive statistics for the columns in the dataframe.

    !IMAGE[elyb32hi.jpg](instructions267328/elyb32hi.jpg) 

===

## Exercise 4: Reporting with Power BI Copilot

### Task 1: Create a Report with Power BI Copilot using data from the Data warehouse and Lakehouse

1. [] In the left menu, select the **Northwind@lab.LabInstance.Id** lLakehouse. 

    !IMAGE[Northwind-Workspace.png](instructions263929/Northwind-Workspace.png)

1. [] On the menu to the left of the **Share** button, select **SQL analytics endpoint**.   

    !IMAGE[SQL-analytics-endpoint.png](instructions263929/SQL-analytics-endpoint.png)

1. [] On the Ribbon menu, select **Reporting** > **New report**.  

    !IMAGE[a7.jpg](instructions267328/a7.jpg)

1. [] In the **New report with all available data** box, select **Continue**.  

    !IMAGE[New-report-with-all-available-data.png](instructions263929/New-report-with-all-available-data.png)

1. []  On the **Ribbon** menu, select **Copilot**.
    
    !IMAGE[Power-BI-Copilot.png](instructions263929/Power-BI-Copilot.png)
    
1. []  In the **Copilot** pane select **Get started**. Then, in the prompt field, enter the following text and select **Send**: 

    ```Copilot-wrap
    Create a new report page named "Top 10 products sold by employees and to customers" summarizing the top 10 products sold by employees and to customers
    ```
    
1. []  Examine the new report page. Understand that a vague prompt leads to interesting results.  

    !IMAGE[Report.png](instructions263929/Report.png)

1. []  In the **Copilot** pane, enter the following and then select **Send**:   
 
    ```Copilot-wrap
    Create a new report page named "Count of Customers by Country" showing a count of customers by country as a heat map
    ```

    >![A screenshot of a computer screen Description automatically generated](instructions263929/image095.png){width="6.268055555555556in"
    height="5.570833333333334in"}  
    >
    >[!Note]The results are much better than the previous results.

===

### Task 2: Create a narrative by using Power BI Copilot 

1. []  At the bottom of the screen, select **+**, to add a new report.  

    !IMAGE[New-Report-2.png](instructions263929/New-Report-2.png)

1. [] In the **Visualizations** pane, select **Table**.

	!IMAGE[4a00hhnb.jpg](instructions267328/4a00hhnb.jpg)

3. [] In the **Data** pane, expand the **Orders** table and select the following fields:

    a.  From Orders, select **OrderID**  

    b.  From Orders, select **ShipCountry**

    b.  From Orders, select **ShipCity**

   
    !IMAGE[ly54uaxy.jpg](instructions267328/ly54uaxy.jpg)

1. []  Your visualization should look like the following table.  

    ![A screenshot of a computer Description automatically generated](instructions266282/DataOut1.png){width="4.729827209098863in"
    height="5.407004593175853in"}

1. []  Select an empty area on the report and add a Narrative visualization.
         
    ![A screenshot of a computer Description automatically generated](instructions263929/image099.png){width="2.239896106736658in"
    height="3.5629975940507435in"}  
    
    >[!Alert]If your table changes, it means you still had it selected when you add the visualization. Just undo the
    last change and try again.

1. []  Select **Copilot** for the Narrative type.   

    ![A screenshot of a computer Description automatically generated](instructions263929/image100.png){width="3.8963768591426073in"
    height="2.906655730533683in"}

1. []  Select each of the options presented and notice how they are added to the prompt, then select **Update**.  

    >[!Note]We are looking at only visuals referenced on the current page.  
    
    ![A screenshot of a computer screen Description automatically generated](instructions263929/image101.png){width="5.417422353455818in"
    height="3.9276312335958004in"}

1. []  Move the Narrative to the right-hand side of the screen so that you can see more easily

1. []  Notice the insights and references to support those insights.
    
    ![](instructions263929/image102.png){width="6.268055555555556in"
    height="4.044444444444444in"}

1. []  Change the **Reference Visuals** to include visuals from another page **Customer Count By Country** and, then select **Update**. 
    
    ![A screenshot of a computer Description automatically generated](instructions263929/image103.png){width="5.37575021872266in"
    height="2.2919860017497813in"}

    >[!Note]The Narrative has now included the additional data.

1. [] Save the report as +++Executive Overview+++.   

    ![A long line with a hand holding a key Description automatically generated with medium confidence](instructions263929/image104.png){width="6.268055555555556in" height="0.9256944444444445in"}

===
>[!Alert] ##**IMPORTANT**
**REPORTING NOTE:** These labs are hosted on the Skillable platform. Completion data is collected by CSU and then exported to Success Factors every Monday. SF require another 1-3 days to process that data. The status for this Lab will be visible in Viva and Learning Paths next week.
>
>Be sure to select **End Lab** on the next page, to get credit for completing this lab.



---
@lab.ActivityGroup(completionsurvey)    

>[!Alert] Please select **Submit your responses**, then select **Next** to proceed.
=== 
Congratulations!

You've successfully completed the lab. Select **End** to mark the lab as **Complete**.


