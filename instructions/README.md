# Tutorial Instructions
## Tutorial Overview
The diagram below provides a high-level overview of the steps we'll follow in this tutorial.

![tutorial_overview](imgs/tutorial_overview.png)

## Part 1: Workspace and Lakehouse Setup
1. In this repository, go to health-analytics/data and download all files in the folder.
2. Navigate to https://app.fabric.microsoft.com/and sign in.
3. Create a new workspace. Click on Workspaces >> +New Workspace, as shown in the screenshot below. You can name your workspace whatever you'd like (recommendation: Health Analytics Tutorial).

![create_workspace](imgs/1_create_workspace.png)

4. In your workspace, click on +New item and search for "Lakehouse." You can name the Lakehouse whatever you'd like (recommendation: health_lakehouse).

![create_lakehouse](imgs/2_create_lakehouse.png)

5. Now, we'll upload the data files downloaded in step 1 to the Lakehouse Files. First, create a new subfolder called "raw_nshealth_analytics:"

![create_subfolder](imgs/3_create_subfolder.png)

6. Open the subfolder, click on Get data >> Upload files. Upload all the files downloaded in step 1. 

![upload_lakehouse](imgs/4_upload_lakehouse.png)

## Part 2: Data Transformation / Cleansing
7. Return to your workspace, click on +New item, and search for "Dataflow Gen2." Note that the search results will return items under "Get data" and "Prepare data," but both will create a blank Dataflow Gen2 (choosing either option is fine). Name the Dataflow Gen2 Action for Health DFG2, and and click Create.

![create_dfg2](imgs/5_create_dfg2.png)

Note: As we won't be using Git integration or deployment pipelines in this tutorial, there will be no impact if you uncheck the box, as shown below.
![uncheck_box](imgs/5a_uncheck_box.png)

8. The blank DFG2 will open. Click on Get data >> More, then search for and select "Lakehouse."
![get data](imgs/6_get_data.png)

9. Choose the Workspace you created in step 3, then click on the Lakehouse you created in step 4. Open the "Files" folder and select "Action_for_Health_20250718.csv." Click Create.
![connect_data](imgs/7_action_for_health_data.png) 

## Part 3: Semantic Model Preparation

## Part 4: Data Visualization
