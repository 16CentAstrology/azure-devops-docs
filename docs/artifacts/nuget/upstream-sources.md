---
title: Use packages from nuget.org
description: How to consume packages from nuget.org with Azure Artifacts upstream sources
ms.assetid: 301f954f-a35a-4fe2-b7fd-c78e534d9b16
ms.service: azure-devops-artifacts
ms.topic: conceptual
ms.date: 11/27/2023
monikerRange: '<= azure-devops'
"recommendations": "true"
---

# Use packages from NuGet Gallery

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

With Azure Artifacts upstream sources, developers are able to consume packages from public registries such as nuget.org and npmjs.com. This article will walk you through the process of setting up your project and using the command line to effectively consume NuGet packages from the NuGet Gallery. In this article, you'll learn how to:

> [!div class="checklist"]    
> * Enable upstream sources for your feed 
> * Add NuGet Gallery as an upstream source 
> * Connect to your feed
> * Install packages from nuget.org

## Prerequisites

- An Azure DevOps organization and a project. Create an [organization](../../organizations/accounts/create-organization.md) or a [project](../../organizations/projects/create-project.md#create-a-project) if you haven't already.

- An Azure Artifacts feed.

- Download [NuGet](https://www.nuget.org/downloads).

- Download and install [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider#azure-artifacts-credential-provider).

## Enable upstream sources on your feed

If you don't have a feed, follow these steps to create a new one and make sure to check the upstream sources checkbox to enable them. If you already have a feed, you can jump to the [next step](#add-nuget-gallery-upstream-source) to add the NuGet Gallery as an upstream source.

[!INCLUDE [](../includes/create-feed.md)]

## Add NuGet Gallery upstream source

If you've checked the upstream sources checkbox when making your feed, NuGet Gallery should have been added automatically. If not, add it manually by following these steps:

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select the gear icon button ![gear icon](../../media/icons/gear-icon.png) to navigate to your **Feed settings**.

1. Select **Upstream Sources**, and then select **Add Upstream** to add a new upstream source.

1. Select **Public source**, and then select **NuGet Gallery** from the dropdown menu.

1. Select **Save** when you're done, and then select **Save** one more time at the top right corner to save your changes.
    
> [!NOTE]
> The service index location for nuget.org is `https://api.nuget.org/v3/index.json`.

## Connect to feed

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select **Connect to feed**, and then select **NuGet.exe**.

1. Add a *nuget.config* file in the same folder as your *.csproj* or *.sln* file. Paste the provided XML snippet into your file. If you use the examples below, make sure you replace the placeholders with the appropriate values for your scenario.

    - **Organization-scoped feed**:
    
        ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <packageSources>
            <clear />
            <add key="<SOURCE_NAME>" value="https://pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/nuget/v3/index.json" />
          </packageSources>
        </configuration>
        ```
    
    - **Project-scoped feed**:
    
        ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <packageSources>
            <clear />
            <add key="<SOURCE_NAME>" value="https://pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/nuget/v3/index.json" />
          </packageSources>
        </configuration>
        ```

## View saved packages

You can view the packages you saved from the NuGet Gallery by selecting your **Source** from the dropdown menu.

::: moniker range=">= azure-devops-2019"

:::image type="content" source="./media/view-saved-packages-nuget.png" alt-text="A screenshot showing how to filter packages by source.":::

::: moniker-end

::: moniker range="tfs-2018"

:::image type="content" source="media/view-cached-packages.png" alt-text="A screenshot showing how to filter packages by source in TFS":::

::: moniker-end

## Related articles

- [Publish NuGet packages with Azure Pipelines](../../pipelines/artifacts/nuget.md)
- [Publish packages to NuGet.org](./publish-to-nuget-org.md)
- [Upstream sources overview](../concepts/upstream-sources.md)