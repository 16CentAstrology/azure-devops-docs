---
title: Connect to Microsoft Azure with an ARM service connection
ms.custom: seodec18, devx-track-arm-template
description: Use an ARM service connection to connect Azure Pipelines or TFS to Microsoft Azure
ms.assetid: 4CC6002E-9EF6-448C-AD48-5C618C103950
ms.topic: conceptual
ms.author: ronai
author: RoopeshNair
ms.date: 08/15/2023
monikerRange: '<= azure-devops'
---

# Connect to Microsoft Azure with an ARM service connection

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

[!INCLUDE [temp](../includes/concept-rename-note.md)]

You can use an Azure Resource Manager (ARM) service connection to connect to Azure resources with Service Principal Authentication (SPA) or an Azure-Managed Service Identity.  With an Azure Resource Manager service connection, you can use a pipeline to deploy to Azure resources like an App Service app without needing to reauthenticate each time. 

There are multiple options for connecting to Azure with Azure Resource Manager service connections:

::: moniker range="azure-devops"
* service principal or managed identity with workload identity federation
::: moniker-end
* service principal with secret	
* agent-assigned managed identity	 

> For other types of connection, and general information about creating and using connections, see [Service connections for builds and releases](service-endpoints.md).

::: moniker range="azure-devops"

## Create an Azure Resource Manager service connection using workload identity federation 

[!INCLUDE [workload-identity-preview](../release/includes/workload-identity-preview.md)]

[Workload identity federation](/azure/active-directory/workload-identities/workload-identity-federation) uses OpenID Connect to authenticate with Microsoft Entra protected resources without needing to manage secrets. 

We recommend this approach if:

* You have the Owner role for your Azure subscription.
* You're not connecting to [Azure Stack](#connect-stack) or an [Azure Government Cloud](#connect-govt).
* You're not connecting from Azure DevOps Server 2019 or earlier versions of TFS.
* Any Marketplace extensions tasks used have been updated to support workload identity federation. 

### Create a new workload identity federation service connection

