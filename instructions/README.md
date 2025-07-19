# Tutorial Instructions
## Tutorial Overview
The diagram below provides a high-level overview of the steps we'll follow in this tutorial.

Note: Zones.csv is missing from this diagram, and the Community Health csv has been removed from the tutorial.

![tutorial_overview](imgs/tutorial_overview.png)

## Part 1: Workspace and Lakehouse Setup
1. In this repository, go to health-analytics/data and download all files in the folder.
2. Navigate to https://app.fabric.microsoft.com/ and sign in.
3. Create a new workspace. Click on Workspaces >> +New Workspace, as shown in the screenshot below. You can name your workspace whatever you'd like (recommendation: Health Analytics Tutorial).

![create_workspace](imgs/1_create_workspace.png)

4. In your workspace, click on +New item and search for "Lakehouse." You can name the Lakehouse whatever you'd like (recommendation: health_lakehouse).

![create_lakehouse](imgs/2_create_lakehouse.png)

5. Now, we'll upload the data files downloaded in step 1 to the Lakehouse Files. First, create a new subfolder called "raw_nshealth_analytics:"

![create_subfolder](imgs/3_create_subfolder.png)

6. Open the subfolder, click on Get data >> Upload files. Upload all the files downloaded in step 1. 

![upload_lakehouse](imgs/4_upload_lakehouse.png)

## Part 2: Data Transformation / Cleansing

#### Action for Health Dataset
7. Return to your workspace, click on +New item, and search for "Dataflow Gen2." Note that the search results will return items under "Get data" and "Prepare data," but both will create a blank Dataflow Gen2 (choosing either option is fine). Name the Dataflow Gen2 Action for Health DFG2, and and click Create.

![create_dfg2](imgs/5_create_dfg2.png)

Note: As we won't be using Git integration or deployment pipelines in this tutorial, there will be no impact if you uncheck the box, as shown below.
![uncheck_box](imgs/5a_uncheck_box.png)

8. The blank DFG2 will open. Click on Get data >> More, then search for and select "Lakehouse."
![get data](imgs/6_get_data.png)

9. Choose the Workspace you created in step 3, then click on the Lakehouse you created in step 4. Open the "Files" folder and select "Action_for_Health_20250718.csv." Click Create.
![connect_data](imgs/7_action_for_health_data.png) 

10. Let's fix the data quality issues with this file!
    a. The headers didn't come through properly, so navigate to Transform>>Use first row as headers to fix this.
    ![promote_first_row](imgs/8_promot_first_row.png)

    b. Columns Zone, Type, and CTAS all have blank values, which is not ideal for downstream processing. Let's replace all blank cells with "Unknown." In the Transform tab, click on Repalce values. Leave "Value to find" blank, and enter "Unknown" in the "Replace with" box, as shown in the screenshot below.
    ![replace_blanks](imgs/9_replace_values.png)

    c. In this dataset, we have the zone name, i.e. Central, Western, etc., but not the associated Zone ID which is preferred for table joins. Let's create a calculated column to get the Zone ID based on the Zone column. In the ribbon, click on Add column>>Custom column.
    ![calculated_col](imgs/10_custom_col.png)

    We need to enter a formula using the DAX expression language to implement the logic. Copy and paste the following formula into the box. Review the screenshot below for comparison, and click OK.
    
    ```
    if [Zone] = "Western" then 1 else if [Zone] = "Northern" then 2 else if [Zone] = "Eastern" then 3 else if [Zone] = "Central" then 4 else 5
    ```

    ![custom_formula](imgs/11_custom_formula.png)

11. We've fixed the data quality issues with the Action for Health dataset! Now, we have to configure the data destination to save the clean data to a Lakehouse table. In the bottom right-hand corner, select Data destination>>Lakehouse.
Note: In the screenshot, the Lakehouse is already configured as a data destination. You will see "No data destination" in your environment.

![data_destination](imgs/12_data_dest.png)

Leave the default connection credentials and click Next. Choose the Lakehouse created in step 4, and update the Table name to "ActionForHealthTbl." Click Next.

![new_tbl](imgs/13_create_new_tbl.png)

12. Return to the Home tab of your DFG2, and click on Save & run. Once the run is complete, verify that the ActionForHealthTbl table is in your Lakehouse.

![save_and_run](imgs/14_save_and_run.png)

#### Diabetes Prevalence Dataset
13. Repeat steps 7-9, this time for the "Diabetes_Crude_Prevalence_20250718.csv." In step 9, name the DFG2 "Diabetes Prevalence DFG2."

