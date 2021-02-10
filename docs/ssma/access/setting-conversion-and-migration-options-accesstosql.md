---
description: " (AccessToSQL) 设置转换和迁移选项"
title: " (AccessToSQL) 设置转换和迁移选项 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- conversion, setting options
- migration options
- modes
- options, conversion settings
- project settings
- schemas
ms.assetid: 0a7304df-2f35-4453-96ef-7ac83dea1167
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 154eaa42bf08f622f3e08359f025284752134bc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066312"
---
# <a name="setting-conversion-and-migration-options-accesstosql"></a> (AccessToSQL) 设置转换和迁移选项
对于每个 SSMA 项目，可以设置项目级别的选项。 这些选项指定如何转换对象、迁移数据的方式以及源数据类型映射到目标数据类型的方式。 在将对象转换为或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure 或将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 之前，请验证配置选项是否适用于项目。  
  
## <a name="configuration-options-and-modes"></a>配置选项和模式  
SSMA 有四组配置设置和四种配置这些设置的模式：默认、乐观、完整和自定义。 对于大多数用户，建议使用默认模式。 将乐观模式用于简单转换。 如果要查看所有消息，请使用完全模式。 在 "自定义" 模式下，设置选项。  
  
此文档的 "用户界面参考" 部分介绍了这些设置。 有关设置以及如何在每个模式下应用这些设置的详细信息，请参阅以下主题：  
  
-   [项目设置（转换）](./project-settings-conversion-accesstosql.md)  
  
-   [项目设置（迁移）](./project-settings-migration-accesstosql.md)  
  
-   [项目设置 (GUI)](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [项目设置（类型映射）](./project-settings-type-mapping-accesstosql.md)  
  
-   [项目设置 (SQL Azure) ](./project-settings-azure-sql-db-accesstosql.md)  
  
## <a name="setting-project-options"></a>设置项目选项  
在 SSMA 中，可以配置所有项目的默认设置。 这些设置将保存到 SSMA 配置文件，并应用于你创建的任何新项目。  
  
**设置默认项目选项**  
  
1.  在 " **工具** " 菜单上，选择 " **默认项目设置**"。  
  
2.  在 " **默认项目设置** " 对话框中，执行下列操作之一：  
  
    -   从 " **迁移目标版本** " 下拉框中选择 "需要查看或更改其设置" 的 "迁移项目类型"，单击左侧窗格底部的 " **常规** "，然后选择 " **转换" 或 "迁移" 或 SQL Azure**。  
  
        > [!NOTE]  
        > 仅当创建的项目类型为 SQL Azure 时，SQL Azure 选项才会出现在 " **常规** " 选项卡中。  
  
    -   若要选择预定义模式，请在 "**模式**" 下拉框中选择 "**默认**"、"**乐观**" 或 "**完全**"。  
  
    -   若要指定自定义模式，请在 "**模式**" 框中选择 "**自定义**"，在左窗格中选择一个选项，单击右窗格中的 "设置" 或 "值"，然后选择或输入新的设置或值。  
  
3.  单击“确定”保存设置。  
  
你还可以自定义当前项目的设置。 这些设置保存到当前项目文件中。  
  
**为当前项目自定义设置**  
  
1.  在 " **工具** " 菜单上，选择 " **项目设置**"。  
  
2.  在 " **项目设置** " 对话框中，执行下列操作之一：  
  
    -   若要选择预定义模式，请在 "**模式**" 下拉框中选择 "**默认**"、"**乐观**" 或 "**完全**"。  
  
    -   若要指定自定义模式，请在 "**模式**" 框中选择 "**自定义**"，在左窗格中选择一个选项，单击右窗格中的 "设置" 或 "值"，然后选择或输入新的设置或值。  
  
3.  单击“确定”保存设置。  
  
## <a name="next-steps"></a>后续步骤  
迁移的下一步取决于你的项目需求：  
  
-   若要自定义源和目标数据类型的映射，请参阅 [映射源和目标数据类型](mapping-source-and-target-data-types-accesstosql.md)  
  
-   若要自定义源和目标数据库的映射，请参阅 [映射源和目标数据库](mapping-source-and-target-databases-accesstosql.md)  
  
-   否则，你可以将 Access 数据库对象定义转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 对象定义。 有关详细信息，请参阅 [转换 Access 数据库对象](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
