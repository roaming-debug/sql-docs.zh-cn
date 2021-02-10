---
description: 设置项目选项 (SybaseToSQL)
title: " (SybaseToSQL) 设置项目选项 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Project Options Setting
ms.assetid: 97b70fc8-1f68-4f15-8e22-db5b784ea4ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: fa8e45af93faa4ef175d129487a1d655a6d02ead
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030755"
---
# <a name="setting-project-options-sybasetosql"></a>设置项目选项 (SybaseToSQL)
对于每个 SSMA 项目，可以设置项目级别选项。 这些选项指定对象转换、对象加载、SQL azure、用户界面和数据迁移设置。 在将对象转换为或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure 或将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 之前，请验证配置选项是否适用于项目。  
  
SSMA 可让你配置所有项目的默认选项。 这些选项将应用于您创建的任何新项目。 然后，你可以自定义每个项目的选项。  
  
## <a name="configuration-options-and-modes"></a>配置选项和模式  
SSMA 有五组项目设置：  
  
1.  项目信息  
  
2.  常规 (转换、迁移和收集数据)   
  
3.  Synchronization  
  
4.  GUI  
  
5.  类型映射  
  
它还具有四种配置这些设置的模式：  
  
1.  默认  
  
2.  乐观  
  
3.  完全  
  
4.  自定义  
  
对于大多数用户，建议使用默认模式。 乐观模式使当前 Sybase 自适应服务器企业 (ASE) 语法更为简单，并且更易于阅读。 但是，保持当前语法可能不准确。 如果 ASE 语法必须转换为等效 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 语法，则完整模式将执行完整转换，但生成的代码可能更难以读取。 在 "自定义" 模式下，设置选项。  
  
此文档的 "用户界面参考" 部分介绍了这些设置。 有关设置以及如何在每个模式下应用这些设置的详细信息，请参阅以下主题：  
  
-   [&#40;转换的项目设置&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
-   [&#40;迁移的项目设置&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md)  
  
-   [&#40;同步的项目设置&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md)  
  
-   [&#40;GUI 的项目设置&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md)  
  
-   [项目设置 &#40;类型映射&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md)  
  
-   [&#40;Azure SQL 数据库的项目设置 &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md)  
  
## <a name="setting-project-options"></a>设置项目选项  
在 SSMA 中，可以配置所有项目的默认设置。 这些设置将保存到 SSMA 配置文件，并应用于你创建的任何新项目。  
  
**设置默认项目选项**  
  
1.  在 " **工具** " 菜单上，选择 " **默认项目设置**"。  
  
2.  在 " **默认项目设置** " 对话框中，使用以下过程之一：  
  
    -   从 " **迁移目标版本** " 下拉框中选择需要查看或更改其设置的 "迁移项目类型"，然后在左窗格底部单击 "常规"，然后选择 "转换" 或 "迁移" 或 SQL Azure。  
  
    -   若要选择预定义模式，请在 " **模式** " 下拉框中选择 " **默认**"、" **乐观**" 或 " **完整**"。  
  
    -   若要指定自定义设置，只需选择或输入新的设置或值。  
  
3.  单击“确定”保存设置。  
  
你还可以自定义当前项目的设置。 这些设置保存到当前项目文件中。  
  
**为当前项目自定义设置**  
  
1.  在 " **工具** " 菜单上，选择 " **项目设置**"。  
  
2.  在 " **项目设置** " 对话框中，使用以下过程之一：  
  
    -   若要选择预定义模式，请在 " **模式** " 下拉框中选择 " **默认**"、" **乐观**" 或 " **完整**"。  
  
    -   若要指定自定义模式，请在 " **模式** " 下拉框中选择 " **自定义**"，选择左窗格中的选项，在右窗格中单击 "设置" 或 "值"，然后选择或输入新的设置或值。  
  
3.  单击“确定”保存设置。  
  
## <a name="next-steps"></a>后续步骤  
迁移的下一步取决于你的项目需求：  
  
-   如果要自定义源和目标数据类型的映射，请参阅将 [SYBASE ASE 和 SQL Server 数据类型映射 &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md)。  
  
-   否则，你可以将 Sybase 数据库对象定义转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 对象定义。 有关详细信息，请参阅将 [SYBASE ASE 数据库对象转换 &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