14. Let's fix the data quality issues!
    
    a. The headers did not come through properly. As in 10a, click on Transform>>Use first row as headers. Refer back to the screenshot in that step if needed.

    b. The zones in this file are in a format that isn't compatible with our other tables. Let's split the Zone column using the " - " delimiter. Click on the Zone column and navigate to Transform>>Split column>>By delimiter.

    ![split_col](imgs/15_split_col.png)

    In the Separator drop-down menu, choose the Custom option.
    
    ![custom_split](imgs/15a_split_custom.png)

    Enter " - " (without the quotations) as the Separator and click OK.

    ![custom_sep](imgs/15b_separator.png)

    The newly split columns need to be renamed. Rename Zone.1 to Zone, and Zone.2 to ZoneName. This can be done by double clicking on the column header.

    ![new_columns](imgs/15c_new_cols.png)

    c. As in 10c, we need to add a calculated column to get the Zone ID. Follow the same steps as in 10c, including copying the DAX formula, to generate the Zone ID column.

    d. Validate all steps have been completed by reviewing the screenshot below (ignore the "Changed column type step, and initial Navigation steps).

    ![diabetes_dataflow](imgs/16_diabetes_dfg2.png)

15. Now that the data is clean, we need to configure the Lakehouse destination and save as a table as in steps 11 and 12. Follow these same steps, this time naming the table, "DiabetesPrevalenceTbl."

#### Accessing Primary Care Dataset
16. Repeat steps 7-9, this time for the "Accessing_Primary_Care_in_Nova_Scotia_20250718.csv." In step 9, name the DFG2 "Primary Care DFG2."

17. Let's fix the data quality issues!
    a. The headers did not come through properly. As in 10a, click on Transform>>Use first row as headers. Refer back to the screenshot in that step if needed.

    b. Columns CTAS and Hospital all have blank values, which is not ideal for downstream processing. Let's replace all blank cells with "Unknown." In the Transform tab, click on Repalce values. Leave "Value to find" blank, and enter "Unknown" in the "Replace with" box. Refer to step 10b for a screenshot if needed.

    c. As in 10c, we need to add a calculated column to get the Zone ID. Follow the same steps as in 10c, including copying the DAX formula, to generate the Zone ID column.

    d. Validate all steps have been completed by reviewing the screenshot below (ignore the "Changed column type step, and initial Navigation steps).
    ![primary_care_dataflow](imgs/17_primary_care_dfg2.png)

18. Now that the data is clean, we need to configure the Lakehouse destination and save as a table as in steps 11 and 12. Follow these same steps, this time naming the table, "PrimaryCareTbl."

#### Zones Dataset
 This file was created to act as a bridging table between multiple tables with Zone ID. Because of this, we know there are no data quality issues, and can take advantage of the Fabric Lakehouse user interface to load Zones into a delta table. In your lakehouse, hover over Zones.csv, click on the ellipsis, then Load to tables>>New table. Name the table Zones, leave the default settings, and click Load.

 ![ui_load_table](imgs/18_ui_load_table.png)

## Part 3: Semantic Model Preparation
19. Verify that there are four tables in your Lakehouse:

    a. ActionForHealthTbl

    b. DiabetesPrevalenceTbl

    c. PrimaryCareTbl

    d. Zones

20. Let's configure the semantic model, which is the backend for our data visualizations. From your Lakehouse, click on "New semantic model." Note: The Lakehouse in the screenshot below contains additional tables that are not required for this tutorial. The required tables are indicated by the red arrow.

![create_sm](imgs/19_lakehouse_tbls.png)

21. Under semantic model name, enter "health_analytics_sm". Below, expan "dbo" and "Tables." Click on the four tables mentioned in step 19 - we want to add all of these to our semantic model. Click on Confirm. Note: Tables PrimaryCareTbl and Zones are not shown in the screenshot below, but should be included in your semantic model.

![configure_semantic_model](imgs/20_configure_semantic_model.png)

22. Navigate to the semantic model, and ensure you are in Editing mode.

![edit_semantic_model](imgs/21_edit_semantic_model.png)

23. Create the following joins. This can be done by dragging the column name from one table to the appropriate column name on another table. Fabric will auto-detect the relationship type for you, but it's always a good idea to double check it's correct! Alternatively, you can use the "Manage relationships" option in the ribbon and select tables and columns using a drop-down menu.

    a. ActionForHealthTbl.ZoneID on Zones.ZoneID (many-to-one relationship)

    b. DiabetesPrevalenceTbl.ZoneID on Zones.ZoneID (many-to-one relationship)

    c. PrimaryCareTbl.ZoneID on Zones.ZoneID (many-to-one relationship)

![configure_relationships](imgs/22_configure_relationships.png)

![verify_relationship](imgs/23_verify_relationships.png)

Note: Importantly, you can edit these relationships at any time! If you double click on any of the relationships (line between tables), you can make modifications as needed. Modifications can also be made using the "Manage relationships" option.

Note: This data model does not reflect and optimal star schema structure which is highly recommended for Power BI reporting. The purpose of this tutorial is to get familiar with Microsoft Fabric workloads, not to create the best possible data model.

## Part 4: Data Visualization

24. Finally, we can build our Power BI report! From your semantic model, go to File>>Create new report.

![create_report](imgs/24_create_report.png)

25. A blank Power BI report will load on your screen. First, let's give the report a name and save it so that you don't lose your work! Got to File>>Save, and name the report "Health Analytics Report."

![save_report](imgs/25_save_report.png)

26. After saving, your blank report will open in Viewing mode. Return to Edit mode by clicking on Edit (look for the pencil icon).

27. Build your report! You are welcome to create whatever you like for this component. Below, some ideas and screenshots are available to help you get started.

*Tip*: Due to the nature of the data, it is suggested to create a report with three pages - one for ActionForHealthTbl, one for DiabetesPrevalenceTbl, and one for PrimaryCareTbl. This is suggested since these datasets are largely unrelated. From the perspective of presenting information correctly, it is best to keep the reports separately.

a. Use the Table visual to list all metrics tracked in the ActionForHealthTbl.
    
b. Use the Slicer visual to create filters on your reports. For example, drag "Zone" into the Slicer visual, allowing users to view the data by Zone.

c. Check out the sample report below, noting that this was generated from a different dataset (use it for ideas!). This report uses the following visuals:

    i. Card visuals for showing target metrics
    
    ii. Slicers for enabling the filtering of Department and Year
    
    iii. Line chart for revenue over time
    
    iv. Line and clustered column chart for cost of care delivery

    v. Stacked bar chart for patients by region

    vi. Pie chart for total facilities provided

    vii. Stacked bar chart for total system capacity

![sample_report](imgs/26_example_report.png)

d. Feeling ambitious? Start learning about creating measures in Power BI! https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-tutorial-create-measures

Questions to consider:

- What other data could I bring in to gain better insights?

- What insights can I learn/share about primary care access?

- Can I bring in geospatial data to better understand and visualize diabetes prevalence?