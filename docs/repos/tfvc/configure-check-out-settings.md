---
title: Configure Check-Out Settings
titleSuffix: Azure Repos
description: Learn how to configure check-out settings for a TFVC repository.
ms.assetid: 9f4eb366-7e99-49f9-899d-cf3209c0ff72
ms.technology: devops-code-tfvc
ms.topic: conceptual
ms.date: 12/17/2021
monikerRange: '<= azure-devops'
---


# Configure check-out settings

[!INCLUDE [temp](../includes/version-tfs-2013-cloud.md)]
[!INCLUDE [temp](../includes/version-vs-2013-vs-2019.md)]

Administrators of Team Foundation version control (TFVC) can configure source control check-out settings. TFVC check-out settings enable files to be edited by more than one person at the same time. 

## Prerequisites

To configure check-out settings, you must have the **Edit project-level information** set to **Allow**. For more information, see [Default TFVC permissions](../../organizations/security/default-tfvc-permissions.md).

### Configure checkout settings

1.  In **Team Explorer**, select the project for which you want to configure check-out settings.

2.  From the **Team** menu, click **Team** **Project** **Settings**, and then click **Source** **Control**.

3.  In the **Source** **Control** **Settings** dialog box, select the **Check-out Settings** tab.

4.  Select or clear the **Enable multiple checkout** box.

5.  Select or clear the **Enable get latest on check-out** box.

6.  Click **OK**.

## Related articles

- [Configure check-out settings](configure-check-out-settings.md)