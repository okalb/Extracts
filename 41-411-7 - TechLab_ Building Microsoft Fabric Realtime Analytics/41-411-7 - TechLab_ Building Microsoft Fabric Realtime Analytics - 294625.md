@lab.Title

This lab is a technical deep dive on MS Fabric - Realtime Intelligence. Before we begin, please take a moment to consider your experience and comfort level with this topic and answer these questions. Thank you!

@lab.ActivityGroup(initialsurvey)

===
#### **Before you begin**

Before you begin, if you are new to this lab platform, please take a minute to review the quick tips for navigating the lab instructions.

+++Select the Type Text icon to insert the green text directly into the Challenge Lab environment.+++

>[!alert] An Alert tells you that a task requires extra care.

>[!note] A Note provides additional helpful information for completing a task.

>[!hint] A Hint will guide you through a portion of the lab.

>[!knowledge] A Knowledge block provides a deeper level of knowledge into a subject. It is a great way to solidify your understanding, but it is not strictly necessary to complete the lab.

>At the top right on the Burger menu there is an option to **End** the lab if you either completed it or do not wish to complete it.





===

# Introduction

Suppose you own an e-commerce website selling bike accessories. You have millions of visitors a month, you want to analyze the website traffic, consumer patterns, and predict sales.  

This workshop will walk you through the process of building an end-to-end [Real-Time Intelligence](https://blog.fabric.microsoft.com/en-us/blog/introducing-real-time-intelligence-in-microsoft-fabric) solution for your e-commerce website by using Microsoft Fabric and the medallion architecture.  

You will learn how to:
- Build a medallion architecture in Fabric Real-Time Intelligence. 
- Use Fabric shortcuts & Data Factory pipelines to get data from operational DBs like SQL Server (with AdventureWorksLT sample data).
- Stream events into Fabric Eventhouse via Eventstream & leverage OneLake availability.
- Create real-time data transformations in Fabric Eventhouse through the power of Kusto Query Language (KQL) & Fabric Copilot.
- Create real-time visualizations using Real-Time Dashboards and automate actions.

See what real customers like [McLaren](https://www.linkedin.com/posts/shahdevang_if-you-missed-flavien-daussys-story-at-build-activity-7199013652681633792-3hdp), [Dener Motorsports](https://customers.microsoft.com/en-us/story/1751743814947802722-dener-motorsport-producose-ltd-azure-service-fabric-other-en-brazil), [Elcome](https://customers.microsoft.com/en-us/story/1770346240728000716-elcome-microsoft-copilot-consumer-goods-en-united-arab-emirates), [Seair Exim Solutions](https://customers.microsoft.com/en-us/story/1751967961979695913-seair-power-bi-professional-services-en-india) & [One NZ](https://customers.microsoft.com/en-us/story/1736247733970863057-onenz-powerbi-telecommunications-en-new-zealand) are saying.

All the **code** in this tutorial can be found here:   
[Building a Medallion Architecture on Fabric Real-Time Intelligence](https://github.com/microsoft/FabricRTIWorkshop/)  

Also, here's a detailed [article](https://techcommunity.microsoft.com/t5/startups-at-microsoft/building-a-real-time-medallion-architecture-using-eventhouse-in/ba-p/4110686) explaining this tutorial.

### Duration
- Lab 1-2 hours (section 8)
- Theoretical context 30-45 minutes (sections 1-6) or optional [presentation](https://github.com/microsoft/FabricRTIWorkshop/blob/main/presentation/_Real-Time%20Intelligence%20in%20Fabric%20L200%20pitch%20deck.PDF).
- Lab [pre-reqs](https://moaw.dev/workshop/?src=gh%3Amicrosoft%2FFabricRTIWorkshop%2Fmain%2Fdocs%2F&step=6) 30-45 minutes (section 7, recommend provisioning trial tenant prior if necessary)

### Authors
- [Denise Schlesinger](https://github.com/denisa-ms), Microsoft, Prin CSA
- [Hiram Fleitas](https://aka.ms/hiram), Microsoft, Sr CSA
- Guy Yehudy, Microsoft, Prin PM

### Feedback - Contributing
- Rate this lab or give us feedback to improve using this short [Eval](https://forms.office.com/r/xhW3GAtAhi). Scan this QR Code to open the Eval form on your phone.

    !IMAGE[QR_Code.jpg](instructions294573/QR_Code.jpg)

- If you'd like to contribute to this lab, report a bug or issue, please feel free to submit a Pull-Request to the [GitHub repo](https://github.com/microsoft/FabricRTIWorkshop/) for us to review or [submit Issues](https://github.com/microsoft/FabricRTIWorkshop/issues) you encounter.

===

# What is the Medallion Architecture?

The inspiration for this tutorial was the realization that **Eventhouse** aligns perfectly with the Medallion's architecture requirements. 

The Medallion architecture is a data design pattern with 3 layers:

&#129353; The Bronze layer: containing raw data being streamed into a data platform, these are **Eventhouse** continuous ingestion capabilities.  

&#129352; The Silver Layer: a curated enriched layer of data, based on transformed data from the Bronze layer. This can be achieved with Eventhouse's **update policies**.   

&#129351; The Gold Layer: aggregated data for reporting and BI, this can be achieved with Eventhouse's **materialized views**.  

A medallion architecture (also coined by Databricks) is a data design pattern used to logically organize data. The goal is to **incrementally** improve the structure and quality of data as it flows through each layer of the architecture. Medallion architectures are sometimes also referred to as "multi-hop" architectures.

Creating a multi-layer data platform allow companies to improve data quality across the layers and at the same time provide for their business needs. Unstructured and raw data are ingested using scalable pipelines to output the highest quality enriched data.

Reference: [Data Engineering Wiki](https://dataengineering.wiki/Concepts/Medallion+Architecture)

In summary, Microsoft Fabric [Real-Time Intelligence (RTI)](https://aka.ms/fabric-docs-rta) features benefit building a medallion architecture. They provide minimal latency for data in-motion, automatic light-weight transformations, dashboards, copilots to help you derive insights in a no-code experience, and allow you to take actions in real-time over your data. Additionally, all data is made avaiable via OneLake in delta-parquet format to Lakehouses using OneLake Availability as OneLogical copy for all your data. 
!IMAGE[Opportunity accross industries](instructions294573/Industries.png)



===

# Fabric Real-Time Intelligence features 

Let's cover the key-features and how we plan to use them for our architecture.

### Event streams
- Clicks and Impressions events are ingested from an Eventstream into the `events` table. This feature allows us to bring real-time events (including Kakfa endpoints) into Fabric, transform them, and then route them to various destinations wihtout writing any code (no-code). Enhanced capabilities allows us to source data into Eventstreams from Azure Event Hubs, IoT Hubs, Azure SQL Database (CDC), PostgreSQL Database (CDC), MySQL Database (CDC), Azure Cosmos DB (CDC), Google Cloud Pub/Sub, Amazon Kinesis Data Streams, Confluent Cloud Kafka, Azure Blog Storage events, Fabric Workspace Item events, Sample data or Custom endpoint (Custom App).
- Feature [documentation](https://learn.microsoft.com/fabric/real-time-analytics/event-streams/overview).

### Copilot
- Copilot for Real-Time Intelligence is an advanced AI tool designed to help you explore your data and extract valuable insights. You can input questions about your data, which are then automatically translated into Kusto Query Language (KQL) queries. Copilot streamlines the process of analyzing data for both experienced KQL users and citizen data scientists.
- Feature [documentation](https://learn.microsoft.com/fabric/get-started/copilot-real-time-intelligence)
!IMAGE[Fabric Copilot in KQL Queryset](instructions294573/copilot.png)


### Data pipelines 
- Bronze layer tables are populated by a Data Factory pipeline to copy data from our operational SQL DB.
- Feature [documentation](https://learn.microsoft.com/fabric/data-factory/tutorial-end-to-end-pipeline).

### Shortcuts
- **Product** and **ProductCategory** SQL tables are defined as external tables (Fabric shortcuts). Meaning the data is not copied but served from the SQL DB itself. Shortcuts allow data to remain stored in outside of Fabric like in our operational SQL DB, yet presented in Fabric as a central location.
- Shortcuts enable us to create live connections between OneLake and existing target data sources, whether internal or external to Azure. This allows us to retrieve data from these locations as if they were seamlessly integrated into Microsoft Fabric.
- A shortcut is a schema entity that references data stored external to a KQL database in your cluster. In Lakehouses, Eventhouses, or KQL Databases it's possible to create shortcuts referencing Internal locations within Microsoft Fabric, ADLS Gen2, Spark Notebooks, AWS S3 storage accounts, or Microsoft Dataverse.
- From my perspective, I value the fact that all data is aligned under a unified namespace, allowing seamless access through the same ADLS Gen2 APIs, even when sourced from AWS S3. By enabling us to reference different storage locations, OneLake's Shortcuts provides a unified source of truth for all our data within the Microsoft Fabric environment and ensures clarity regarding the origin of our data.
- Feature [documentation](https://learn.microsoft.com/fabric/real-time-analytics/onelake-shortcuts?tabs=onelake-shortcut).

### Eventhouse
- The Eventhouse can host multiple KQL Databases for easier management. It will store relational data from an operational SQL DB, levergage shortcuts and automate transformations in real-time.
- The Eventhouse is the best place to store streaming data in Fabric. It provides a highly-scalable analytics system with built-in Machine Learning capabilities for discrete analytics over high-granular data. It's useful for any scenario that includes event-based data, for example, telemetry and log data, time series and IoT data, security and compliance logs, or financial records. The Eventhouse supports Kusto Query Languanguage (KQL) queries, T-SQL queries and Python. The data is automatically made available in delta-parquet format and can be easily accessed from Notebooks for mroe advanced transformaitons. The Eventhouse is **specifically tailored** to time-based, streaming or batch events with structured, semistructured, and unstructured data.
- Feature [documentation](https://learn.microsoft.com/fabric/real-time-intelligence/eventhouse).

### KQL Update policies
- This feature is also known as a mini-ETL. Update policies are automation mechanisms triggered when new data is written to a table. They eliminate the need for special orchestration by running a query to transform the ingested data and save the result to a destination table. Multiple update policies can be defined on a single table, allowing for different transformations and saving data to multiple tables simultaneously. **Target** tables can have a different schema, retention policy, and other policies than the **Source** table. The data in derived silver layer tables (targets) of our medallion architecture is inserted upon ingestion to bronze tables (sources). Based on Kusto's update policy feature, this allows to append transformed rows in real-time to a target table as data is landing in a source table and can also be set to run in a transaction. Meaning if the data from bronze fails to be transformed to silver, it will not be loaded to bronze either, by default this is set to off allowing maximum throughput.
- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/management/update-policy).

### KQL Materialized Views
- Materialized views expose an aggregation query over a source table, or over another materialized view. We will use materialized views to create the Gold Layer in our medallion architecture. Most common materialized views provide the current reading of a metric or statistics of metrics over time. They can also be backfilled with historial data; however, by default they are automatically populated by newly ingested data.
- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/management/materialized-views/materialized-view-overview).

### One Logical Copy
- Creates a one logical copy of KQL Database data by turning on OneLake availability. Turning on OneLake availability for your KQL tables, database or Eventhouse means that you can query the data in your KQL database in Delta Lake format via other Fabric engines such as Direct Lake mode in Power BI, Warehouse, Lakehouse, Notebooks, and more. When activated, it will copy via mirroring the KQL data to your Fabric Datalake in delta-parquet format. Allowing you to shortcut tables from your KQL Database via OneLake to your Fabric Lakehouse, Data Warehouse, and also query the data in delta-parquet format using Spark Notebooks or the SQL-endpoint of the Lakehouse.
- Feature [documentation](https://learn.microsoft.com/fabric/real-time-analytics/one-logical-copy).

### KQL Dynamic fields
- Dynamic fields are a powerful feature of Eventhouse / KQL DB that support evolving schema changes and object polymorphism, allowing to store different event types that have a common denominator of base fields.
- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/query/scalar-data-types/dynamic).

### Kusto Query Language (KQL)
- KQL commands will be automatically written by the Get Data UI wizard when configuring the Eventhouse KQL Database destination in Eventstream. The commands will create the `events` table and JSON mapping. Secondly, the control commands will be issued in a database script that automate creation of additional schema items such as Tables, Shortcuts, Functions, Policies and Materialized-Views.
- KQL is also known as the language of the cloud. It's available in many other servies such as Microsoft Sentinel, Azure Monitor, Azure Resource Graph and Microsoft Defender. The code-name **Kusto** engine was invented by 4 engineers from the Power BI team over 10 years ago and has been implemented across all Microsoft services including Github Copilot, LinkedIn, Azure, Office 365, and XBOX.
- KQL queries are easy to write, read and edit. The language is most commonly used to analyze logs, sign-on events, application traces, diagnostics, signals, metrics and much more. Supports multi-statement queries, relational operators such as filters (where clauses), union, joins aggregations to produce a tabular output. It allows the ability to simply pipe (|) additional commands for ad-hoc analytics without needing to re-write entire queries. It has similaries to PowerShell, Excel functions, LINQ, function SQL, and OS Shell (Bash). It supports DML statements, DDL statements (referred to as Control Commands), built-in machine learning operators for forecasting & anomaly dectection, plus more... including in-line Python & R-Lang.
- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/query/).

### Real-time Dashboards
- Will contain a collection of visual tiles _Click Through Rate_ stat KPIs, _Impressions_ area chart, _Clicks_ area chart, _Impressions by Location_ map for geo-spatial analytics and _Average Page Load Time_ in a line chart. This feature support filter parameters, additional pages, markdown tiles, inlcuding Plotly, multiple KQL datasources, base queries, embedding. Supports sharing with permissions controls, setting an Alert by leveraging Data Activator for actions, and automatic refresh with a minimum frequency of 30 seconds. These dashboards are commonly used for Operations and Power BI is commonly used for Business Intelligence. Power BI supports more advanced visualizations and rich data-story capabilities. Real-time Dashboards refresh very fast and allow with ease to togle between visual analytist to pro-developer that can explore queries or edit without needing to download a desktop tool. They make the experience simpler for analysts to visualize over high-granular data.
- Feature [documentation](https://learn.microsoft.com/fabric/real-time-intelligence/dashboard-real-time-create).

### Data Activator
- We will Set an Alert in our Real-time Dashboard to message me in Teams. Data Activator (code-name Reflex) is a no-code experience in Microsoft Fabric for automatically taking actions when patterns or conditions are detected in changing data. It monitors data in Power BI reports, Eventstreams items and Real-time Dashboards, for when the data hits certain thresholds or matches other patterns. It then automatically takes appropriate action such as alerting users or kicking off Power Automate workflows.
- Some common use cases are:
  - Run Ads when same-store sales decline.
  - Alert store managers to move food from failing freezers before it spoils.
  - Retain customers who had a bad experience by tracking their journey through apps, websites etc.
  - Help logistics companies find lost shipments proactively by starting an investigation when package status isn't updated for a certain length of time.
  - Alert account teams when customers fall behind with conditional threasholds.
  - Track data pipeline quality, to either re-run jobs, alert for detected failures or anomalies.
- Feature [documentation](https://learn.microsoft.com/fabric/data-activator/data-activator-introduction).



===

# The e-commerce store

The e-commerce store database entities are:  
- **Product:** the product catalog. 
- **ProductCategory:** the product categories.  
- **Customer:** the customers that purchased items in the store.
- **Address:** the addresses of the customers.
- **SalesOrderHeader:** the metadata for the orders.
- **SalesOrderDetail:** every item purchased in an order.
- **events:** a click or impression event.   
  - An **impression event** is logged when a product appears in the search results.
    !IMAGE[HimiwayBikes.png](instructions294573/HimiwayBikes.png)
  - A **click event** is logged when the product is clicked and the customer has viewed the details.  
    !IMAGE[HEADAccessories.png](instructions294573/HEADAccessories.png)

Photo by <a href="https://unsplash.com/@himiwaybikes?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Himiway Bikes</a> on <a href="https://unsplash.com/photos/black-and-gray-motorcycle-parked-beside-brown-wall-Gj5PXw1kM6U?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>  
Photo by <a href="https://unsplash.com/@headaccessories?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">HEAD Accessories</a> on <a href="https://unsplash.com/photos/silver-and-orange-head-lamp-9uISZprJdXU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>  
Photo by <a href="https://unsplash.com/@jxk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Jan Kopřiva</a> on <a href="https://unsplash.com/photos/a-close-up-of-a-helmet-with-sunglasses-on-it-CT6AScSsQQM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
  




===

# Architecture

### Components of Fabric's Real-Time Intelligence
!IMAGE[Components of Fabric's Real-Time Intelligence](instructions294573/RTIComponents.png)
Real-Time Intelligence allows organizations to ingest, process, analyze, ask questions over your data using natural language, transform and automatically act on data. All with a central hub (Real-Time Hub) to easily access and visualize all internal and external, first- and third-party streaming data. We can achieve faster, more accurate decision-making and accelerated time to insight.

### Lab Architecture
!IMAGE[Architecture Diagram](instructions294573/architectureDiagram.png)
Now with Data Activator (Reflex), we can also set alerts on Real-time Dashboards to send a message in Teams with conditional threasholds or even more advanced actions.



===

# Data schema

### Data flow
!IMAGE[DBSchema.png](instructions294573/DBSchema.png)

<br />
---
### Tables
| Table| Origin     | Description|
|------|------------|------------|
| **events**|EventHouse table|Streaming events representing the product being seen or clicked by the customer. Will be streamed into Fabric Eventhouse from an eventstream. We'll use a Fabric Notebook to simulate and push synthetic data (fake data) into an endpoint.|
| **Customer**| Copied using Pipeline| Describes customers and their geographic location|
| **Address**| Copied using Pipeline|Customers addresses|
| **SalesOrderHeader**| Copied using Pipeline|Information about sales orders|
| **SalesOrderDetail**| Copied using Pipeline|Detailed information about sales orders, including product IDs and quantities sold|
| **SilverCustomer**|EventHouse table|Table created based on an update policy with **transformed data**|
| **SilverAddress**|EventHouse table|Table created based on an update policy with **transformed data**|
| **SilverOrdersHeader**|EventHouse table|Table created based on an update policy with transformed data|
| **SilverOrdersDetail**|EventHouse table|Table created based on an update policy with transformed data|
<br />
---
### External Tables
| Table| Origin     | Description|
|------|------------|------------|
| **Product**|**Shortcut** to SQL DB|Products, including descriptions and prices|
| **ProductCategory**|**Shortcut** to SQL DB|Product category|
<br />
---
### Functions
| Function| Description|
|------------|------------|
|**ParseAddress**|Adds watermark column based on **ingestion_time()**|
|**ParseCustomer**|Adds watermark column based on **ingestion_time()**|
|**ParseSalesOrderHeader**|Adds calculated column **DaysShipped** by measuring the number of days between **ShipDate** and **OrderDate**. Also, adds watermark column based on **ingestion_time()** |
|**ParseSalesOrderDetail**|Adds watermark column based on **ingestion_time()**|
<br />
---
### Materialized-Views
| View | Origin     | Description|
|------|------------|------------|
| **GoldAddress**|EventHouse silver table|Materialized view showing only the **latest** changes in the source table showing how to handle duplicate or updated records|
| **GoldCustomer**|EventHouse silver table|Materialized view showing only the **latest** changes in the source table showing how to handle duplicate or updated records|
| **GoldSalesOrderHeader**|EventHouse silver table|Materialized view showing only the **latest** changes in the source table showing how to handle duplicate or updated records|
| **GoldSalesOrderDetail**|EventHouse silver table|Materialized view showing only the **latest** changes in the source table showing how to handle duplicate or updated records|
<br />
---


===
# Exercise 1: Configure pre-requisites

It is recommended that you review the following articles to learn more about some of the technologies that you will be working with:
  - [KQL Learn](https://aka.ms/learn.kql)
  - [Real-Time Intelligence Tutorial](https://learn.microsoft.com/fabric/real-time-intelligence/tutorial-introduction)

===

## Task 1: Create a Fabric workspace

>[!note] The lab instructions include screenshots to help you understand the steps. When we name resources, we use some descriptive words followed by an 8-digit number. This number helps guarantee that all resources created are unique. The 8-digit number used in the screenshots is representative of what you may see and will differ from the number that is generated for your lab instance.

1. [] Sign into the @lab.VirtualMachine(Base23C).SelectLink virtual machine by using the following credentials:

    | Field | Value |
    |:---------|:---------|
    | Username   | +++@lab.VirtualMachine(Base23C).UserName+++   |
    | Password   | +++@lab.VirtualMachine(Base23C).Password+++   |

1. [] Open a browser window and go to +++https://app.fabric.microsoft.com/+++.

1. [] In the open browser window, sign in by using the following credentials:

    | Field | Value |
    |:---------|:---------|
    | Username   | +++@lab.CloudPortalCredential(CSU-User).Username+++   |
    | Password   | +++@lab.CloudPortalCredential(CSU-User).Password+++   |

1. [] In the navigation pane on the left side of the page, select **Workspaces** and then select **+New workspace**.

	!IMAGE[09463lmf.jpg](instructions294573/09463lmf.jpg)

1. [] In the workspace Name field, enter +++**RealTimeAnalytics@lab.LabInstance.Id**+++.

1. [] Select **Apply**.

    !IMAGE[xyjhk88c.jpg](instructions294573/xyjhk88c.jpg)

   
===

## Task 2: Create a new Eventhouse

An [Eventhouse](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/eventhouse) is designed to handle real-time data streams efficiently, which lets organizations ingest, process, and analyze data in near real-time. Event houses are particularly useful for scenarios where **timely insights are crucial**. 

Event houses provide a scalable infrastructure that allows organizations to handle growing volumes of data, ensuring optimal performance and resource use. Event houses are the **preferred** engine for semistructured and free text analysis. An event house is a workspace of databases which might be shared across a specific project. It allows you to manage multiple databases at once, sharing capacity and resources to optimize performance and cost. 

Event houses provide unified monitoring and management across all databases and per database. Event houses are **specifically tailored** to time-based, streaming events with structured, semistructured, and unstructured data. You can get data from multiple sources or pipelines (For example, Eventstream, SDKs, Kafka, Logstash, data flows, and more) and multiple data formats. This data is automatically indexed and partitioned based on ingestion time.


1. [] On the workspace page, select **+ New item**. Scroll down to the **Store data** section and select **Eventhouse**.

   !IMAGE[yj7cobd7.jpg](instructions294573/yj7cobd7.jpg)

1. [] In the Eventhouse name field, enter +++**RTADemo@lab.LabInstance.Id**+++ and then select **Create**. Dismiss any pop-up messages that may display.

    !IMAGE[a0lz2xyd.jpg](instructions294573/a0lz2xyd.jpg)

===

## Task 3: Create a new Eventstream

In this section, events (impressions and click events) generated by a notebook are streamed. These events are directed into an event stream and subsequently consumed by the Eventhouse KQL Database. This process ensures efficient handling and utilization.

!IMAGE[e8h3x4pv.png](instructions294573/e8h3x4pv.png)

1. [] In the left navigation pane, select **Workspaces** and then select the **RealTimeAnalytics@lab.LabInstance.Id** workspace. 

1. [] Select **+ New item**. In the **Get data** section, select **Eventstream**.

	!IMAGE[iazk6bbj.jpg](instructions294573/iazk6bbj.jpg)


1. [] In the Eventstream name field, enter +++**RTADemoEventStream@lab.LabInstance.Id**+++ and then select **Create**. Dismiss any pop-up messages that may display.

	!IMAGE[83q3yoqs.jpg](instructions294573/83q3yoqs.jpg)

    >[!Alert]You may see an option to enable Preview Features. For this lab, do not select the checkbox to enable preview features. If you did enable preview features some prompts may differ from the images presented in the lab instructions.

1. [] On the Eventstream page, select **Add source** and then select **Custom endpoint**.

    !IMAGE[04h76d7k.jpg](instructions294573/04h76d7k.jpg)

    >[!knowledge] Custom endpoints were previously known as Custom apps.

1. [] In the **Source name** field, enter +++**RTADemoE-Hub@lab.LabInstance.Id**+++ and then select **Add**.

    !IMAGE[co928lzu.jpg](instructions294573/co928lzu.jpg)


1. [] At the top right of the page, select **Publish**. 

	!IMAGE[2u1m2po6.jpg](instructions294573/2u1m2po6.jpg)

    >[!note] It may take several minutes for the publishing process to complete. Wait until the RTADemoE-Hub object on the design canvas displays the **Active** status before you proceed to the next step.

    !IMAGE[cx14luow.jpg](instructions294573/cx14luow.jpg)

1. [] Select the **RTADemoE-Hub** Eventstream source. The Details pane displays.

	!IMAGE[2jvqp290.jpg](instructions294573/2jvqp290.jpg)

    >[!note]In the next several steps, you will collect the Event hub name and Connection string-primary key values. You will use these values later in the lab to configure the notebook that will generate and send events. 

1. [] In the Details pane, select **Event Hub** and then select **SAS Key Authentication**.

	!IMAGE[7dvyrhuy.jpg](instructions294573/7dvyrhuy.jpg)

1. [] Copy the value for the **Event hub name** key into the following text field:

    @lab.TextBox(EventHubName)

    >[!NOTE] After pasting the value into the text field, move the mouse cursor outside of the text box or press the **Tab** key. This saves the values for use later in the lab.

1. [] Select the **View** icon to the right of the **Connection string-primary key** field to display the key.  

	!IMAGE[anbu0tr0.jpg](instructions294573/anbu0tr0.jpg)

    >[!Alert] Be sure to select Connection string-primary key and not Primary key.

1. [] Copy the Connection string-primary key value into the following text field:

    @lab.TextBox(connectionString)
	

===

## Task 4: Import the Data Generator notebook

1. [] Open a new browser window and go to `https://github.com/microsoft/FabricRTA-Retail/blob/main/notebooks/Generate%20synthetic%20events.ipynb`. A GitHub repository displays.

	!IMAGE[3nosfod2.jpg](instructions294573/3nosfod2.jpg)

1. [] In the left navigation pane for the page, in the notebooks section, select **Generate synthetic events.ipynb**.

	!IMAGE[002636oa.jpg](instructions294573/002636oa.jpg)

1. [] On the right side of the page, in the menu bar, select **Download raw file**.

	!IMAGE[wjcy9ej6.jpg](instructions294573/wjcy9ej6.jpg)

1. [] Return to the Microsoft Fabric browser window. Select the **RealTimeAnalytics@lab.LabInstance.Id** workspace.

1. [] On the menu bar for the workspace page, select **Import**. Select **Notebook** and then select **From this computer**.

	!IMAGE[yglc02gm.jpg](instructions294573/yglc02gm.jpg)

1. [] In the Import status pane, select **Upload**.

	!IMAGE[ccz9y1ej.jpg](instructions294573/ccz9y1ej.jpg)

1. [] Navigate to the folder where you save downloaded files. Select **Generate synthetic events.ipynb** and then select **Open**.
    

===
## Task 5: Configure and run the notebook

1. [] In the lower portion of the RealTimeAnalytics@lab.LabInstance.Id workspace page, select **Generate synthetic events**. The notebook displays.

	>[!Alert] You should see an error message asking you to choose an environment to run the notebook. This is expected. In this task, you will create a new environment and then associate the notebook with the environment.
    >
    >!IMAGE[h8agcz0c.jpg](instructions294573/h8agcz0c.jpg)

	!IMAGE[uraa9y3x.jpg](instructions294573/uraa9y3x.jpg)

1. [] On the menu bar for the notebook, select **Home**.

	!IMAGE[6gihxtut.jpg](instructions294573/6gihxtut.jpg)

1. [] Select the **Environment** dropdown list and then select **New environment**.

	!IMAGE[eoun1ajk.jpg](instructions294573/eoun1ajk.jpg)

1. In the New Environment dialog, enter +++DemoEnv@lab.LabInstance.Id+++ and then select **Create**.

	!IMAGE[tvcjdd1s.jpg](instructions294573/tvcjdd1s.jpg)

1. [] Select the **Go back** button in the browser to return to the notebook.

	!IMAGE[e5nlpulb.jpg](instructions294573/e5nlpulb.jpg)

1. [] Select the **Environment** dropdown list and then select **Change environment**.

	!IMAGE[o2koxfwt.jpg](instructions294573/o2koxfwt.jpg) 

1. [] In the **Choose an enironment for this notebook** dialog, select **DemoEnv@lab.LabInstance.Id** and then select **Confirm**.

	>[!Alert] If you do not select the new environment in this step you will not be able to run cells later in this task.

	!IMAGE[j8yycgo0.jpg](instructions294573/j8yycgo0.jpg)

1. [] Add the following line of code to line two of the first code cell in the notebook.

	+++! python -m pip install --upgrade pip+++

	!IMAGE[hn84bmet.jpg](instructions294573/hn84bmet.jpg)

1. [] Locate the two placeholders in the second code cell. Use the values in the following table to replace the placeholders in the code cell.

	| Placeholder| New value to use|
	|------------|------------|
    |&lt;eventHubConnString>|+++@lab.Variable(connectionString)+++|
    |&lt;eventHubNameevents>|+++@lab.Variable(EventHubName)+++|

   !IMAGE[iyuf0wvb.jpg](instructions294573/iyuf0wvb.jpg)

1.  [] Hover over the first code cell. The Run cell button (&#9655;) displays. Select the Run cell button to execute the code in the cell. 

	>[!Note]This code installs specific versions of required libraries. It may take 3-5 minutes for code execution to complete. A counter at the bottom left of code cells shows current execution time. The counter is replaces with a green check mark (&#10003;) when execution completes. Error messages will display below the cell.

	>[!Alert] This step may generate errors related to dependency conflicts because of updates and changes to libraries and their dependencies. For example, during testing the following error message displayed: </br></br>
"ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
nni 3.0 requires filelock<3.12, but you have filelock 3.13.1 which is incompatible."</br></br>If you see errors similar to this, hover over the top left of the first code cell and select **+ Code**. This adds a new cell above the first cell. </br></br>Add code to the cell to install the specific version of the library required and then run the cell. For example, to correct the error we observed during testing, we ran the following code:</br></br>**+++! pip install filelock==3.12.0+++**</br></br>After you install the new library version run the following cell again. Do not proceed to the next step until code cell execution completes successfully.

	!IMAGE[hn84bmet.jpg](instructions294573/hn84bmet.jpg)



1. [] On the menu bar at the top of the notebook, select **Run all** to execute all code in the notebook and start generating streaming events.

    !IMAGE[g09soiio.jpg](instructions294573/g09soiio.jpg)

	>[!note] The notebook will automatically run code in all of the other cells. After code in the last cell runs, generated syntetic events (in JSON format) will display at the bottom of the notebook.

    >[!Alert] If there are no errors in any of the notebook cells, the notebook will stay active throughout the remainder of the lab. 
    >
    >The code generates simulated signals that you will ingest later in the lab. Leave the notebook running and proceed to Task 6.

===

## Task 6: Configure the destination for the EventStream

1. [] In the left navigation pane, select **Workspaces** and then select RealTimeAnalytics@lab.LabInstance.Id.

1. [] Select the **RTADemoEventStream@lab.LabInstance.Id** event stream in your Fabric workspace. After the page loads completely, at the bottom of the page you will see events that have recently been ingested.

	!IMAGE[nigkhz9n.jpg](instructions294573/nigkhz9n.jpg)


	!IMAGE[04ve0zes.jpg](instructions294573/04ve0zes.jpg)

1. [] At the top right of the page, if the mode is currently set to **Live**, select the dropdown list and then select **Edit**.

	!IMAGE[t5ssnclk.jpg](instructions294573/t5ssnclk.jpg)


1. [] On the design canvas, select **Transform events or add destination** and then select **Eventhouse**.

	>[!Note] You may need to adjust the location of the **Transform events or add destination** shape to see **Eventhouse** at the bottom of the list.

    !IMAGE[rx5dvqc0.jpg](instructions294573/rx5dvqc0.jpg)

1. [] Configure the Eventhouse by using the values in the following table. Leave all other fields at default values:

    | Field | Value |
    |:---------|:---------|
    | Data ingestion mode   | Direct ingestion   |
    | Destination Name   | +++RTADemoEventhouse@lab.LabInstance.Id+++   |
    | Workspace   | **RealTimeAnalytics@lab.LabInstance.Id**   |
    | Eventhouse   | **RTADemo@lab.LabInstance.Id**   |
    | KQL Database   | **RTADemo@lab.LabInstance.Id**   |

1. [] Select **Save**.

    !IMAGE[gw8g0v8v.jpg](instructions294573/gw8g0v8v.jpg)

1. [] At the top right of the page, select **Publish**. 

	!IMAGE[2u1m2po6.jpg](instructions294573/2u1m2po6.jpg)

    >[!note] It may several minutes for the publishing process to complete. Wait until the RTADemoEventhouse object on the design canvas displays the **Unconfigured** status before you proceed to the next step.

    !IMAGE[cv2kkay2.jpg](instructions294573/cv2kkay2.jpg)

1. [] On the RTADemoEventhouse object, select **Configure**.

	!IMAGE[jkilsa35.jpg](instructions294573/jkilsa35.jpg)

1. [] In the Get data dialog, select **+ New Table**.

	!IMAGE[hwzureji.jpg](instructions294573/hwzureji.jpg)

1. [] Enter +++events+++ as the table name.

	!IMAGE[9noomufy.jpg](instructions294573/9noomufy.jpg)

    >[!note] The table name is case sensitive and specifically called events for the remainder of the lab queries.

1. [] Select the checkmark (&#x2713;) next to the table name to validate the name.

    !IMAGE[l9u0gxmn.png](instructions294573/l9u0gxmn.png)

1. [] Select **Next**.

	!IMAGE[m35ocjkb.jpg](instructions294573/m35ocjkb.jpg)

1. [] Review the sample of the streaming data. The data will display **CLICK** and **IMPRESSION** events.

    !IMAGE[y2d0o7kb.png](instructions294573/y2d0o7kb.png)

1. [] Select **Finish**.

    !IMAGE[d3izhlil.png](instructions294573/d3izhlil.png)

1. [] After data preparation is complete, select **Close**. The EventStream page displays. The status for the data destination object in the design canvas should now say **Active**. If it does not, refresh the page.

    !IMAGE[xfyic3l3.jpg](instructions294573/xfyic3l3.jpg)


===

## Task 7: Configure access to the KQL database 

This "one logical copy" feature automatically allows KQL database tables to be accessed from a Lakehouse or notebook in delta-parquet format by using OneLake.

- When activated it will constantly copy the KQL data to your Fabric OneLake in delta format. It allows you to query KQL Database tables as delta tables using Spark or SQL endpoint on the Lakehouse. We recommend enabling this feature "before" we load the more data into our KQL Database. Also, consider this feature can be enabled/disabled per table if necessary. You can read more about this feature here: [Announcing Delta Lake support in Real-Time Intelligence KQL Database](https://support.fabric.microsoft.com/blog/announcing-delta-support-in-real-time-analytics-kql-db?ft=All).
- Enabling data availability of KQL Database in OneLake means you can enjoy the best of both worlds. (1) Query the data with high-performance and low-latency in the KQL database and (2) Query the same data in Delta Lake format via any other Fabric engines such as Power BI Direct Lake mode, Warehouse, Lakehouse, Notebooks, and more.
- KQL Database offers a robust mechanism to batch the incoming streams of data into one or more Parquet files suitable for analysis. The Delta Lake representation is provided to keep the data open and reusable. This logical copy is managed once, is paid for once and users should consider it a single data set.
!IMAGE[Schema_Lakhouse.png](instructions294573/Schema_Lakhouse.png)


1. [] In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. [] In the list of resources, in the RTADemo@lab.LabInstance.Id section, select the **RTADemo@lab.LabInstance.Id** KQL database. The Details page for the database displays.

	!IMAGE[77s8opow.jpg](instructions294573/77s8opow.jpg)

    !IMAGE[rjiae4wx.jpg](instructions294573/rjiae4wx.jpg)

1. [] In the **Database details** pane that appears at the right side of the page, change the value of the Availability setting from Off to **On**. 

    !IMAGE[57i867sr.jpg](instructions294573/57i867sr.jpg)

1. [] In the **Enable OneLake** availability dialog, ensure that the **Apply to existing tables** check box is selected and then select **Enable**.

	!IMAGE[lpsu9z8n.jpg](instructions294573/lpsu9z8n.jpg)

===
## Task 8: Build the KQL database schema

In this section, all the tables in the KQL Database are created. Two of these tables, namely 'product' and 'productCategory', serve as shortcuts to the SQL Database. It's important to note that the data from these two tables is not duplicated into the KQL Database. Instead, they remain in the SQL database and are accessed as needed. This approach ensures efficient data management and utilization.

!IMAGE[ig5w4wqx.png](instructions294573/ig5w4wqx.png)

1. [] In the KQL databases pane, select the ellipses (&hellip;) next to **RTADemo@lab.LabInstance.Id** and then select **Query data**. The query pane displays.

    !IMAGE[o03vcify.jpg](instructions294573/o03vcify.jpg)

    !IMAGE[21iphedj.jpg](instructions294573/21iphedj.jpg)

1. [] Select and delete the existing queries and comments. 

	!IMAGE[4hr4vzo6.jpg](instructions294573/4hr4vzo6.jpg)

1. [] Open a new browser window and go to +++https://github.com/microsoft/FabricRTA-Retail/blob/main/kql/createAll.kql+++ and copy the contents.

1. [] Copy all of the code into the Windows clipboard. 

1. [] Return to the browser window which hosts Microsoft Fabric and paste the code into the query window.

	!IMAGE[wpr5bqks.jpg](instructions294573/wpr5bqks.jpg)

1. [] Select **Run**.

    !IMAGE[7ixpjrc7.jpg](instructions294573/7ixpjrc7.jpg)

1. [] Confirm that the RTADemo@lab.LabInstance.Id database contains multiple tables.

    !IMAGE[wymqurnj.jpg](instructions294573/wymqurnj.jpg)

    >[!Alert] You may need to refresh the browser window after the query completes to see all of the tables that were added.

===

## Task 9: Create a new lakehouse

1. [] In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. [] Select **+ New item**. Scroll down to the **Store data** section and select **Lakehouse**.

    !IMAGE[u5in4i0k.jpg](instructions294573/u5in4i0k.jpg)

1. [] In the New lakehouse dialog, enter +++**RTADemoLakehouse@lab.LabInstance.Id**+++ in the Name field and then select **Create**.

	>[!Alert] Be sure that you do not select the **Lakehouse schema (Public Preview)** checkbox. Selecting this option can lead to failures later in this task.

    !IMAGE[9txbb3wj.jpg](instructions294573/9txbb3wj.jpg)

1. [] From the RTADemoLakehouse@lab.LabInstance.Id page, in the menu bar, select **Get Data** and then select **New Shortcut**.

    !IMAGE[qk8wavqd.jpg](instructions294573/qk8wavqd.jpg)

1. [] In the New shortuct dialog, select **Microsoft OneLake**.

	!IMAGE[snvqukdp.jpg](instructions294573/snvqukdp.jpg)


1. [] In the Select a data source type dialog, select the **RTADemo@lab.LabInstance.Id** KQL database and then select **Next**.

    !IMAGE[qjkicm1o.jpg](instructions294573/qjkicm1o.jpg)

1. [] Expand the **Tables** node and select all the tables. Select **Next**.

    !IMAGE[932v63go.jpg](instructions294573/932v63go.jpg)

1. [] Select **Create**.

    !IMAGE[yb5gjtwa.jpg](instructions294573/yb5gjtwa.jpg)

1. [] Confirm that the Eventhouse KQL database tables are now available in the Lakehouse.

    >[!Alert] You may need to refresh the browser window to display the tables. It will take several minutes for Fabric to load preview data into each of the tables.

	!IMAGE[oqu4dcwt.jpg](instructions294573/oqu4dcwt.jpg)



===

#Exercise 2: Create a data pipeline

In this exercise, you will configure Fabric data pipelines to copy data from a SQL DB into an Eventhouse KQL DB using batch ingest. This type of ingestion can be executed as a one-time process or scheduled to run periodically. The data pipeline that is about to be created will be configured to run periodically, copying data to the Eventhouse database.

!IMAGE[pydcdbdt.png](instructions294573/pydcdbdt.png)

===
## Task 1: Import the Address table

1. [] Open a new browser tab and go to +++https://portal.azure.com+++.

1. [] If prompted, sign in by using the following credentials:

    | Field | Value |
    |:---------|:---------|
    | Username   | +++@lab.CloudPortalCredential(CSU-User).Username+++   |
    | Password   | +++@lab.CloudPortalCredential(CSU-User).Password+++   |

1. [] Dismiss any pop-up messages that display. From the Portal home page, select the search bar at the top and enter +++sql+++, then select **SQL servers**.

    !IMAGE[1j0i0ruz.jpg](instructions294573/1j0i0ruz.jpg)

1. [] Select **sql@lab.LabInstance.Id**.

	!IMAGE[xr59ok4i.jpg](instructions294573/xr59ok4i.jpg)

1. [] In the list of options for the SQL Server instance, select **Security** and then select **Networking**.

	!IMAGE[k9aqekb6.jpg](instructions294573/k9aqekb6.jpg)

1. [] At the bottom of the **Networking** tab, select the **Allow Azure services and resources to access this server** checkbox and then select **Save**.

    !IMAGE[x4qug7l8.jpg](instructions294573/x4qug7l8.jpg)

1. [] Return to the browser window which hosts Microsoft Fabric. In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. [] Select **+ New**. In the **Get data** section, select **Data pipeline**.

    !IMAGE[g1pcj0mi.jpg](instructions294573/g1pcj0mi.jpg)

1. [] Enter +++CopyTablesPipeline@lab.LabInstance.Id+++ as the pipeline name, then select **Create**.

    !IMAGE[pipelineName.png](instructions294573/pipelineName.png)
    

1. [] In the **Start with a blank canvas** section, select **Pipeline Activity**.

	!IMAGE[g7ccy918.jpg](instructions294573/g7ccy918.jpg)

1. [] In the **Move and Transform** section, select **Copy data**. The Copy data activity is added to the canvas. The details pane for the activity appears below the canvas.

    !IMAGE[4rlrawq1.jpg](instructions294573/4rlrawq1.jpg)

1. [] In the Details pane on the General tab, enter +++Address+++ in the Name field.

    !IMAGE[ActivityAddName.png](instructions294573/ActivityAddName.png)

1. [] Select the **Source** tab.

    !IMAGE[mwl39tw4.png](instructions294573/mwl39tw4.png)
    

1. [] Select the **Connection** dropdown list and then select **More**.

    !IMAGE[7b91unk1.jpg](instructions294573/7b91unk1.jpg)

1. [] In the Get Data dialog, select **Azure SQL Database**.

	>[!note] You may need select **View more** to see Azure SQL database option. You can also search for Azure SQL database.

    !IMAGE[44y9mir1.png](instructions294573/44y9mir1.png)

1. [] Configure the data source settings by using the values in the following table. Leave all other values at default settings:

    | Field | Value |
    |:---------|:---------|
    | Server   | +++sql@lab.LabInstance.Id.database.windows.net+++   |
    | Database   | +++aworks+++   |
    | Username   | +++azureadmin1+++   |
    | Password   | +++M+6e!jJ0DaM+6e!jJ0Da+++   |

    !IMAGE[zg24tt9g.jpg](instructions294573/zg24tt9g.jpg)

1. [] Select **Connect**.

	>[!Note] It may take a minute or two to connect to the database. After the connection is established, you are returned to the pipleline page.

1. [] Within the **Source** tab, select **Test Connection** to verify connectivity.

    !IMAGE[9ghjiahd.jpg](instructions294573/9ghjiahd.jpg)

1. [] In the **Table** dropdown list, select **SalesLT.Address**.

    !IMAGE[touzmd11.jpg](instructions294573/touzmd11.jpg)

1. [] Select the **Destination** tab.

    !IMAGE[hedywkhh.png](instructions294573/hedywkhh.png)
    <!--!IMAGE[0fuhsy58.png](instructions294573/0fuhsy58.png)-->

1. [] In the **Connection** dropdown list, select **RTADemo@lab.LabInstance.Id**. Wait for the table menu to load.

    

1. [] In the **Table** dropdown list, select **Address**.

    !IMAGE[8vnruiar.jpg](instructions294573/8vnruiar.jpg)

1. [] Select the **Mapping** tab and select **Import schemas**.

    !IMAGE[s6uurhtg.png](instructions294573/s6uurhtg.png)
    <!--!IMAGE[715v8dwa.png](instructions294573/715v8dwa.png)-->

1. [] Verify all fields are mapped to the correct types without warnings.

    !IMAGE[he3yvdk3.png](instructions294573/he3yvdk3.png)

1. [] In the pipeline menu bar, select **Run** and then select **Run**.

    !IMAGE[appr758y.jpg](instructions294573/appr758y.jpg)

1. [] If prompted, select **Save and run**.

    >[!Note] The pipeline will run until you see Pipeline status ✅ **Succeeded**.
    >
    >!IMAGE[owai26ix.png](instructions294573/owai26ix.png)



===
## Task 2: Verify that address data is imported

1. [] In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. [] In the list of objects in the workspace, in the **RTADemo@lab.LabInstance.Id** section, select **RTADemo@lab.LabInstance.Id_queryset**.

	!IMAGE[57i1bfb3.jpg](instructions294573/57i1bfb3.jpg)

1. [] On the **RTADemo@lab.LabInstance.Id_queryset** page, select the **+** to add a new tab.

    !IMAGE[newTabQueySet.png](instructions294573/newTabQueySet.png)

1. [] Enter +++Address+++ into the editor panel.

1. [] Select **Run**.

    !IMAGE[zlfsgj53.png](instructions294573/zlfsgj53.png)

1. [] The query should display 450 records. 

    !IMAGE[SalesLT.AddressRecords.png](instructions294573/SalesLT.AddressRecords.png)

1. [] Select the **+** to add a new tab.

1. [] In the editor panel, add +++SilverAddress+++ to the query.

1. [] Select **Run**.

1. [] Like the result set in step 5 there are 450 rows. An additional column named **IngestionDate** has been added. This transformation occurred automatically in real-time by the KQL function **parseAddress()**. The function was defined in the Update Policy when we ran createAll.kql database script (see lines 41-45).

    >[!note] You can use +++.show table SilverAddress policy update+++ to view the **Policy** settings. Also, +++.show functions+++ will show the **Body** definition of all functions and +++.show materialized-views+++ will show the **Query**, health and more. 

1. [] Select the **+** to add a new tab and add ++GoldAddress++ to the editor panel.

1. [] Select **Run**. 

1. [] There are 450 rows. The Gold layer uses materialized views based on the maximum IngestionDate to show only the **latest** ingested rows. 

>[!note]This transformation occured automatically in real-time by the [materialized-view](https://learn.microsoft.com/azure/data-explorer/kusto/management/materialized-views/materialized-view-overview). 
>
>The view was defined in the [createAll.kql](https://github.com/microsoft/FabricRTIWorkshop/blob/main/kql/createAll.kql) database script (see lines 64-68). The **arg_max()** aggregate function is documented [here](https://learn.microsoft.com/azure/data-explorer/kusto/query/arg-max-aggregation-function). 
>
>Materialized views always return an up-to-date result of the aggregation query (always fresh). Querying a materialized view is more performant than running the aggregation directly over the source table.
>    ```
    //GOLD LAYER
    // use materialized views to view the latest changes in the SilverAddress table
    .create materialized-view with (backfill=true) GoldAddress on table SilverAddress
    {
        SilverAddress
        | summarize arg_max(IngestionDate, *) by AddressID
    }
    ```   

1. [] On the menu bar, select **Save**.


===
## Task 3: Import the Customer table

1. [] In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. [] In the list of objects in the workspace, , select **CopyTablesPipeline@lab.LabInstance.Id**.

1. [] Select the **Address** Copy data activity and then select **Clone**.

    !IMAGE[CloneAddress.png](instructions294573/CloneAddress.png)

1. [] In the Details pane for the new activity, change the name to +++Customer+++.

    !IMAGE[7wiz0cvs.jpg](instructions294573/7wiz0cvs.jpg)

1. [] Right-click the **Address** Copy Data activity and select **Deactivate**.

    !IMAGE[DeactivateActivity.png](instructions294573/DeactivateActivity.png)

1. [] On the **Customer** activity, select the **Source** tab.

    !IMAGE[mb2a9aqb.png](instructions294573/mb2a9aqb.png)

1. [] In the **Connection** dropdown list, select **sql@lab.LabInstance.Id.database.windows.net**.

    !IMAGE[j86g8wwd.jpg](instructions294573/j86g8wwd.jpg)

1. [] In the **Connection Type** dropdown list, select **Azure SQL Database**.

    !IMAGE[ConnTypeAzSQLDB.png](instructions294573/ConnTypeAzSQLDB.png)

1. [] In the **Table** dropdown list, select **SalesLT.Customer**.

    !IMAGE[osda6ovb.jpg](instructions294573/osda6ovb.jpg)

1. [] Select the **Destination** tab.

    !IMAGE[bs21l6dl.png](instructions294573/bs21l6dl.png)

1. [] In the **Connection** dropdown list, select **RTADemo@lab.LabInstance.Id** and wait for the table menu to load.

    !IMAGE[k7ohx6nj.jpg](instructions294573/k7ohx6nj.jpg)

1. [] Under the **Table** dropdown list, select **Customer**.

    !IMAGE[2l3jo11w.png](instructions294573/2l3jo11w.png)

1. [] Select the **Mapping** tab and select **Import schemas**.

    !IMAGE[MappingTab.png](instructions294573/MappingTab.png)

1. [] Verify that all fields are mapped to the correct types without warnings.

    !IMAGE[he3yvdk3.png](instructions294573/he3yvdk3.png)

===
## Task 4: Import the SalesOrderHeader table

1. [] If necessary, return to the **CopyTablesPipeline@lab.LabInstance.Id** pipeline.

1. [] Clone the **Customer** activity.

    !IMAGE[CloneCustomer.png](instructions294573/CloneCustomer.png)

1. [] In the lower part of the pipeline, name the activity +++SalesOrderHeader+++.

    !IMAGE[onxzdrwg.png](instructions294573/onxzdrwg.png)

1. [] In the lower part of the pipeline, select the **Source** tab.

    !IMAGE[mb2a9aqb.png](instructions294573/mb2a9aqb.png)

1. [] In the **Connection** dropdown list, select **sql@lab.LabInstance.Id.database.windows.net**.

    !IMAGE[bi5e5k8q.jpg](instructions294573/bi5e5k8q.jpg)

1. [] In the **Connection Type** dropdown list, select **Azure SQL Database**.

    !IMAGE[ConnTypeAzSQLDB.png](instructions294573/ConnTypeAzSQLDB.png)

1. [] In the **Table** dropdown list, select **SalesLT.SalesOrderHeader**.

    !IMAGE[qqajwq4k.jpg](instructions294573/qqajwq4k.jpg)

1. [] Select the **Destination** tab.

    !IMAGE[bs21l6dl.png](instructions294573/bs21l6dl.png)

1. [] Select the **Connection** menu, select **RTADemo@lab.LabInstance.Id** and wait for the table menu to load.

    !IMAGE[nkxpuwui.jpg](instructions294573/nkxpuwui.jpg)

1. [] In the **Table** dropdown list, select **SalesOrderHeader**.

    !IMAGE[u08av9j8.jpg](instructions294573/u08av9j8.jpg)

1. [] Select the **Mapping** tab and select **Import schemas**.

    !IMAGE[MappingTab.png](instructions294573/MappingTab.png)

1. [] Verify that all fields are mapped to the correct types without warnings.

    !IMAGE[4t4hfasf.png](instructions294573/4t4hfasf.png)

===
## Task 5: Import the SalesOrderDetail table

1. [] If necessary, return to the **CopyTablesPipeline@lab.LabInstance.Id** pipeline.

1. [] Clone the **SalesOrderHeader** activity.

    !IMAGE[CloneHeader.png](instructions294573/CloneHeader.png)

1. [] In the lower part of the pipeline, name the activity +++SalesOrderDetail+++.

    !IMAGE[3d1dzvet.png](instructions294573/3d1dzvet.png)

1. [] In the lower part of the pipeline, select the **Source** tab.

    !IMAGE[mb2a9aqb.png](instructions294573/mb2a9aqb.png)

1. [] In the **Connection** dropdown list, select **sql@lab.LabInstance.Id.database.windows.net**.

    !IMAGE[iflep90m.jpg](instructions294573/iflep90m.jpg)

1. [] In the **Connection Type** dropdown list, select **Azure SQL Database**.

    !IMAGE[ConnTypeAzSQLDB.png](instructions294573/ConnTypeAzSQLDB.png)

1. [] In the **Table** dropdown list, select **SalesLT.SalesOrderDetail**

    !IMAGE[tw6cf72i.jpg](instructions294573/tw6cf72i.jpg)

1. [] Select the **Destination** tab.

    !IMAGE[bs21l6dl.png](instructions294573/bs21l6dl.png)

1. [] Select the **Connection** menu, select **RTADemo@lab.LabInstance.Id**, and wait for the table menu to load.

    !IMAGE[h26sdxz0.jpg](instructions294573/h26sdxz0.jpg)

1. [] In the **Table** dropdown list, select **SalesOrderDetail**.

    !IMAGE[48n2id64.jpg](instructions294573/48n2id64.jpg)

1. [] Select the **Mapping** tab and select **Import schemas**.

    !IMAGE[MappingTab.png](instructions294573/MappingTab.png)


===
## Task 6: Run pipeline activities

1. [] In the pipeline menu bar, select **Run** to run all the activities except the Address activity (which is deactivated).

    !IMAGE[RunButton.png](instructions294573/RunButton.png)

1. [] If prompted, select **Save and run**.

    >[!Note] The pipeline will run until you see Pipeline status ✅ **Succeeded**
    >
    >!IMAGE[owai26ix.png](instructions294573/owai26ix.png)

@lab.Activity(Question10)

@lab.Activity(Question11)

@lab.Activity(Question12)



<!-- ---------------------------------------------------------------------------------------------------------->
===

#Exercise 3: Create a Real-Time dashboard

You will create a Real Time Analysis dashboard  to visualize the streaming data. It will be updated, streamed in real time, and provide a live visual representation of the data flow. This allows for immediate insights and understanding of the current state of the data as it changes and evolves. The dashboard is designed to refresh every 30 seconds.

!IMAGE[RTAnalysisDashboard.png](instructions294573/RTAnalysisDashboard.png)


===
##Task 1: Define the data source

1. [] In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. [] Select **+ New item**. 

1. [] Scroll down to the Visualize data section and select **Real-Time Dashboard**.

    !IMAGE[x7jijw5q.jpg](instructions294573/x7jijw5q.jpg)


1. [] Enter +++RTADashboard@lab.LabInstance.Id+++ in the Real-Time Dashboard name field and then select **Create**.

    !IMAGE[ds83i6tz.jpg](instructions294573/ds83i6tz.jpg)

1. [] On the menu bar, select **New data source** and then select **Eventhouse / KQL Database**.

	!IMAGE[ezszkdfc.jpg](instructions294573/ezszkdfc.jpg)

1. [] In the OneLake catalog dialog, select **RTADemo@lab.LabInstance.Id** and then select **Connect**.

	!IMAGE[it7h8z9h.jpg](instructions294573/it7h8z9h.jpg)

1. In the Create new data source dialog, select **Add**.

	!IMAGE[abtzbdus.jpg](instructions294573/abtzbdus.jpg)

===
###Task 2: Create the Clicks by Hour tile


1. [] On the dashboard canvas, select **Add Tile**.

	!IMAGE[z1qopcx8.jpg](instructions294573/z1qopcx8.jpg)

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    //Clicks by hour
    events
    | where eventDate between (_startTime.._endTime) and eventType == "CLICK" 
    | summarize date_count = count() by bin(eventDate, 1h) 
    | render timechart  
    | top 30 by date_count 
    ```
	!IMAGE[6pme2o9v.jpg](instructions294573/6pme2o9v.jpg)

1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jpwlunhl.jpg](instructions294573/jpwlunhl.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.

    | Field | Value |
    |:---------|:---------|
    | Tile Name   | +++Clicks by Hour+++   |
    | Visual Type   | **Area chart**   |
    | Y Column   |  **date_count (long)**  |
    | X Column   | **eventDate (datetime)**  |

	!IMAGE[hfnxq9b6.jpg](instructions294573/hfnxq9b6.jpg)

1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.

	!IMAGE[4vu3zgr5.jpg](instructions294573/4vu3zgr5.jpg)

===
##Task 3: Create the Impressions by Hour tile


1. [] On the dashboard canvas menu bar, select **New Tile**.

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    //Impressions by hour
    events 
    | where eventDate between (_startTime.._endTime) and eventType == "IMPRESSION" 
    | summarize date_count = count() by bin(eventDate, 1h) 
    | render timechart  
    | top 30 by date_count 
    ```

1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.

    | Field | Value |
    |:---------|:---------|
    | Tile Name   | ++Impressions by Hour++   |
    | Visual Type   | Area chart   |
    | Y Column   |  date_count (long)  |
    | X Column   |  eventDate (datetime)  |

    !IMAGE[tora0l54.jpg](instructions294573/tora0l54.jpg)

1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.

===
<!--##Task 4: Create a tile that shows a map of impression locations

---
1. [] On the dashboard canvas menu bar, select **New Tile**.

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    //show map of impressions location
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "IMPRESSION" 
    | join external_table('products') on $left.productId == $right.ProductID 
    | project lon = geo_info_from_ip_address(ip_address).longitude, lat = geo_info_from_ip_address(ip_address).latitude, Name 
    | render scatterchart with (kind = map)
    ```

1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.

    | Field | Value |
    |:---------|:---------|
    | Tile Name   | ++Impression location++   |
    | Visual Type   |  Map  |
    |  Define location by  |  Latitude and Longitude  |
    |  Latitude Column  |  lat (dynamic) |
    |  Longitude Column  |  lon (dynamic)  |
    |  Label column  |  Name (string)  |

    !IMAGE[2i2pldzl.png](instructions294573/2i2pldzl.png)

1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.
-->

##Task 4: Create the Average Page Load time tile

1. [] On the dashboard canvas menu bar, select **New Tile**.

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    //Average Page Load time
    events 
    | where eventDate   between (_startTime.._endTime) and eventType == "IMPRESSION" 
    | summarize average_loadtime = avg(page_loading_seconds) by bin(eventDate, 1h) 
    | render linechart 
    ```
1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.


    | Field | Value |
    |:---------|:---------|
    | Tile Name   | ++Average Page Load time++   |
    | Visual Type   | Line chart |
    | Y Column   |  average_loadtime (real)  |
    | X Column   |  eventDate (datetime)  |

    !IMAGE[ky606158.png](instructions294573/ky606158.png)

1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.

===
##Task 5: Create the Impressions tile

1. [] On the dashboard canvas menu bar, select **New Tile**.

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    // Impressions
    let imp =  
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "IMPRESSION" 
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10) 
    | summarize imp_count = count() by dateOnly; 
    let clck =  
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "CLICK" 
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10) 
    | summarize clck_count = count() by dateOnly;
    imp  
    | join clck on $left.dateOnly == $right.dateOnly 
    | project selected_date = dateOnly , impressions = imp_count , clicks = clck_count, CTR = clck_count * 100 / imp_count
    ```
1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.



    | Field | Value |
    |:---------|:---------|
    | Tile Name   | ++Impressions++   |
    | Visual Type   | Stat   |
    | Value Column   |  impressions (long)  |

    !IMAGE[4ke9vx4w.png](instructions294573/4ke9vx4w.png)

1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.

===
##Task 6: Create the Clicks tile

1. [] On the dashboard canvas menu bar, select **New Tile**.

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    // clicks
    let imp =  
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "IMPRESSION" 
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10) 
    | summarize imp_count = count() by dateOnly; 
    let clck =  
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "CLICK" 
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10) 
    | summarize clck_count = count() by dateOnly;
    imp  
    | join clck on $left.dateOnly == $right.dateOnly 
    | project selected_date = dateOnly , impressions = imp_count , clicks = clck_count, CTR = clck_count * 100 / imp_count
    ```

1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.


    | Field | Value |
    |:---------|:---------|
    | Tile Name   | ++Clicks++   |
    | Visual Type   | Stat   |
    | Value Column   |  clicks (long)  |

    !IMAGE[6elbq42s.png](instructions294573/6elbq42s.png)


1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.

===
##Task 7: Create the CTR tile

1. [] On the dashboard canvas menu bar, select **New Tile**.

1. [] Delete any code in the query editor and enter the following query:

    ```KQL
    // CTR by date
    let imp =  
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "IMPRESSION" 
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10) 
    | summarize imp_count = count() by dateOnly; 
    let clck =  
    events 
    | where eventDate  between (_startTime.._endTime) and eventType == "CLICK" 
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10) 
    | summarize clck_count = count() by dateOnly;
    imp  
    | join clck on $left.dateOnly == $right.dateOnly 
    | project selected_date = dateOnly , impressions = imp_count , clicks = clck_count, CTR = clck_count * 100 / imp_count
    ```

1. [] Select **&#9655; Run** to execute the query.

	!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)

1. [] In the lower pane, select **+ Add visual**.

1. [] Configure the options for the tile by using the following values. Leave all other settings at default values.



    | Field | Value |
    |:---------|:---------|
    | Tile Name   | ++Click Through Rate++   |
    | Visual Type   | Stat   |
    | Value Column   |  CTR (long)  |

    !IMAGE[lrtzl15g.png](instructions294573/lrtzl15g.png)

1. [] At the top right of the page, select **Apply changes**. The dashboard canvas displays.

===
##Task 8: Save and test the dashboard

1. [] On the dashboard canvas menu bar, select **Save**.

1. [] On the dashboard canvas menu bar, select **&#x26A1; Set Alert** and then select **Clicks by hour**. 

	!IMAGE[op3cv032.jpg](instructions294573/op3cv032.jpg)

1. [] Configure the options for the Set alert panel by using the following values. Leave all other settings at default values.

    | Field | Value |
    |:---------|:---------|
    | Condition   | **Becomes greater than**   |
    | Vaue  | ++250++ |
    | Action   |  ***Message me in Teams**  |
    | Item   |  ***Create a new Item**  |


1. [] Select **Create**.

	!IMAGE[avu4wyk5.jpg](instructions294573/avu4wyk5.jpg)

    !IMAGE[lwck44zu.jpg](instructions294573/lwck44zu.jpg)

    >[!Note] It may take a minute or to to create the alert.
  

@lab.Activity(Question15)

@lab.Activity(Question16)

@lab.Activity(Question17)
===

##Task 9: Stop generating synthetic events

In Exercise 1, Task 6 you ran a notebook to generate synthetic events. The notebook is still running. In this task you will stop generating events.

1. [] In the left navigation pane, select **Workspaces** and then select **RealTimeAnalytics@lab.LabInstance.Id**.

1. []Select the **Generate Synthetic Events** notebook.

	!IMAGE[spmzzotj.jpg](instructions294573/spmzzotj.jpg)

1. [] On the menu bar for the notebook, select **Cancel All**.

    !IMAGE[4gl2dxz1.jpg](instructions294573/4gl2dxz1.jpg)


===

## That's All Folks!!

&#127881; Congratulations on completing this lab! 

Did you like it, did you not like it? Let us know in this short [Eval](https://forms.office.com/r/xhW3GAtAhi). Scan this QR Code to open the Eval form on your phone.

!IMAGE[QR Code](instructions294573/AssetsQRCode.png)

If you'd like to contribute to this lab or report a bug-issue, please send a Pull-request for us to review or submit the issue in our [GH repo](https://github.com/microsoft/FabricRTIWorkshop/).

Please take a moment to answer these questions. Providing answers to these questions will complete this lab. Your answers will help us understand the effectiveness of the lab and help to improve these labs as a whole.

@lab.ActivityGroup(completionsurvey)

===

## Continue your learning

- [Implement a Real-Time Intelligence Solution with Microsoft Fabric](https://learn.microsoft.com/training/paths/explore-real-time-analytics-microsoft-fabric/)
- [Real-Time Intelligence documentation in Microsoft Fabric](https://aka.ms/fabric-docs-rta)
- [Microsoft Fabric Updates Blog](https://aka.ms/fabricblog)
- [Get started with Microsoft Fabric](https://aka.ms/fabric-learn)
- [The mechanics of Real-Time Intelligence in Microsoft Fabric](https://youtube.com/watch?v=4h6Wnc294gA)
- [Real-Time Intelligence in Microsoft Fabric](https://youtube.com/watch?v=ugb_w3BTZGE)


### Thank you!

![logo](https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/RE1Mu3b?ver=5c31)

!IMAGE[ze9t6c6d.jpg](instructions294573/ze9t6c6d.jpg)
!IMAGE[zg24tt9g.jpg](instructions294573/zg24tt9g.jpg)
!IMAGE[1j0i0ruz.jpg](instructions294573/1j0i0ruz.jpg)
!IMAGE[2jvqp290.jpg](instructions294573/2jvqp290.jpg)
!IMAGE[2u1m2po6.jpg](instructions294573/2u1m2po6.jpg)
!IMAGE[3nosfod2.jpg](instructions294573/3nosfod2.jpg)
!IMAGE[3xn5xqkb.jpg](instructions294573/3xn5xqkb.jpg)
!IMAGE[04h76d7k.jpg](instructions294573/04h76d7k.jpg)
!IMAGE[04ve0zes.jpg](instructions294573/04ve0zes.jpg)
!IMAGE[4gl2dxz1.jpg](instructions294573/4gl2dxz1.jpg)
!IMAGE[4hr4vzo6.jpg](instructions294573/4hr4vzo6.jpg)
!IMAGE[4rlrawq1.jpg](instructions294573/4rlrawq1.jpg)
!IMAGE[4tyr78t4.jpg](instructions294573/4tyr78t4.jpg)
!IMAGE[4vu3zgr5.jpg](instructions294573/4vu3zgr5.jpg)
!IMAGE[5xewvfp2.jpg](instructions294573/5xewvfp2.jpg)
!IMAGE[6bnwo4gr.jpg](instructions294573/6bnwo4gr.jpg)
!IMAGE[6gihxtut.jpg](instructions294573/6gihxtut.jpg)
!IMAGE[6pme2o9v.jpg](instructions294573/6pme2o9v.jpg)
!IMAGE[6uwkauky.jpg](instructions294573/6uwkauky.jpg)
!IMAGE[7b91unk1.jpg](instructions294573/7b91unk1.jpg)
!IMAGE[7dvyrhuy.jpg](instructions294573/7dvyrhuy.jpg)
!IMAGE[7ixpjrc7.jpg](instructions294573/7ixpjrc7.jpg)
!IMAGE[8cbl8nqe.jpg](instructions294573/8cbl8nqe.jpg)
!IMAGE[8vnruiar.jpg](instructions294573/8vnruiar.jpg)
!IMAGE[9ghjiahd.jpg](instructions294573/9ghjiahd.jpg)
!IMAGE[9noomufy.jpg](instructions294573/9noomufy.jpg)
!IMAGE[9txbb3wj.jpg](instructions294573/9txbb3wj.jpg)
!IMAGE[21iphedj.jpg](instructions294573/21iphedj.jpg)
!IMAGE[30sn2vpp.jpg](instructions294573/30sn2vpp.jpg)
!IMAGE[48n2id64.jpg](instructions294573/48n2id64.jpg)
!IMAGE[77s8opow.jpg](instructions294573/77s8opow.jpg)
!IMAGE[83q3yoqs.jpg](instructions294573/83q3yoqs.jpg)
!IMAGE[932v63go.jpg](instructions294573/932v63go.jpg)
!IMAGE[002636oa.jpg](instructions294573/002636oa.jpg)
!IMAGE[09463lmf.jpg](instructions294573/09463lmf.jpg)
!IMAGE[38007zmy.jpg](instructions294573/38007zmy.jpg)
!IMAGE[a0lz2xyd.jpg](instructions294573/a0lz2xyd.jpg)
!IMAGE[a7rz3ild.jpg](instructions294573/a7rz3ild.jpg)
!IMAGE[abtzbdus.jpg](instructions294573/abtzbdus.jpg)
!IMAGE[anbu0tr0.jpg](instructions294573/anbu0tr0.jpg)
!IMAGE[avu4wyk5.jpg](instructions294573/avu4wyk5.jpg)
!IMAGE[b3egm6m1.jpg](instructions294573/b3egm6m1.jpg)
!IMAGE[bddphvxt.jpg](instructions294573/bddphvxt.jpg)
!IMAGE[bi5e5k8q.jpg](instructions294573/bi5e5k8q.jpg)
!IMAGE[ccz9y1ej.jpg](instructions294573/ccz9y1ej.jpg)
!IMAGE[co928lzu.jpg](instructions294573/co928lzu.jpg)
!IMAGE[cv2kkay2.jpg](instructions294573/cv2kkay2.jpg)
!IMAGE[cx14luow.jpg](instructions294573/cx14luow.jpg)
!IMAGE[d1wqf6li.jpg](instructions294573/d1wqf6li.jpg)
!IMAGE[dq6c7uoz.jpg](instructions294573/dq6c7uoz.jpg)
!IMAGE[ds83i6tz.jpg](instructions294573/ds83i6tz.jpg)
!IMAGE[e5nlpulb.jpg](instructions294573/e5nlpulb.jpg)
!IMAGE[eoun1ajk.jpg](instructions294573/eoun1ajk.jpg)
!IMAGE[f6vote52.jpg](instructions294573/f6vote52.jpg)
!IMAGE[fyhuzyiv.jpg](instructions294573/fyhuzyiv.jpg)
!IMAGE[g1pcj0mi.jpg](instructions294573/g1pcj0mi.jpg)
!IMAGE[g7ccy918.jpg](instructions294573/g7ccy918.jpg)
!IMAGE[g09soiio.jpg](instructions294573/g09soiio.jpg)
!IMAGE[gw8g0v8v.jpg](instructions294573/gw8g0v8v.jpg)
!IMAGE[h0uq5wfq.jpg](instructions294573/h0uq5wfq.jpg)
!IMAGE[h26sdxz0.jpg](instructions294573/h26sdxz0.jpg)
!IMAGE[hfnxq9b6.jpg](instructions294573/hfnxq9b6.jpg)
!IMAGE[hn84bmet.jpg](instructions294573/hn84bmet.jpg)
!IMAGE[hwzureji.jpg](instructions294573/hwzureji.jpg)
!IMAGE[iazk6bbj.jpg](instructions294573/iazk6bbj.jpg)
!IMAGE[iflep90m.jpg](instructions294573/iflep90m.jpg)
!IMAGE[it7h8z9h.jpg](instructions294573/it7h8z9h.jpg)
!IMAGE[iv8cvblc.jpg](instructions294573/iv8cvblc.jpg)
!IMAGE[iyuf0wvb.jpg](instructions294573/iyuf0wvb.jpg)
!IMAGE[j86g8wwd.jpg](instructions294573/j86g8wwd.jpg)
!IMAGE[jkilsa35.jpg](instructions294573/jkilsa35.jpg)
!IMAGE[jli8t4gc.jpg](instructions294573/jli8t4gc.jpg)
!IMAGE[jpwlunhl.jpg](instructions294573/jpwlunhl.jpg)
!IMAGE[k7ohx6nj.jpg](instructions294573/k7ohx6nj.jpg)
!IMAGE[k9aqekb6.jpg](instructions294573/k9aqekb6.jpg)
!IMAGE[l7zkwl9k.jpg](instructions294573/l7zkwl9k.jpg)
!IMAGE[lsfrlsvz.jpg](instructions294573/lsfrlsvz.jpg)
!IMAGE[lwck44zu.jpg](instructions294573/lwck44zu.jpg)
!IMAGE[lxzlprxc.jpg](instructions294573/lxzlprxc.jpg)
!IMAGE[m35ocjkb.jpg](instructions294573/m35ocjkb.jpg)
!IMAGE[mp376v0m.jpg](instructions294573/mp376v0m.jpg)
!IMAGE[n51l4xe8.jpg](instructions294573/n51l4xe8.jpg)
!IMAGE[nigkhz9n.jpg](instructions294573/nigkhz9n.jpg)
!IMAGE[nkxpuwui.jpg](instructions294573/nkxpuwui.jpg)
!IMAGE[o03vcify.jpg](instructions294573/o03vcify.jpg)
!IMAGE[op3cv032.jpg](instructions294573/op3cv032.jpg)
!IMAGE[oqu4dcwt.jpg](instructions294573/oqu4dcwt.jpg)
!IMAGE[osda6ovb.jpg](instructions294573/osda6ovb.jpg)
!IMAGE[otmxq3ya.jpg](instructions294573/otmxq3ya.jpg)
!IMAGE[pasfemer.jpg](instructions294573/pasfemer.jpg)
!IMAGE[pex918us.jpg](instructions294573/pex918us.jpg)
!IMAGE[ptiyu5g3.jpg](instructions294573/ptiyu5g3.jpg)
!IMAGE[pwnnm7s1.jpg](instructions294573/pwnnm7s1.jpg)
!IMAGE[qd1jxmr2.jpg](instructions294573/qd1jxmr2.jpg)
!IMAGE[qjkicm1o.jpg](instructions294573/qjkicm1o.jpg)
!IMAGE[qk8wavqd.jpg](instructions294573/qk8wavqd.jpg)
!IMAGE[qqajwq4k.jpg](instructions294573/qqajwq4k.jpg)
!IMAGE[rjiae4wx.jpg](instructions294573/rjiae4wx.jpg)
!IMAGE[rx5dvqc0.jpg](instructions294573/rx5dvqc0.jpg)
!IMAGE[snvqukdp.jpg](instructions294573/snvqukdp.jpg)
!IMAGE[spmzzotj.jpg](instructions294573/spmzzotj.jpg)
!IMAGE[sz8kkocg.jpg](instructions294573/sz8kkocg.jpg)
!IMAGE[t5ssnclk.jpg](instructions294573/t5ssnclk.jpg)
!IMAGE[tora0l54.jpg](instructions294573/tora0l54.jpg)
!IMAGE[touzmd11.jpg](instructions294573/touzmd11.jpg)
!IMAGE[tvcjdd1s.jpg](instructions294573/tvcjdd1s.jpg)
!IMAGE[tw6cf72i.jpg](instructions294573/tw6cf72i.jpg)
!IMAGE[u5in4i0k.jpg](instructions294573/u5in4i0k.jpg)
!IMAGE[u08av9j8.jpg](instructions294573/u08av9j8.jpg)
!IMAGE[unbfpk5g.jpg](instructions294573/unbfpk5g.jpg)
!IMAGE[uraa9y3x.jpg](instructions294573/uraa9y3x.jpg)
!IMAGE[v2jie5e7.jpg](instructions294573/v2jie5e7.jpg)
!IMAGE[wjcy9ej6.jpg](instructions294573/wjcy9ej6.jpg)
!IMAGE[wpr5bqks.jpg](instructions294573/wpr5bqks.jpg)
!IMAGE[wwtfqh12.jpg](instructions294573/wwtfqh12.jpg)
!IMAGE[wymqurnj.jpg](instructions294573/wymqurnj.jpg)
!IMAGE[x4qug7l8.jpg](instructions294573/x4qug7l8.jpg)
!IMAGE[x7jijw5q.jpg](instructions294573/x7jijw5q.jpg)
!IMAGE[xfyic3l3.jpg](instructions294573/xfyic3l3.jpg)
!IMAGE[xr59ok4i.jpg](instructions294573/xr59ok4i.jpg)
!IMAGE[xyjhk88c.jpg](instructions294573/xyjhk88c.jpg)
!IMAGE[yb5gjtwa.jpg](instructions294573/yb5gjtwa.jpg)
!IMAGE[yglc02gm.jpg](instructions294573/yglc02gm.jpg)
!IMAGE[yj7cobd7.jpg](instructions294573/yj7cobd7.jpg)
!IMAGE[z4gmtuf0.jpg](instructions294573/z4gmtuf0.jpg)!IMAGE[rpjl9eqa.png](instructions294573/rpjl9eqa.png)
!IMAGE[s6uurhtg.png](instructions294573/s6uurhtg.png)
!IMAGE[s39j3y48.png](instructions294573/s39j3y48.png)
!IMAGE[sbx3d2i8.png](instructions294573/sbx3d2i8.png)
!IMAGE[showAsTabMenu.png](instructions294573/showAsTabMenu.png)
!IMAGE[si295lnq.png](instructions294573/si295lnq.png)
!IMAGE[swcdxrg5.png](instructions294573/swcdxrg5.png)
!IMAGE[tx2q6kcy.png](instructions294573/tx2q6kcy.png)
!IMAGE[txcuu0t9.png](instructions294573/txcuu0t9.png)
!IMAGE[txfnleqr.png](instructions294573/txfnleqr.png)
!IMAGE[usf92gvg.png](instructions294573/usf92gvg.png)
!IMAGE[uw5ym4ga.png](instructions294573/uw5ym4ga.png)
!IMAGE[v4yarnka.png](instructions294573/v4yarnka.png)
!IMAGE[vuntpic7.png](instructions294573/vuntpic7.png)
!IMAGE[w30b6w27.png](instructions294573/w30b6w27.png)
!IMAGE[y2d0o7kb.png](instructions294573/y2d0o7kb.png)
!IMAGE[y27she1c.png](instructions294573/y27she1c.png)
!IMAGE[y76dmnbe.png](instructions294573/y76dmnbe.png)
!IMAGE[yuxrbq5f.jpg](instructions294573/yuxrbq5f.jpg)
!IMAGE[zbf8bl06.png](instructions294573/zbf8bl06.png)
!IMAGE[zlfsgj53.png](instructions294573/zlfsgj53.png)
!IMAGE[0fuhsy58.png](instructions294573/0fuhsy58.png)
!IMAGE[0qmftson.png](instructions294573/0qmftson.png)
!IMAGE[1aiiy59g.png](instructions294573/1aiiy59g.png)
!IMAGE[1bwrfgw3.png](instructions294573/1bwrfgw3.png)
!IMAGE[1i2is346.png](instructions294573/1i2is346.png)
!IMAGE[1uy65eoz.png](instructions294573/1uy65eoz.png)
!IMAGE[1xvaeh0x.png](instructions294573/1xvaeh0x.png)
!IMAGE[2i2pldzl.png](instructions294573/2i2pldzl.png)
!IMAGE[2l3jo11w.png](instructions294573/2l3jo11w.png)
!IMAGE[3d1dzvet.png](instructions294573/3d1dzvet.png)
!IMAGE[3oyop48h.png](instructions294573/3oyop48h.png)
!IMAGE[3rnbr3fh.png](instructions294573/3rnbr3fh.png)
!IMAGE[4ke9vx4w.png](instructions294573/4ke9vx4w.png)
!IMAGE[4t4hfasf.png](instructions294573/4t4hfasf.png)
!IMAGE[5frsw5j2.png](instructions294573/5frsw5j2.png)
!IMAGE[6elbq42s.png](instructions294573/6elbq42s.png)
!IMAGE[6esjvdk8.png](instructions294573/6esjvdk8.png)
!IMAGE[6eynpss5.png](instructions294573/6eynpss5.png)
!IMAGE[6qr1h16x.png](instructions294573/6qr1h16x.png)
!IMAGE[7ralgety.png](instructions294573/7ralgety.png)
!IMAGE[7ttnxb18.png](instructions294573/7ttnxb18.png)
!IMAGE[7wiz0cvs.jpg](instructions294573/7wiz0cvs.jpg)
!IMAGE[8yy6u6os.png](instructions294573/8yy6u6os.png)
!IMAGE[9id046vr.png](instructions294573/9id046vr.png)
!IMAGE[9j9w31al.png](instructions294573/9j9w31al.png)
!IMAGE[9v9y2c0t.png](instructions294573/9v9y2c0t.png)
!IMAGE[38i42tx1.png](instructions294573/38i42tx1.png)
!IMAGE[40nqiuil.png](instructions294573/40nqiuil.png)
!IMAGE[44y9mir1.png](instructions294573/44y9mir1.png)
!IMAGE[75q3q99w.png](instructions294573/75q3q99w.png)
!IMAGE[99nbgvkc.png](instructions294573/99nbgvkc.png)
!IMAGE[715v8dwa.png](instructions294573/715v8dwa.png)
!IMAGE[ActiveModal.png](instructions294573/ActiveModal.png)
!IMAGE[ActivityAddName.png](instructions294573/ActivityAddName.png)
!IMAGE[AddressDeactivate.png](instructions294573/AddressDeactivate.png)
!IMAGE[ai11b7bj.png](instructions294573/ai11b7bj.png)
!IMAGE[architectureDiagram.png](instructions294573/architectureDiagram.png)
!IMAGE[AssetsQRCode.png](instructions294573/AssetsQRCode.png)
!IMAGE[AziosConnection.png](instructions294573/AziosConnection.png)
!IMAGE[b2utr3il.png](instructions294573/b2utr3il.png)
!IMAGE[bs21l6dl.png](instructions294573/bs21l6dl.png)
!IMAGE[c0hngha4.png](instructions294573/c0hngha4.png)
!IMAGE[CancelAllBTN.png](instructions294573/CancelAllBTN.png)
!IMAGE[CloneAddress.png](instructions294573/CloneAddress.png)
!IMAGE[CloneCustomer.png](instructions294573/CloneCustomer.png)
!IMAGE[CloneHeader.png](instructions294573/CloneHeader.png)
!IMAGE[ConnTypeAzSQLDB.png](instructions294573/ConnTypeAzSQLDB.png)
!IMAGE[copilot.png](instructions294573/copilot.png)
!IMAGE[d3izhlil.png](instructions294573/d3izhlil.png)
!IMAGE[DBSchema.png](instructions294573/DBSchema.png)
!IMAGE[de2flf0m.png](instructions294573/de2flf0m.png)
!IMAGE[DeactivateActivity.png](instructions294573/DeactivateActivity.png)
!IMAGE[DestSalesOrderDetail.png](instructions294573/DestSalesOrderDetail.png)
!IMAGE[dp3zrpek.png](instructions294573/dp3zrpek.png)
!IMAGE[dvm1b8oa.png](instructions294573/dvm1b8oa.png)
!IMAGE[dxk2o7m2.png](instructions294573/dxk2o7m2.png)
!IMAGE[e8h3x4pv.png](instructions294573/e8h3x4pv.png)
!IMAGE[EnableRTDashboards.png](instructions294573/EnableRTDashboards.png)
!IMAGE[EventhouseSamples.png](instructions294573/EventhouseSamples.png)
!IMAGE[FabricWorkspace.png](instructions294573/FabricWorkspace.png)
!IMAGE[fdrdaxw8.png](instructions294573/fdrdaxw8.png)
!IMAGE[fkxwpmgb.jpg](instructions294573/fkxwpmgb.jpg)
!IMAGE[fr5af46p.png](instructions294573/fr5af46p.png)
!IMAGE[fu18l2a3.png](instructions294573/fu18l2a3.png)
!IMAGE[GoldAddressRecordCount.png](instructions294573/GoldAddressRecordCount.png)
!IMAGE[gp680prf.png](instructions294573/gp680prf.png)
!IMAGE[he3yvdk3.png](instructions294573/he3yvdk3.png)
!IMAGE[HEADAccessories.png](instructions294573/HEADAccessories.png)
!IMAGE[hedywkhh.png](instructions294573/hedywkhh.png)
!IMAGE[HimiwayBikes.png](instructions294573/HimiwayBikes.png)
!IMAGE[hxw3s6q1.png](instructions294573/hxw3s6q1.png)
!IMAGE[i84x1ckj.png](instructions294573/i84x1ckj.png)
!IMAGE[ic2jtcjz.png](instructions294573/ic2jtcjz.png)
!IMAGE[iej7i3dx.png](instructions294573/iej7i3dx.png)
!IMAGE[ig5w4wqx.png](instructions294573/ig5w4wqx.png)
!IMAGE[Industries.png](instructions294573/Industries.png)
!IMAGE[k2i0zrwb.jpg](instructions294573/k2i0zrwb.jpg)
!IMAGE[KafkaEndpoint.png](instructions294573/KafkaEndpoint.png)
!IMAGE[KeysReq.png](instructions294573/KeysReq.png)
!IMAGE[kmk12u86.png](instructions294573/kmk12u86.png)
!IMAGE[kpd1ex1s.png](instructions294573/kpd1ex1s.png)
!IMAGE[KQLQuerySetRun.png](instructions294573/KQLQuerySetRun.png)
!IMAGE[ky606158.png](instructions294573/ky606158.png)
!IMAGE[l8hs7g0j.png](instructions294573/l8hs7g0j.png)
!IMAGE[l9u0gxmn.png](instructions294573/l9u0gxmn.png)
!IMAGE[lrtzl15g.png](instructions294573/lrtzl15g.png)
!IMAGE[m5icm7rr.png](instructions294573/m5icm7rr.png)
!IMAGE[m87plj38.png](instructions294573/m87plj38.png)
!IMAGE[MappingTab.png](instructions294573/MappingTab.png)
!IMAGE[mb2a9aqb.png](instructions294573/mb2a9aqb.png)
!IMAGE[OneLakeAvailabilityEditIcon.png](instructions294573/OneLakeAvailabilityEditIcon.png)
!IMAGE[QR_Code.jpg](instructions294573/QR_Code.jpg)
!IMAGE[RealTimeSources.png](instructions294573/RealTimeSources.png)
!IMAGE[RTAnalysisDashboard.png](instructions294573/RTAnalysisDashboard.png)
!IMAGE[RTIComponents.png](instructions294573/RTIComponents.png)
!IMAGE[RunButton.png](instructions294573/RunButton.png)
!IMAGE[SalesLT.AddressRecords.png](instructions294573/SalesLT.AddressRecords.png)
!IMAGE[SalesLT.SalesOrderDetail.png](instructions294573/SalesLT.SalesOrderDetail.png)
!IMAGE[SalesOrderDetailMapping.png](instructions294573/SalesOrderDetailMapping.png)
!IMAGE[Schema_Lakhouse.png](instructions294573/Schema_Lakhouse.png)
!IMAGE[SilverAddressIngestionDate.png](instructions294573/SilverAddressIngestionDate.png)
!IMAGE[StopAllNotebooks.png](instructions294573/StopAllNotebooks.png)
!IMAGE[TypeTextIcon.png](instructions294573/TypeTextIcon.png)
!IMAGE[UserStartTrial.png](instructions294573/UserStartTrial.png)
!IMAGE[WorkspaceManageAccess.jpg](instructions294573/WorkspaceManageAccess.jpg)
!IMAGE[mr18r8ez.png](instructions294573/mr18r8ez.png)
!IMAGE[mwl39tw4.png](instructions294573/mwl39tw4.png)
!IMAGE[n0roztd2.png](instructions294573/n0roztd2.png)
!IMAGE[newTabQueySet.png](instructions294573/newTabQueySet.png)
!IMAGE[o8mmv059.png](instructions294573/o8mmv059.png)
!IMAGE[onxzdrwg.png](instructions294573/onxzdrwg.png)
!IMAGE[oodcqykj.png](instructions294573/oodcqykj.png)
!IMAGE[owai26ix.png](instructions294573/owai26ix.png)
!IMAGE[p3b0hm51.png](instructions294573/p3b0hm51.png)
!IMAGE[pipelineName.png](instructions294573/pipelineName.png)
!IMAGE[pydcdbdt.png](instructions294573/pydcdbdt.png)
!IMAGE[qrtgqp7t.png](instructions294573/qrtgqp7t.png)
!IMAGE[r106iwg0.png](instructions294573/r106iwg0.png)
!IMAGE[r928hx5w.png](instructions294573/r928hx5w.png)