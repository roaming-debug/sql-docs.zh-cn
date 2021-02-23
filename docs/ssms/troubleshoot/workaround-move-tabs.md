---
description: 移动选项卡的解决方法
title: 可在不让 SSMS 崩溃的情况下移动选项卡
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: ae7c79792d962ce578059e672de7165f477f1c4a
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654626"
---
# <a name="workaround-to-move-tabs"></a>移动选项卡的解决方法

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

可能需要一种解决方法来移动查询编辑器选项卡，无论是在窗口中还是停靠先前删除的选项卡。如果已应用所有可用的 Windows 更新，但发现操作查询编辑器选项卡时仍遇到崩溃，请按照以下[解决方法](#workaround)操作。

## <a name="applicable-environments"></a>适用的环境
Windows 针对 .NET Framework 的更新引入了一个已知问题，在停靠选项卡或拆分窗口时，该问题会导致 SQL Server Management Studio (SSMS) 应用程序崩溃。  你可在 [SQL Server 用户反馈](https://feedback.azure.com/forums/908035/suggestions/42651556)中找到最新信息。

## <a name="workaround"></a>解决方法

如果在应用所有可用的 Windows 更新后仍然遇到崩溃，请按照以下步骤来解决此问题：

1. 关闭所有 SQL Server Management Studio (SSMS) 实例。

2. 找到你的 SSMS 应用程序文件 (exe)。  该文件通常位于 `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE`。

3. 以管理员身份在记事本中打开文件 `Ssms.exe.config`。

4. 找到 `AppContextSwitchOverrides` 节点，然后将这两个属性追加到值。
    ```
    ;Switch.System.Windows.Interop.MouseInput.OptOutOfMoveToChromedWindowFix=true; Switch.System.Windows.Interop.MouseInput.DoNotOptOutOfMoveToChromedWindowFix=true
    ```

    ![编辑 ssms.exe.config](../media/troubleshoot/execonfig-edit.png)

5. 保存配置文件，然后重新打开 SSMS。
