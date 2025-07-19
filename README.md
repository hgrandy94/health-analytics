# Health Analytics Tutorial
## Preparing data and building data visualizations in Microsoft Fabric

### Prerequisites
1. A basic understanding of what capabilities are available within Microsoft Fabric. If you're not familiar with Fabric, the two resources below are a great start!

      https://learn.microsoft.com/en-us/fabric/fundamentals/microsoft-fabric-overview
  
      https://learn.microsoft.com/en-us/training/paths/get-started-fabric/ 
  
2. A Microsoft Fabric capacity - a trial license is sufficient: https://learn.microsoft.com/en-us/fabric/fundamentals/fabric-trial

In this tutorial, you'll learn how to transform raw data csv using Dataflows Gen2 in Microsoft Fabric to structured tables in a Fabric Lakehouse. Once you've landed the data in the Lakehouse, you'll then create a semantic model which is the backend that powers Power BI reports. The last step is visualizing the cleansed data in a Power BI report.

Below is an explanation of the assets you'll find in this repository:
- data/
  
  Contains select raw data files (csv format) from the Nova Scotia Open Data Portal website: https://data.novascotia.ca/browse?sortBy=alpha&page=1&pageSize=20
  
- instructions/
  
  Contains the step-by-step instructions for transforming the raw data, loading to Lakehouse tables, building a semantic model, and creating a Power BI report.

- solution/
  
  If you're stuck on any of the steps, the clean data files (csv format), and a Power BI file (.pbix) are available for your reference.