1. In Azure DevOps, open the **Service connections** page from the [project settings page](../../project/navigation/go-to-service-page.md#open-project-settings).

1. Choose **+ New service connection** and select **Azure Resource Manager**.

   ![Screenshot of choosing a workload identity service connection type.](media/new-service-connection-azure-resource-manager.png)

1. Select **Workload identity federation (automatic)**. 

   ![Screenshot of selecting a workload identity service connection type.](media/select-workload-identity-service.png)

1. Specify the following parameters.

   | Parameter | Description |
   | --------- | ----------- |
   | Subscription | Select an existing Azure subscription. If you don't see any Azure subscriptions or instances, see [Troubleshoot Azure Resource Manager service connections](../release/azure-rm-endpoint.md). |
   | Resource Group | Leave empty to allow users to access all resources defined within the subscription, or select a resource group to which you want to restrict users' access (users will be able to access only the resources defined within that group). |
   | Service connection name | Required. The name you will use to refer to this service connection in task properties. This is not the name of your Azure subscription. |


1. After the new service connection is created, copy the connection name into your code as the `azureSubscription` value.

1. To deploy to a specific Azure resource, the task will need additional data about that resource. Go to the resource in the Azure portal, and then copy the data into your code. For example, to deploy a web app, you would copy the name of the App Service into the `WebAppName` value.

### Convert an existing ARM service connection to use workload identity federation

You can quickly convert an existing ARM service connection to use workload identity federation instead as long as the service connection meets these requirements. 

- Azure DevOps originally created the service connection. If you created your service connection manually, you cannot convert the service connection using the tool because Azure DevOps does not have permission to modify its credentials.
- Only one project uses the service connection. You can't convert [cross-project service connections](service-endpoints.md#project-permissions---cross-project-sharing-of-service-connections). 

1. In Azure DevOps, open the **Service connections** page from the [project settings page](../../project/navigation/go-to-service-page.md#open-project-settings).

1. Go to **Pipelines** > **Service connections** and open an existing service connection. 

1. Select the service connection you want to convert to use workload identity. 

1. Select **Convert**. 

    :::image type="content" source="media/federated-convert-credential.png" alt-text="Screenshot of selecting convert for federated credential.":::

1. Select **Convert** again to confirm that you want to create a new service connection. You will have seven days to revert the connection. The conversion may take a few minutes to process. Once the process completes, you'll be able to use the new service connection. 

### Revert an existing ARM service connection to use a service principal secret

You can revert a converted service connection with its secret for seven days. After seven days, you'll need to manually create a new secret.

To revert a connection:

1. In Azure DevOps, open the **Service connections** page from the [project settings page](../../project/navigation/go-to-service-page.md#open-project-settings).

1. Go to **Pipelines** > **Service connections** and open an existing service connection. 

1. Select the service connection you want to revert. 

1. Select **Revert conversion to the original scheme**. 

    :::image type="content" source="media/federated-revert-credential.png" alt-text="Screenshot of selecting revert for federated credential.":::

1. Select **Revert** again to confirm your choice. 

::: moniker-end


::: moniker range=">=azure-devops-2020"

## Create an Azure Resource Manager service connection using a service principal secret

We recommend this simple approach if:

* You're signed in as the owner of the Azure Pipelines organization and the Azure subscription.
* You don't need to further limit the permissions for Azure resources accessed through the service connection.
* You're not connecting to [Azure Stack](#connect-stack) or an [Azure Government Cloud](#connect-govt).
* You're not connecting from Azure DevOps Server 2019 or earlier versions of TFS

1. In Azure DevOps, open the **Service connections** page from the [project settings page](../../project/navigation/go-to-service-page.md#open-project-settings).
   In TFS, open the **Services** page from the "settings" icon in the top menu bar.

1. Choose **+ New service connection** and select **Azure Resource Manager**.

   ![Choosing a service connection type](media/new-service-endpoint-2.png)

1. Specify the following parameters.

   | Parameter | Description |
   | --------- | ----------- |
   | Connection Name | Required. The name you will use to refer to this service connection in task properties. This is not the name of your Azure subscription. |
   | Scope level | Select Subscription or Management Group. [Management groups](/azure/azure-resource-manager/management-groups-overview) are containers that help you manage access, policy, and compliance across multiple subscriptions. |
   | Subscription | If you selected Subscription for the scope, select an existing Azure subscription. If you don't see any Azure subscriptions or instances, see [Troubleshoot Azure Resource Manager service connections](../release/azure-rm-endpoint.md). |
   | Management Group | If you selected Management Group for the scope, select an existing Azure management group. See [Create management groups](/azure/azure-resource-manager/management-groups-create). |
   | Resource Group | Leave empty to allow users to access all resources defined within the subscription, or select a resource group to which you want to restrict users' access (users will be able to access only the resources defined within that group). |

1. After the new service connection is created:

   * If you're using the classic editor, select the connection name you assigned in the **Azure subscription** setting of your pipeline.
   * If you're using YAML, copy the connection name into your code as the `azureSubscription` value.

1. To deploy to a specific Azure resource, the task will need additional data about that resource.

   * If you're using the classic editor, select data you need. For example, the App Service name.
   * If you're using YAML, then go to the resource in the Azure portal, and then copy the data into your code. For example, to deploy a web app, you would copy the name of the App Service into the `WebAppName` value.

> [!NOTE]
> 
> When you follow this approach, Azure DevOps *connects with Microsoft Entra ID and creates an app registration with a secret that's valid for two years*. When the service connection is close to two years old, Microsoft Entra ID displays this prompt: **A certificate or secret is expiring soon. Create a new one**. In this scenario, you must refresh the service connection.
>
> To refresh a service connection, in the Azure DevOps portal, edit the connection and select **Verify**. After you save the edit, the service connection is valid for another two years.
> 

See also: [Troubleshoot Azure Resource Manager service connection](../release/azure-rm-endpoint.md).

If you have problems using this approach (such as no subscriptions being shown in the drop-down list),
or if you want to further limit users' permissions, you can instead use a [service principal](#use-spn)
or a [VM with a managed service identity](#use-msi).  

::: moniker-end


<a name="use-spn"></a>

## Create an Azure Resource Manager service connection with an existing service principal

1. If you want to use a predefined set of access permissions, and you don't already have a suitable service principal defined, follow one of these tutorials to create a new service principal:

   * [Use the portal to create a Microsoft Entra application and a service principal that can access resources](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
   * [Use Azure PowerShell to create an Azure service principal with a certificate](/azure/active-directory/develop/howto-create-service-principal-portal#option-1-upload-a-certificate)   

1. In Azure DevOps, open the **Service connections** page from the [project settings page](../../project/navigation/go-to-service-page.md#open-project-settings).
   In TFS, open the **Services** page from the "settings" icon in the top menu bar.

1. Choose **+ New service connection** and select **Azure Resource Manager**.

   ![Choosing a service connection type](media/new-service-endpoint-2.png)

1. Choose Service Principal (manual) option and enter the Service Principal details.

   ![Opening the full version of the service  dialog](media/rm-endpoint-link.png)

1. Enter a user-friendly **Connection name** to use when referring to this service connection.

1. Select the **Environment** name (such as Azure Cloud, Azure Stack, or an Azure Government Cloud).

1. If you _do not_ select **Azure Cloud**, enter the Environment URL. For Azure Stack, this will be something like `https://management.local.azurestack.external`

1. Select the **Scope level** you require: 
   
   * If you choose **Subscription**, select an existing Azure subscription. If you don't see any Azure subscriptions or instances, see [Troubleshoot Azure Resource Manager service connections](../release/azure-rm-endpoint.md). |
   * If you choose **Management Group**, select an existing Azure management group. See [Create management groups](/azure/azure-resource-manager/management-groups-create). |

1. Enter the information about your service principal into the Azure subscription dialog textboxes:

   * Subscription ID
   * Subscription name
   * Service principal ID
   * Either the service principal client key or, if you have selected **Certificate**, enter the contents of both the certificate and private key sections of the *.pem file.
   * Tenant ID<p/>

   You can obtain this information if you don't have it to hand by downloading and running
   [this PowerShell script](https://github.com/Microsoft/vsts-rm-extensions/blob/master/TaskModules/powershell/Azure/SPNCreation.ps1) in an Azure PowerShell window.
   When prompted, enter your subscription name, password, role (optional), and the type of cloud such as Azure Cloud (the default), Azure Stack, or an Azure Government Cloud.

1. Choose **Verify connection** to validate the settings you entered.

1. After the new service connection is created:

   * If you are using it in the UI, select the connection name you assigned in the **Azure subscription** setting of your pipeline.
   * If you are using it in YAML, copy the connection name into your code as the **azureSubscription** value.

1. If required, modify the service principal to expose the appropriate permissions. For more details, see 
   [Use Role-Based Access Control to manage access to your Azure subscription resources](/azure/role-based-access-control/role-assignments-portal).
   [This blog post](https://devblogs.microsoft.com/devops/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)
   also contains more information about using service principal authentication.

See also: [Troubleshoot Azure Resource Manager service connections](../release/azure-rm-endpoint.md).

<a name="use-msi"></a>

## Create an Azure Resource Manager service connection to a VM with a managed service identity

> [!NOTE]
> 
> You are required to use a self-hosted agent on an Azure VM in order to use managed service identity 

You can configure Azure Virtual Machines (VM)-based agents with an
[Azure Managed Service Identity](/azure/active-directory/managed-service-identity/overview)
in Microsoft Entra ID. This lets you use the system assigned identity (Service Principal)
 to grant the Azure VM-based agents access to any Azure resource that supports Microsoft Entra ID,
such as Key Vault, instead of persisting credentials in Azure DevOps for the connection.



1. In Azure DevOps, open the **Service connections** page from the [project settings page](../../project/navigation/go-to-service-page.md#open-project-settings).
   In TFS, open the **Services** page from the "settings" icon in the top menu bar.

1. Choose **+ New service connection** and select **Azure Resource Manager**.

   ![Choosing a service connection type](media/new-service-endpoint-2.png)

1. Select the **Managed Identity Authentication** option.

   ![Opening the managed service identity settings](media/rm-endpoint-msi.png)

1. Enter a user-friendly **Connection name** to use when referring to this service connection.

1. Select the **Environment** name (such as Azure Cloud, Azure Stack, or an Azure Government Cloud).


1. Enter the values for your subscription into these fields of the connection dialog:

   * Subscription ID
   * Subscription name
   * Tenant ID<p/>

1. After the new service connection is created:

   * If you are using it in the UI, select the connection name you assigned in the **Azure subscription** setting of your pipeline.
   * If you are using it in YAML, copy the connection name into your code as the **azureSubscription** value.

1. Ensure that the VM (agent) has the appropriate permissions.
   For example, if your code needs to call Azure Resource Manager, assign the VM the appropriate role using Role-Based Access Control (RBAC) in Microsoft Entra ID.
   For more details, see [How can I use managed identities for Azure resources?](/azure/active-directory/managed-identities-azure-resources/overview#how-can-i-use-managed-identities-for-azure-resources) and
   [Use Role-Based Access Control to manage access to your Azure subscription resources](/azure/role-based-access-control/role-assignments-portal).

See also: [Troubleshoot Azure Resource Manager service connections](../release/azure-rm-endpoint.md).

<a name="connect-govt"></a>

## Connect to an Azure Government Cloud

For information about connecting to an Azure Government Cloud, see:

* [Connecting from Azure Pipelines (Azure Government Cloud)](/azure/azure-government/documentation-government-get-started-connect-with-vsts)

<a name="connect-stack"></a>

## Connect to Azure Stack

For information about connecting to Azure Stack, see:

* [Connect to Azure Stack](/azure/azure-stack/azure-stack-connect-azure-stack)
* [Connect Azure Stack to Azure using VPN](/azure/azure-stack/azure-stack-connect-vpn)
* [Connect Azure Stack to Azure using ExpressRoute](/azure/azure-stack/azure-stack-connect-expressroute)

[!INCLUDE [rm-help-support-shared](../includes/rm-help-support-shared.md)]
