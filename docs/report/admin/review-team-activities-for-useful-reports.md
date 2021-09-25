---
title: Review team activities to support useful reports
titleSuffix: Azure DevOps Server
description: Ensure basic operations are performed by team members to generate useful SQL Server reports when working in Team Foundation Server    
ms.assetid: 46456FBF-EECC-4096-9A98-3A9457F97EB2  
ms.technology: devops-analytics
ms.topic: conceptual
ms.author: kaelli
author: KathrynEE
monikerRange: '< azure-devops' 
ms.date: 11/19/2018
---

# Review team activities to support useful reports 

[!INCLUDE [temp](../includes/tfs-report-platform-version.md)]


> [!IMPORTANT]
>**Feature availability**: You can only add a report server to an on-premises TFS.  If you're using Azure DevOps, adding a report server isn't a supported option, instead, you can use [PowerBI](../powerbi/overview.md).

This is the fourth task in the four-task sequence to add reports to your team project. To generate useful reports, team members must perform certain tasks. this article summarizes those tasks.     

[![Add a report server](media/step-1-add-a-report-server.png)](add-a-report-server.md)
[![Upload reports](media/step-2-upload-reports.png)](upload-reports.md)
[![Grant permissions](media/step-3-grant-permissions.png)](grant-permissions-to-reports.md) 
[![Review team activities](media/step-4-review-team-activities.png)](review-team-activities-for-useful-reports.md)


Now that you've uploaded reports, how do you use them to track progress, gain insight, and improve processes?

First, make sure your team is performing the activities that create the data that these reports use. Your team is probably performing most of these activities already.

Here's a summary of the reports that TFS provides and the team activities that are associated with them. Over time, you can use these reports to see trends and identify which practices and processes require more attention to deliver desired results.

## Monitor code quality
Build reports track the quality of software under development. By defining tests to run automatically as part of each build definition and instrumenting tests to gather code coverage data, you can gain insight about the quality of the builds, tests, and code. 

