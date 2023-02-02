---
title: Agent software version 3
description: Learn how to run pipelines using the version 3 agent software.
monikerRange: '= azure-devops'
ms.topic: conceptual
ms.date: 01/26/2023
---

# Agent software version 3 preview

The pipelines team is upgrading the agent software from version 2.x (using .NET Core 3.1) to version 3.x (using .NET 6). The new agent version supports new Apple silicon hardware and newer operating systems like Ubuntu 22.04, or Windows on ARM64.

## Upgrade to 3.x agent on supported operating systems

If you're running your self-hosted agents on newer operating systems [supported by .NET 6](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md), the upgrade to the new agent version is automatic when the 3.x agent is released to general availability. To try out the preview version of the 3.x agent, see [Install agent version 3.x preview](#install-agent-version-3x-preview).

## Upgrade to 3.x agent on unsupported operating systems

If you're running your self-hosted agents on an operating system supported by the current version 2.x agent software built using [.NET Core 3.1](https://github.com/dotnet/core/blob/main/release-notes/3.1/3.1-supported-os.md) that isn't supported by .NET 6, you must update your machines to use a newer supported operating system [supported by .NET 6](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md).

The following list of operating systems are commonly used for self-hosted 2.x agents. These operating systems aren't supported by .NET 6 and can't be used to run the new .NET 6 based version 3.x agent.

| System/Distribution | Version not supported by .NET 6 |
|---------------------|---------------------------------|
| CentOS | < 7 |
| Debian | <= 4.9 |
| Fedora | <= 32 |
| RedHat Enterprise Linux | <= 6 |
| Ubuntu | < 18.04 LTS |
| macOS | < 10.15 |

You can use a [script](https://github.com/microsoft/azure-pipelines-agent/tree/master/tools/FindAgentsNotCompatibleWithAgent) to predict whether the agents in your self-hosted pools will be able to upgrade from 2.x to 3.x.

## Install agent version 3.x preview

The version 3.x agent software is currently in the preview phase. You can install this software on [.NET 6 supported operating systems](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md). [Let us know your feedback](https://github.com/microsoft/azure-pipelines-agent/issues).

A preview of the new version 3.x agent software is available from the [Azure Pipelines releases page on GitHub](https://github.com/microsoft/azure-pipelines-agent/releases), in the pre-release section.

To use the new version 3 agent, install the latest .NET 6 agent from the pre-releases section of the [Azure Pipelines releases page on GitHub](https://github.com/microsoft/azure-pipelines-agent/releases) onto your agent machine, and register it with the desired [agent pool](pools-queues.md).

## FAQ

### What is the difference between the 2.x and 3.x agents?

The 2.x agents (e.g., 2.212) are .NET Core 3.1 and the 3.x agents (e.g., 3.212) are .NET 6. During Phase I and II, we'll have both versions simultaneously with the 3.x versions being in prerelease.

### How can I check my agents to see if they can upgrade to 3.x?

You can use a [script](https://github.com/microsoft/azure-pipelines-agent/tree/master/tools/FindAgentsNotCompatibleWithAgent) to predict whether the agents in your self-hosted pools will be able to upgrade from 2.x to 3.x.

### How will security issues in the agent be patched going forward?

When the .NET 6 agent becomes generally available for self-hosted pools in Q1 2023, there will be no patches done, in general, for the 2.x agents. The patches will be done only for the 3.x agents. However, we also have Azure DevOps Server customers that will still be relying on 2.x agents. So, we will review the security issues on a case by case basis to decide.

### What do I need to do when I’m on an unsupported OS?

You should migrate to a newer operating system that is supported by .NET 6 now. Otherwise, your agent may attempt to upgrade, and it will fail as .NET 6 cannot be installed on your OS. We will publish some guidance in a follow-up blog post that will prevent auto-upgrades of the agent. However, that is only meant to be a temporary solution to give you some more time to upgrade your agent machines.

### Can I stay on 2.x agents if I am not working on any changes in my project anymore?

No. The pipelines team is regularly adding new features to Azure Pipelines and some of them may require an update to the agent even though your pipeline does not explicitly depend on that feature. When you prevent auto-upgrades of the agent using the guidance in a follow-up blog, that agent cannot be used to schedule the pipeline. If no agent with the required capabilities can be found, the pipeline execution will fail.

### I use Azure DevOps Server and not Azure DevOps Service. Does this change impact me?

No. The new agent is only applicable for Azure DevOps Service customers at this time. However, a future version of Azure DevOps Server will include the new agent. The pipelines team recommends that you update your agent machines to newer operating systems that are supported by .NET 6 starting now, if you plan to keep up with the Azure DevOps Server releases in the future.

### What is the timeline for agent version 3 deployment?

Agent version 3 is being rolled out gradually. Agent version 2 will be retired at the end of Q1 2023. See [Upgrade of .NET agent for Azure Pipelines](https://devblogs.microsoft.com/devops/upgrade-of-net-agent-for-azure-pipelines/).

### What will happen when a task requires an agent to be updated to agent version 3?
Normally, when a task requires a newer version of the agent, it will automatically update itself. For now, while agent version 2 continues to be updated, we have disabled auto update from agent version 2 to agent version 3. Once we enable it, for Operating Systems that are not compatible with agent version 3, agent version 2.217 and newer will not attempt to update itself to the v3 agent. Instead, a warning will be shown informing users they need to upgrade the Operating System first: `The operating system the agent is running on is <OS>, which will not be supported by the .NET 6 based v3 agent. Please upgrade the operating system of this host to ensure compatibility with the v3 agent.` See [https://aka.ms/azdo-pipeline-agent-version](https://aka.ms/azdo-pipeline-agent-version)'
