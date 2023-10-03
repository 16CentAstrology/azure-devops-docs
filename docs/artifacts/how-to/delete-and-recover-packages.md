---
title: Delete and recover packages
description: How to manually delete packages, set up retention policies, and recover deleted packages from the Recycle Bin.
ms.service: azure-devops-artifacts
ms.assetid: 10f5e81f-2518-41b9-92b6-e00c905b59b3
ms.custom: contperf-fy21q2, contperf-fy22q1
ms.topic: conceptual
ms.date: 09/05/2023
monikerRange: '<= azure-devops'
"recommendations": "true"
---

# Delete and recover packages

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

Azure Artifacts securely stores various package types within your feed, whether you've published them directly or saved them from upstream sources. As older package versions become less relevant, you may consider removing them through manual deletion or by using retention policies. In this article, you'll learn how to:

> [!div class="checklist"]  
> * Delete packages from your feed.
> * Set up retention policies.
> * Manually delete packages permanently.
> * Recover recently deleted packages.

> [!NOTE]
> To delete packages or set up retention policies, you must be a feed **Owner** or **Administrator**.

## Delete packages

In Azure Artifacts, packages are immutable. Once you publish a package to your feed, its version number is reserved permanently. Even if you delete it from your feed, you cannot publish a new package with the same version number.

#### [NuGet](#tab/nuget/)

> [!NOTE]
> You must be a **Contributor** to unlist a package and an **Owner** to delete it.

