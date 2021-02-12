---
description: 设置项目选项 (MySQLToSQL)
title: " (MySQLToSQL) 设置项目选项 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Setting project options,configuration options
ms.assetid: 08820d88-e157-4d49-9401-38580dd7ec2d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75c4f6ca899135141aa68ec147889fcce8cfa549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074508"
---
# <a name="setting-project-options-mysqltosql"></a>设置项目选项 (MySQLToSQL)
对于每个 SSMA 项目，可以设置项目级别的选项。 这些选项指定如何转换对象、迁移数据的方式以及源数据类型映射到目标数据类型的方式。  在将对象转换为 SQL Server 或 SQL Azure 或将数据迁移到 SQL Server 或 SQL Azure 之前，请验证配置选项是否适用于该项目。  
  
SSMA 可让你配置所有项目的默认选项。 这些选项将应用于您创建的任何新项目。 然后，你可以自定义每个项目的选项。  
  
## <a name="configuration-options-and-modes"></a>配置选项和模式  
SSMA 有五组项目设置：  
  
-   项目信息  
  
-   常规 (转换、迁移和 SQL Azure)   
  
-   Synchronization  
  
-   GUI  
  
-   类型映射  
  
可以通过以下四种方式配置项目设置：  
  
-   默认  
  
-   乐观  
  
-   完全  
  
-   自定义  
  
对于大多数用户，建议使用默认模式。 乐观模式将保留最新的 MySQL 语法，并且更易于阅读。 但是，保持当前语法可能不准确。 如果 MySQL 语法必须转换为等效的 SQL Server 或 SQL Azure 语法，则完整模式将执行最完整的转换。 但是，生成的代码可能更难以读取。 在 "自定义" 模式下，可以设置选项。  
  
有关设置以及如何在每个模式下应用这些设置的详细信息，请参阅以下主题：  
  
-   [&#40;转换的项目设置&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
-   [&#40;迁移的项目设置&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md)  
  
-   [ (GUI 的项目设置)  (SSMA 通用) ](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [项目设置 &#40;类型映射&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)  
  
-   [&#40;同步的项目设置&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
-   [&#40;Azure SQL 数据库的项目设置&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md)  
  
## <a name="setting-project-options"></a>设置项目选项  
在 SSMA 中，可以配置所有项目的默认设置。 这些设置将保存到 SSMA 配置文件，并应用于你创建的任何新项目。  
  
**设置默认项目选项**  
  
1.  在 " **工具** " 菜单上，单击 " **默认项目设置**"。  
  
2.  在 " **默认项目设置** " 对话框中，使用以下过程之一：  
  
    1.  从 " **迁移目标版本** " 下拉框中选择 "需要查看或更改其设置" 的 "迁移项目类型"，单击左侧窗格底部的 " **常规** "，然后选择 " **转换" 或 "迁移" 或 SQL Azure** 选项。  
  
    2.  若要选择预定义模式，请从 "**模式**" 下拉框中选择 "**默认**"、"**乐观**" 或 "**完整**"。  
  
    3.  若要指定自定义设置，请选择或输入新的设置或值。  
  
3.  单击“确定”保存设置。  
  
你还可以自定义当前项目的设置。 设置将保存到当前项目文件中。  
  
**为当前项目自定义设置**  
  
1.  在 " **工具** " 菜单上，单击 " **ProjectSettings**"。  
  
2.  在 " **ProjectSettings** " 对话框中，使用以下过程之一：  
  
    1.  若要选择预定义模式，请从 "**模式**" 下拉框中选择 "**默认**"、"**乐观**" 或 "**完整**"。  
  
    2.  若要指定自定义模式，请从 "**模式**" 下拉框中选择 "**自定义**"。 然后选择适当的项目设置。  
  
3.  单击“确定”保存设置。  
  
## <a name="next-step"></a>下一步  
迁移的下一步取决于你的项目需求：  
  
-   若要自定义源和目标数据类型的映射，请参阅 [&#40;MySQLToSQL 映射 MySQL 和 SQL Server 数据类型&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
-   否则，你可以将 MySQL 数据库对象定义转换为 SQL Server 或 SQL Azure 对象定义。 有关详细信息，请参阅将 [MySQL 数据库转换 &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>另请参阅  
[&#40;MySQLToSQL&#41;映射 MySQL 和 SQL Server 数据类型 ](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
