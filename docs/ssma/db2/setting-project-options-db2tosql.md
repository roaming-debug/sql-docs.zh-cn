---
description: " (DB2ToSQL) 设置项目选项"
title: " (DB2ToSQL) 设置项目选项 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f325a606-97ac-48bc-b344-b55f5e086a48
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 72e4fadcd1422ea0afacc0af5e8766ebff9e6386
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071931"
---
# <a name="setting-project-options-db2tosql"></a> (DB2ToSQL) 设置项目选项
对于每个 SSMA 项目，可以设置项目级别选项。 这些选项指定对象转换、对象加载、用户界面和数据迁移设置。 在将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或将数据迁移到之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请验证配置选项是否适用于项目。  
  
SSMA 可让你配置所有项目的默认选项。 这些选项将应用于您创建的任何新项目。 然后，你可以自定义每个项目的选项。  
  
## <a name="configuration-options-and-modes"></a>配置选项和模式  
SSMA 有五组项目设置：  
  
-   项目信息  
  
-   常规 (转换、迁移、加载对象)   
  
-   Synchronization  
  
-   GUI  
  
-   类型映射  
  
它还具有四种配置这些设置的模式：  
  
-   默认  
  
-   乐观  
  
-   完全  
  
-   自定义  
  
对于大多数用户，建议使用默认模式。 乐观模式会保留更多当前 DB2 语法，并且更易于阅读。 但是，保持当前语法可能不准确。 如果必须将 DB2 语法转换为等效 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法，则完整模式将执行最完整的转换，但生成的代码可能更难以读取。 在 "自定义" 模式下，设置选项。  
  
有关设置以及如何在每个模式下应用这些设置的详细信息，请参阅以下主题：  
  
-   [&#40;转换的项目设置&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md)  
  
-   [&#40;迁移的项目设置&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md)  
  
-   [&#40;同步的项目设置&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md)  
  
-   [&#40;GUI 的项目设置&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md)  
  
-   [项目设置 &#40;类型映射&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md)  
  
## <a name="setting-project-options"></a>设置项目选项  
在 SSMA 中，可以配置所有项目的默认设置。 这些设置将保存到 SSMA 配置文件，并应用于你创建的任何新项目。  
  
**设置默认项目选项**  
  
1.  在 " **工具** " 菜单上，单击 " **默认项目设置**"。  
  
2.  在 " **默认项目设置** " 对话框中，使用以下过程之一：  
  
    -   从 " **迁移目标版本** " 下拉框中选择需要查看或更改其设置的 "迁移项目类型"，然后在左窗格底部单击 " **常规** "，然后选择 "转换" 或 "迁移"。  
  
    -   若要选择预定义模式，请在 " **模式** " 下拉框中选择 " **默认**"、" **乐观**" 或 " **完整**"。  
  
    -   若要指定自定义设置，请选择或输入新的设置或值。  
  
3.  单击“确定”保存设置。  
  
你还可以自定义当前项目的设置。 这些设置保存到当前项目文件中。  
  
**为当前项目自定义设置**  
  
1.  在 " **工具** " 菜单上单击 " **项目设置**"。  
  
2.  在 " **项目设置** " 对话框中，使用以下过程之一：  
  
    -   若要选择预定义模式，请在 " **模式** " 下拉框中选择 " **默认**"、" **乐观**" 或 " **完整**"。  
  
    -   若要指定自定义模式，请在 " **模式** " 框中选择 " **自定义**"，然后选择相应的项目设置。  
  
3.  单击“确定”保存设置。  
  
## <a name="next-steps"></a>后续步骤  
迁移的下一步取决于你的项目需求：  
  
-   若要自定义源和目标数据类型的映射，请参阅 [映射 DB2 和 SQL Server 数据类型 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md)。  
  
-   否则，您可以将 DB2 数据库对象定义转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象定义。 有关详细信息，请参阅将 [DB2 架构转换 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[映射 DB2 和 SQL Server 数据类型 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md)  
  