Two options are available to delete a NuGet package from your feed, [Unlist](#qa) and [Delete](#qa).

::: moniker range=">= azure-devops-2019"

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select the package that you want to delete or deprecate, and then select **Unlist** or **Delete**.

    :::image type="content" source="../media/delete/unlist-delete-nuget-package-newnav.png" alt-text="Screenshot that shows buttons for unlisting and deleting NuGet packages.":::

::: moniker-end

::: moniker range="tfs-2018"

1. Navigate to your project `http://ServerName:8080/tfs/DefaultCollection/<ProjectName>`.

1. Select **Build and Release**.

1. Select **Packages**, and then select the package that you want to delete. 

1. Select **Unlist** or **Delete latest**.

    :::image type="content" source="../media/delete/unlist-delete-nuget-package.png" alt-text="Screenshot that shows the buttons for unlisting and deleting NuGet packages in Team Foundation Server.":::

::: moniker-end

### Unlist a NuGet package by using NuGet.exe

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed from the dropdown menu. Select **Connect to feed**.

   ::: moniker range=">= azure-devops-2019"

     :::image type="content" source="../media/connect-to-feed-azure-devops-newnav.png" alt-text="Screenshot that shows the button for connecting to a feed.":::

   ::: moniker-end

   ::: moniker range="tfs-2018"

    :::image type="content" source="../media/connect-to-feed.png" alt-text="Screenshot that shows the button for connecting to a feed in Team Foundation Server.":::

   ::: moniker-end

2. Select **NuGet.exe**, and then find and copy your **Package Source** URL.

3. Run the following command:

    ```Command
    nuget.exe delete <PACKAGE_NAME> <PACKAGE_VERSION> -Source <PACKAGE_SOURCE_URL> -ApiKey <KEY>
    ```

> [!NOTE]
> Azure DevOps and Visual Studio Team Foundation Server interpret the `nuget.exe delete` command as an unlist operation. To delete a package, you must use the [REST API](/rest/api/azure/devops/artifactspackagetypes/nuget/delete-package-version) or the web interface.

#### [npm](#tab/npm/)

> [!NOTE]
> You must be a **Contributor** to deprecate a package and an **Owner** to unpublish it.

There are two options to delete an npm package from your feed, [Deprecate](#qa) and [Unpublish](#qa).

::: moniker range=">= azure-devops-2019"

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select the package that you want to delete or deprecate, and then select **Deprecate** or **Unpublish latest**.

    :::image type="content" source="../media/delete/deprecate-unpublish-npm-package-newnav.png" alt-text="Screenshot that shows the buttons for deprecating and unpublishing.":::

::: moniker-end

::: moniker range="tfs-2018"

1. Navigate to your project `http://ServerName:8080/tfs/DefaultCollection/<ProjectName>`.

1. Select **Build and Release**.

1. Select **Packages**, and then select the package that you want to delete. 

1. Select **Deprecate** or **Unpublish latest**.

    :::image type="content" source="../media/delete/deprecate-unpublish-npm-package.png" alt-text="Screenshot that shows the buttons for deprecating and unpublishing in Team Foundation Server.":::

::: moniker-end

#### Deprecate or unpublish an npm package by using the CLI

1. [Set up your client's .npmrc file](../npm/npmrc.md).

1. Use the following command to deprecate an npm package:

    ```Command
    npm deprecate <package>[@<version>] <message>
    ```

   Use the following command to unpublish an npm package:

    ```Command
    npm unpublish <package>@<version>
    ```

> [!NOTE]
> The `npm unpublish` command won't unpublish all versions of the package. For more information, see the [deprecate](https://docs.npmjs.com/cli/deprecate) or [unpublish](https://docs.npmjs.com/cli/unpublish) documentation.

#### [Python](#tab/python/)

> [!NOTE]
> You must be a feed **Owner** to delete a Python package.

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select the package that you want to delete, and then select **Delete latest**.

:::image type="content" source="../media/delete/delete-python-package.png" alt-text="Screenshot that shows the button for deleting a package in Python.":::

#### [Maven](#tab/maven/)

::: moniker range=">= azure-devops-2019"

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select the package that you want to delete, and then select **Delete latest**. Select **Delete** to confirm your choice. 

::: moniker-end

::: moniker range="tfs-2018"

1. Navigate to your project `http://ServerName:8080/tfs/DefaultCollection/<ProjectName>`.

1. Select **Build and Release**, and then select **Packages**.

1. Select your feed, and then select the package that you want to delete.

1. Select **Delete latest** to delete the latest version of your package.

    :::image type="content" source="../media/delete/delete-maven-package.png" alt-text="Screenshot that shows the button to delete packages from feeds.":::  

::: moniker-end

#### [Universal Package](#tab/universal/)

> [!NOTE]
> You must be a feed **Owner** to delete a Universal Package.

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select the package that you want to delete, and then select **Delete latest**.

:::image type="content" source="../media/delete/delete-universal-package.png" alt-text="Screenshot that shows the button for deleting a Universal Package.":::

* * *

## Delete a package permanently 

Packages placed in the **Recycle Bin** get deleted permanently after 30 days. However, these packages still count as part of your storage bill. If you want to delete them sooner, you can manually delete them from the Recycle Bin following these steps:

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select your feed.

1. Select **Recycle Bin** located in the upper-right corner.

    :::image type="content" source="../media/delete/recycle-bin.png" alt-text="A screenshot showing how to access the recycle bin in Azure Artifacts.":::

1. Select the package you wish to delete permanently, and then select **Permanently Delete**.

    :::image type="content" source="../media/delete/delete-package-permanently.png" alt-text="A screenshot showing how to permanently delete a package in Azure Artifacts.":::

1. Select **Permanently Delete** one more time to confirm your choice. Your package will be deleted permanently.

     :::image type="content" source="../media/delete/delete-package-permanently-confirmation.png" alt-text="A screenshot showing a confirmation message before you delete a package permanently.":::

## Delete packages automatically with retention policies

The number of versions for each package hosted in your feed can grow quickly. To free up storage space, you can set up retention policies to automatically delete old packages.

If you want to retain a package indefinitely, you can promote it to a [view](../concepts/views.md). Packages promoted to a view are exempt from retention policies and won't be deleted.

> [!NOTE]
> Package demotion is not supported. If you want this feature to be added to future releases, feel free to use **Suggest a feature** on our [Azure DevOps Developer Community](https://developercommunity.visualstudio.com/spaces/21/index.html) page.

Follow the steps below to set up retention policies for your feed:

::: moniker range=">= azure-devops-2019"

1. Select **Artifacts**.

    :::image type="content" source="../media/goto-feed-hub-azure-devops-newnav.png" alt-text="Screenshot that shows the Artifacts button.":::

1. Select the gear icon ![gear icon](../../media/icons/gear-icon.png) to navigate to your feed's settings.

    :::image type="content" source="../media/feed-settings.png" alt-text="A screenshot showing how to navigate to feed settings.":::

1. Select **Feed details**, and then select the **Enable package retention** checkbox. Specify a value for both the **Maximum number of versions per package** and **Days to keep recently downloaded packages**.

    :::image type="content" source="../media/retention-policy-settings.png" alt-text="Screenshot that shows how to enable retention policies for your feed.":::
    
1. Select **Save** when you're done.

- **Maximum number of versions per package**: The number of versions for each package you wish to retain.

- **Days to keep recently downloaded packages**: Packages will only get deleted if they haven't been downloaded for the specified number of days indicated here.

::: moniker-end

::: moniker range="tfs-2018"

1. Navigate to your project `http://ServerName:8080/tfs/DefaultCollection/<ProjectName>`.

1. Select **Build and Release**, and then select **Packages**.

1. Select the gear icon ![gear icon](../../media/icons/gear-icon.png) to access your feed's settings. 

    :::image type="content" source="../media/edit-feed-full.png" alt-text="Screenshot that shows how access the feed's settings in Team Foundation Server.":::

1. From the **Retention** tab, specify a value for both the **Maximum number of versions per package** and **Days to keep recently downloaded packages**.
    
    :::image type="content" source="../media/retention-policy-settings-tfs.png" alt-text="Screenshot that shows retention policies in Team Foundation Server.":::
   
1. Select **Save** when you're done.

- **Maximum number of versions per package**: The number of versions for each package you wish to retain.

- **Days to keep recently downloaded packages**: Packages will only get deleted if they haven't been downloaded for the specified number of days indicated here.

::: moniker-end

> [!NOTE]
> When you enable package retention, a version of a package will be deleted when *both* of the following criteria are met:
> - The number of published versions reaches the **Maximum number of versions per package** limit.
> - A version of that package has not been downloaded for the period defined in **Days to keep recently downloaded packages**.

## Recover deleted packages

Deleted packages remain in the Recycle Bin for 30 days. After that, they'll be permanently deleted. You must be a feed **Owner** to recover deleted packages.

::: moniker range=">= azure-devops-2019"

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select **Recycle Bin**.

    :::image type="content" source="../media/artifacts-recycle-bin.png" alt-text="A screenshot showing how to access the recycle bin.":::

1. Select your package, and then select **Restore**.

    :::image type="content" source="../media/restore-package.png" alt-text="A screenshot showing how to restore deleted packages.":::

::: moniker-end

::: moniker range="tfs-2018"

1. Navigate to your project `http://ServerName:8080/tfs/DefaultCollection/<ProjectName>`.

1. Select **Build and Release**, and then select **Packages**. 

1. Select **Recycle Bin**.

    :::image type="content" source="../media/recycle-bin/find-recycle-bin.png" alt-text="Screenshot of how to access the Recycle Bin in Team Foundation Server.":::

1. Select the appropriate package, and then select the package version that you want to delete.

    :::image type="content" source="../media/recycle-bin/recycle-bin-view.png" alt-text="Screenshot that shows the package in the Recycle Bin in Team Foundation Server.":::

1. Select **Restore to feed**.

    :::image type="content" source="../media/recycle-bin/recycle-bin-restore.png" alt-text="Screenshot that shows the button for restoring to feed in Team Foundation Server.":::

::: moniker-end

## Q&A

### Q: What is the difference between *Deprecate*, *Unpublish*, *Unlist*, and *Delete* a package version?

A: *Unpublish* and *Deprecate* applies to npm packages, while *Unlist* and *Delete* applies to NuGet packages. You can also *Delete* package versions for the rest of the package types (Maven, Python, and Universal Packages):

- **Deprecate** (npm): When you deprecate a package version, a warning message is added to the package's metadata. Azure Artifacts and most npm clients will display the warning message whenever the package is viewed or installed.
- **Unpublish** (npm): Unpublishing a package version makes it unavailable to install. Unpublished packages can be restored from the Recycle Bin within 30 days of deletion. After that, the packages will be permanently deleted.

- **Unlist** (NuGet): Unlisting a package version hides it from the search results in Azure Artifacts feeds and on NuGet.org.
- **Delete**: Deleting a package version makes it unavailable to install. Deleted packages can be restored from the Recycle Bin within 30 days of deletion. After that, the packages will be permanently deleted.

### Q: What happens with old or existing packages when we enable retention policies?

A: Old or existing packages will be soft-deleted and moved to the Recycle Bin. The deletion job runs once a day, but there might be an initial delay after the policy is turned on for the first time because of an influx of packages. 

Packages remain in the Recycle Bin for 30 days before they're permanently deleted. To remove the packages from your billable storage, you can choose to delete them manually by using the UI or the REST API before the 30 days are up. 

### Q: How long does it take for the billed storage amount to update after deleting Artifacts?

A: Typically, storage consumption should be updated within 24 hours, although in certain cases it might take up to 48 hours for the changes to reflect. The Artifacts usage on the billing page of your organization is updated once a day. However, The Artifact Storage page is updated more frequently, which may lead to a minor discrepancy between the information displayed on the two pages.

## Related articles

- [Artifacts storage consumption](../artifact-storage.md)
- [Promote a package to a view](../feeds/views.md)
- [Manage permissions](../feeds/feed-permissions.md)