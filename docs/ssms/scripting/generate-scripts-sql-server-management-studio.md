---
title: 生成脚本
description: 了解如何使用“生成和发布脚本向导”为多个对象创建 Transact-SQL 脚本，以及如何使用对象资源管理器中的“编写脚本为”菜单为单个对象或多个对象生成脚本。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 9711c617-3c68-4e5a-aea3-befc64d51524
author: markingmyname
ms.author: maghan
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.date: 04/07/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 31907e51df24aaaee20b1fc27bbc97bf809abe03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271300"
---
# <a name="generate-scripts-sql-server-management-studio"></a>生成脚本 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了两种机制，用于生成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 可以使用“生成和发布脚本向导”为多个对象创建脚本  。 还可以通过使用 **“对象资源管理器”** 中的 **“编写脚本为”** 菜单为单个对象或多个对象生成脚本。

有关介绍如何使用 SQL Server Management Studio (SSMS) 编写各种对象的脚本的详细教程，请参阅[教程：在 SSMS 中编写脚本](../tutorials/scripting-ssms.md)。

## <a name="before-you-begin"></a>开始之前

选择最能满足您的需求的机制。 

###  <a name="generate-and-publish-scripts-wizard"></a><a name="GenPubScriptWiz"></a> 生成和发布脚本向导

使用 **“生成和发布脚本向导”** 可为多个对象创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 此向导为数据库中的所有对象或所选对象子集生成脚本。 该向导具有许多用于您的脚本的选项，例如是否包括权限、排序规则和约束等。 有关使用向导的说明，请参阅 [Generate and Publish Scripts Wizard](./generate-and-publish-scripts-wizard.md).
  
### <a name="object-explorer-script-as-menu"></a><a name="OEScriptAsMenu"></a> 对象资源管理器“编写脚本为”菜单

可以使用对象资源管理器“编写脚本为”菜单编写单个对象的脚本、编写多个对象的脚本或为单个对象编写多条脚本语句  。 您可以选择多种脚本类型之一；例如，创建、更改或删除对象。 您可以将脚本保存到查询编辑器窗口、文件或剪贴板。 脚本以 Unicode 格式创建。

## <a name="to-generate-a-script-of-a-single-object"></a><a name="ScriptSingleObject"></a> 生成单个对象的脚本

**编写单个对象的脚本**

1. 在对象资源管理器中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。

2. 展开 **“数据库”** ，然后展开包含要对其编写脚本的对象的数据库。

3. 展开该对象的类别。 例如，展开 **“表”** 或 **“视图”** 节点。

4. 右键单击该对象，指向“编写 \<object type> 脚本为”，例如，指向“编写表脚本为” 。

5. 指向脚本类型，如 **“创建到”** 或 **“更改到”** 。

6. 选择要保存该脚本的位置，如 **“新查询编辑器窗口”** 或 **“剪贴板”** 。

    ![编写表脚本](media/generate-scripts-sql-server-management-studio/script-table.png)

可以使用“对象资源管理器详细信息”窗格为相同类别的多个对象生成一个脚本  。

1. 在对象资源管理器中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。

2. 展开 **“数据库”** ，然后展开包含要对其编写脚本的对象的数据库。

3. 展开要编写脚本的对象类型的类别节点，如 **“表”** 节点。

4. 可通过选中 **F7** 或打开 **“视图”** 菜单并选择 **“对象资源管理器详细信息”** 来打开 **“对象资源管理器详细信息”** 窗格。

    ![“视图”菜单](media/generate-scripts-sql-server-management-studio/object-explorer-details-view-menu.png)

5. 左键单击要编写脚本的对象之一。

6. 然后，按住 Ctrl 并左键单击第二个要编写脚本的对象。

7. 右键单击所选对象之一，然后选择“编写 \<object type> 脚本为”。

    ![详细信息](media/generate-scripts-sql-server-management-studio/object-explorer-details.png)