:::row:::
   :::column span="1":::
   
   **Build and test activities** 

   1. [Configure a build system](../../pipelines/agents/agents.md)
   2. [Get started with CI/CD](/azure/devops/pipelines/create-first-pipeline)
   3. [Run tests in your build process](../../pipelines/ecosystems/dotnet-core.md#run-your-tests)
   4. (Optional) [Rate completed builds](/previous-versions/ms181734(v=vs.140)) to populate the Build Quality dimension.

   :::column-end:::
   :::column span="1":::
   
   **Build reports**
   
   - [Build Quality Indicators](../sql-reports/build-quality-indicators-report.md) (Agile and CMMI only)
   - [Build Success Over Time](../sql-reports/build-success-over-time-report.md) (pictured)
   - [Build Summary](../sql-reports/build-summary-report.md)
   
   :::column-end:::
:::row-end:::


> [!IMPORTANT]  
> Build reports are only applicable for XAML builds, which are deprecated for TFS 2018 and later versions. If your build process isn't based on XAML builds, this report and the TFS Warehouse for builds won't yield any meaningful data.  


**Sample build success over time report**  

![Sample build summary report](media/IC665009.png)  

## Monitor progress
Project management reports provide insight into how much work the team is tackling within a sprint or release, and the rate of their progress. By linking work items and updating specific fields as work is performed, you can track the progress of individual stories and be able to more accurately estimate future activities. 

:::row:::
   :::column span="1":::
   
   **Work item tracking activities**

   1. [Create the backlog](../../boards/backlogs/create-your-backlog.md).   
      - Create product backlog items and specify the **Effort** (Scrum).
      - Create user stories and specify the **Story Points** (Agile).
      - Create requirements and specify the **Size** (CMMI).  
   2. [Work in sprints](../../boards/sprints/assign-work-sprint.md). Assign backlog items to sprints, create tasks and link them to parent backlog items, and assign to a team member.
   3. [Update Remaining Work for tasks](../../boards/sprints/task-board.md). For Agile and CMMI team projects, update **Completed Work** as well.
      > [!Tip]  
      > The only report that references **Original Estimate** is [Status on All Iterations](../sql-reports/status-on-all-iterations-report.md).
   4. Create test cases and bugs, link them to their parent backlog item, and update their **State**.
   5. (Optional) Assign work items to areas to filter reports.

   :::column-end:::
   :::column span="1":::
   
   **Project management (Scrum) reports**
   
   - [Backlog Overview (Scrum)](../sql-reports/backlog-overview-scrum.md)
   - [Release Burndown](../sql-reports/release-burndown.md)
   - [Sprint Burndown (Scrum)](../sql-reports/sprint-burndown-scrum.md)
   
   **Project management (Agile and CMMI) reports**
   
   - [Burndown and Burn Rate](../sql-reports/burndown-and-burn-rate-report.md)
   - [Remaining Work](../sql-reports/remaining-work-report.md)
   - [Requirements Overview (CMMI)](../sql-reports/requirements-overview-report-cmmi.md)
   - [Requirements Progress (CMMI)](../sql-reports/requirements-progress-report-cmmi.md)
   - [Status on All Iterations](../sql-reports/status-on-all-iterations-report.md)
   - [Stories Overview (Agile)](../sql-reports/stories-overview-report-agile.md)
   - [Stories Progress (Agile)](../sql-reports/stories-progress-report-agile.md)
   - [Unplanned Work](../sql-reports/unplanned-work.md)
   
   :::column-end:::
:::row-end:::


**Sample stories overview report**  
![Sample stories overview report](media/IC665011.png)  


## Monitor test plans and bug tracking
Test planning reports support monitoring the test progress and coverage of backlog items or user stories. Bug tracking reports illustrate the team's capacity to find and resolve bugs.

:::row:::
   :::column span="3":::
   
   **Test planning and bug tracking activities**

   1. Define test plans and test cases, and update their **State** as work progresses.
   2. [Mark the results of each validation step in manual tests](../../test/run-manual-tests.md) as either passed or failed.
   3. **Create bugs**, specify the **Priority** and **Severity**, assign to a team member, and update the **State**.
   4. (Optional) Assign test cases and bugs to areas and iterations to filter reports.

   :::column-end:::
   :::column span="1":::
   
   **Bug and test reports**
   
   - [Bug Status](../sql-reports/bug-status-report.md)
   - [Bug Trends](../sql-reports/bug-trends-report.md)
   - [Reactivations](../sql-reports/reactivations-report.md)
   - [Test Case Readiness](../sql-reports/test-case-readiness-report.md)
   - [Test Plan Progress](../sql-reports/test-plan-progress-report.md)
   
   :::column-end:::
:::row-end:::


**Sample test plan progress report**   
![Sample test plan progress report](media/IC665012.png)  

## Q & A
<!-- BEGINSECTION class="md-qanda" -->


### Q: Do reports handle stories and substories or tasks and subtasks?
**A:**  Yes, you can subdivide stories or backlog items as well as tasks, creating a nested hierarchy of both backlog items and tasks. You can nest items several levels deep. If you subdivide a task into subtasks, specify hours only for the subtasks. These hours are rolled up as summary values for the parent task and their parent backlog item. To correct reports you believe are in error, see [Address inaccuracies published for summary values](../sql-reports/address-inaccuracies-published-for-summary-values.md).

### Q: Which reports depend on linking work items?
**A:**  The overview and progress reports depend on linking tasks, test cases, and bugs to backlog items. You must link these items using the parent-child link for tasks and bugs and the Tested By link for test cases.

### Q: Are these reports the same as the charts that appear in the web portal?
 **A:**  While some reports do display similar information, such as sprint burndown and velocity or status on all iterations, these reports are formatted differently and support additional filters. 

### Q: Do you want to create additional product areas or release milestones?
**A:**  See [Modify areas or iterations](../../organizations/settings/set-area-paths.md).

### Q: Do you want to bulk edit work items to assign them to an area, iteration, team member, or priority?
**A:**  See [Bulk modify work items](../../boards/backlogs/bulk-modify-work-items.md).

### Q: Do you want to add a field to track additional data?
**A:**  See [Add or modify a work item field to support reporting](../../reference/xml/add-or-modify-work-item-fields-to-support-reporting.md).  

<!-- ENDSECTION -->