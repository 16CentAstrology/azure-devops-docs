---
title: Register an agent using a service principal
description: Learn how to register a self-hosted agent using a Service Principal
ms.topic: conceptual
ms.date: 10/09/2023
monikerRange: 'azure-devops'
---

# Register an agent using a service principal

You can register an agent using a Service Principal starting with [agent version 3.227.1](https://github.com/microsoft/azure-pipelines-agent/releases/tag/v3.227.1) by specifying **SP** as the agent authentication option.

## Grant the Service Principal access to the agent pool

Before registering an agent using a Service Principal you must have [created a Service Principal](../../integrate/get-started/authentication/service-principal-managed-identity.md) and granted it permission to access the agent pool.

1. Open a browser and navigate to the **Agent pools** tab for your Azure Pipelines organization.

   [!INCLUDE [agent-pools-tab](./includes/agent-pools-tab/agent-pools-tab.md)]

1. Select the desired agent pool on the right side of the page and then choose **Security**. Choose Add, and add the Service Principal with the **Administrator** role.

   :::image type="content" source="./media/agent-registration/agent-pool-security.png" alt-text="Screenshot of agent pool security tab.":::

1. If the Service Principal you're going to use is not shown, then get an administrator to add it, granting the Service Principal the administrator role for the agent pool. The administrator can be an agent pool administrator, an [Azure DevOps organization owner](../../organizations/accounts/faq-user-and-permissions-management.yml#find-owner), or a [TFS or Azure DevOps Server administrator](/azure/devops/server/admin/add-administrator).

   If it's a [deployment group](../release/deployment-groups/index.md) agent, the administrator can be a deployment group administrator, an [Azure DevOps organization owner](../../organizations/accounts/faq-user-and-permissions-management.yml#find-owner), or a [TFS or Azure DevOps Server administrator](/azure/devops/server/admin/add-administrator).

   You can add a Service Principal to the deployment group administrator role in the **Security** tab on the **Deployment Groups** page in **Azure Pipelines**.

> [!NOTE]
> If you see a message like this: **Sorry, we couldn't add the identity. Please try a different identity.** or **Cannot modify the role for self identity. Please try with a different identity.**, you probably followed the above steps for an organization owner or TFS or Azure DevOps Server administrator. You don't need to do anything; you already have permission to administer the agent pool.
>
> If you are adding the Service Principal to the agent pool security group using **Project Settings**, **Agent pools**, you must first add the Service Principal as an organization user with **Basic** **Access level** (recommended) or higher.

## Register the agent using a Service Principal

1. Specify **SP** when prompted for authentication type during agent configuration to use a Service Principal to authenticate during agent registration.

1. When prompted, supply the **Client(App) ID** and the **Tenant ID**.

   :::image type="content" source="./media/agent-registration/service-principal-ids.png" alt-text="Screenshot of application IDs.":::

1. Specify the client secret. The client secret is used only during agent registration.

   :::image type="content" source="./media/agent-registration/service-principal-client-secret.png" alt-text="Screenshot of client secret.":::

1. Specify the name of the agent pool for which you granted administrator permission for the Service Principal, and continue the agent registration steps.


