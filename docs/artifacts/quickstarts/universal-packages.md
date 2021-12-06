---
title: Publish and download universal packages with Azure Artifacts
description: How to publish and download universal packages to and from Artifacts feeds.
ms.assetid: f47b858c-138d-426d-894c-a5fe1d5aa08e
ms.technology: devops-artifacts
ms.topic: conceptual
ms.date: 12/03/2021
monikerRange: '>= tfs-2017'
---

# Publish and download universal packages with Azure CLI

With universal packages, you can store different types of packages other than the widely used ones such as NuGet, npm, Maven, or Python packages. Using Azure CLI, you can publish and download universal packages from the command line. Uploaded packages can vary in size (up to 4 TB), but should always have a name and a version number. This article will walk you through the steps to publish and download your universal packages to Azure Artifacts feeds.

## Prerequisites

1. [Install Azure CLI](/cli/azure/install-azure-cli).
1. If you're using Linux, make sure you install the [.NET on Linux](/dotnet/core/linux-prerequisites) version.
1. [Azure DevOps Services account](https://azure.microsoft.com/services/devops/).
1. [Azure Artifacts feed](../get-started-nuget.md).

## Connection setup

1. To check the version of Azure CLI modules and extensions that you currently have, run the following command: 

   ```Command
   az --version
   ```

2. Install the Azure DevOps extension.

   ```Command
   az extension add --name azure-devops
   ```

3. If you already have the Azure DevOps extension but you want to update to the latest version, run the following command:

   ```Command
   az extension update --name azure-devops
   ```

4. Log in to Azure.

    ```Command
    az login
    ```

> [!TIP]
> To access tenants without subscriptions, run `az login --allow-no-subscription`.

5. Set your project and organization as the CLI's default.

    ```Command
    az devops configure --defaults project=<YOUR_PROJECT_NAME> organization=https://dev.azure.com/<YOUR_ORGANIZATION_NAME> 
    ```

## Publish universal packages

To publish a universal package, run the following command in an elevated command prompt. Package names must be lowercase and can only use letters, numbers, and dashes. Package versions must be lowercase without build metadata (+ suffix). See [SemVer](https://semver.org/spec/v2.0.0.html) to learn more about semantic versioning.

The following command will publish a universal package to an organization-scoped feed:

```Command
az artifacts universal publish --organization https://dev.azure.com/<YOUR_ORGANIZATION> --feed <FEED_NAME> --name <PACKAGE_NAME> --version <PACKAGE_VERSION> --description <PACKAGE_DESCRIPTION> --path <PACKAGE_DIRECTORY>
```

## View published packages

1. Navigate to your Azure DevOps organization.

1. Select your project, and then select **Artifacts**.

1. Select your feed from the drop-down menu. 

    :::image type="content" source="media/universal-in-feed.png" alt-text="Screenshot showing the newly published package.":::

## Download universal packages

To download a universal package using Azure CLI, run the following command in an elevated command prompt.

```Command
az artifacts universal download --organization https://dev.azure.com/<YOUR_ORGANIZATION> --feed <FEED_NAME>  --name <PACKAGE_NAME> --version <PACKAGE_VERSION> --path <DOWNLOAD_PATH>
```

## Download specific files

If you only want to download specific files, you can use the `--file-filter` parameter to download a subset of files.

Example: `--file-filter *logs/*.log` would match any file ending with *logs* and with the extension *.log* (Example: build123_logs.log). See [File matching patterns reference](../../pipelines/tasks/file-matching-patterns.md) for more details.

```Command
az artifacts universal download --organization https://dev.azure.com/<YOUR_ORGANIZATION> --feed <FEED_NAME> --name <PACKAGE_NAME> --version <PACKAGE_VERSION>  --path <DOWNLOAD_PATH>  --file-filter <MATCH_PATTERN>
```

## Download the latest version

You can use wildcards to download the latest version of your Universal Packages.

**Examples**:

- `--version *`: the latest version.

- `--version 1.*`: the latest version with major 1. 

- `--version 1.2.*`: the latest patch release with major 1 and minor 2.  
  
> [!NOTE]
> Wildcards are not supported in pre-release pipelines.

## Related articles

- [Publish and download universal packages in Azure Pipelines](../../pipelines/artifacts/universal-packages.md).
- [universal package task](../../pipelines/tasks/package/universal-packages.md).
- [Delete and recover packages](../how-to/delete-and-recover-packages.md).
- [Configure permissions](../feeds/feed-permissions.md)