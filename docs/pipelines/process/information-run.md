---
title: Pipeline information runs
description: What are pipeline information runs
ms.topic: reference
ms.assetid: 96a52d0d-5e01-4b30-818d-1893387522cd
ms.author: sandrica
author: silviuandrica
ms.date: 02/03/2022
monikerRange: 'azure-devops'
---

# Pipeline information runs

[!INCLUDE [header](../includes/information-run-include.md)]

## When is a pipeline information run created?

The first step of running a YAML pipeline is to retrieve its source code. When this step fails, the system creates an information run. These runs are created only if the pipeline's code is in a GitHub or BitBucket repository.

A pipeline may run in response to:
- Pushes to branches in its `trigger` branch list
- Creating or updating Pull Requests that target branches in its `pr` branch list
- Scheduled runs
- Webhooks called
- Resource repository updates
- Resource external builds complete
- Resource pipelines complete
- New resource package versions are available
- Resource containers changes

Here's an example of when a pipeline information run is created. Suppose you have a repo in your local BitBucket Server and a pipeline that builds the code in that repo. Assume you scheduled your pipeline to run every day, at 03:00. Now, imagine it's 03:00 and your BitBucket Server is experiencing an outage. Azure DevOps reaches out to your local BitBucket Server to fetch the pipeline's YAML code, but it can't, because of the outage. At this moment, the system creates a pipeline information run, similar to the one shown in the previous screenshot.

# Next Steps

Learn more about [Triggers](../build/triggers.md) and building your [GitHub](../repos/github.md) or [BitBucket](../repos/bitbucket.md) repositories.
