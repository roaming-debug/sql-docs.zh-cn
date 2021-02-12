---
description: 映射 MySQL 和 SQL Server 数据类型 (MySQLToSQL)
title: " (MySQLToSQL) 映射 MySQL 和 SQL Server 数据类型 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, customize data type mapping
- Mapping, Type mapping
ms.assetid: 14f98054-13b4-4231-a6b0-2452f3b9941d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f7024f944030714e7aa2431815e326b1be5be701
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077198"
---
# <a name="mapping-mysql-and-sql-server-data-types-mysqltosql"></a>映射 MySQL 和 SQL Server 数据类型 (MySQLToSQL)
MySQL 数据库类型与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库类型不同。 将 MySQL 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 对象时，必须指定如何将数据类型从 MySQL 映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 您可以接受默认的数据类型映射，也可以自定义映射，如下面的过程所示。  
  
## <a name="default-mappings"></a>默认映射  
SSMA 具有一组默认的数据类型映射。 有关默认映射的列表，请参阅 [项目设置 &#40;类型映射&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)。  
  
## <a name="type-mapping-inheritance"></a>类型映射继承  
您可以在项目级别自定义类型映射、对象类别级别 (例如所有存储过程) 或对象级别）。 除非在较低级别上被重写，否则将从较高级别继承设置。 例如，如果在项目级别将 **smallint** 映射到 **int** ，则项目中的所有对象都将使用此映射，除非你在对象或类别级别自定义映射。  
  
查看 SSMA 中的 " **类型映射** " 选项卡时，背景会进行颜色编码，以显示继承了哪些类型的映射。 对于任何继承的类型映射，类型映射的背景为黄色; 对于在当前级别指定的任何映射，背景为白色。  
  
## <a name="customizing-data-type-mappings"></a>自定义数据类型映射  
  
-   **映射数据类型：**  
  
    下面的过程演示如何在项目、数据库或数据库对象级别映射数据类型：  
  
    1.  若要为整个项目自定义数据类型映射，请打开 " **项目设置** " 对话框。 在 "工具" 菜单上，选择 " **项目设置**"。  
  
        在左窗格中，选择 " **类型映射**"。 类型映射图表和按钮将显示在右窗格中。  
  
    2.  若要在数据库级或表级自定义数据类型映射，请在 MySQL 元数据资源管理器中选择数据库或表。 在 MySQL 元数据资源管理器中，选择要自定义的文件夹或对象。  
  
        在右侧窗格中，单击 " **类型映射**"。  
  
-   **若要添加新映射，请执行以下操作：**  
  
    1.  在 "类型映射" 窗格中，单击 " **添加** "。  
  
    2.  在 "新建类型映射" 对话框中的 " **源类型**" 下，选择要映射的 MySQL 数据类型。  
  
    3.  如果类型需要长度，请通过选择 " **从** " 和 " **到** " 复选框，然后输入值来指定映射的最小和最大数据长度。  
  
    4.  这使您可以为相同数据类型的较小值和较大值自定义数据映射。 在 " **目标类型**" 下，选择目标 SQL Server 或 SQL Azure 数据类型。  
  
        1.  某些类型需要目标数据类型长度。 如果需要，请在 " **替换为** " 框中输入新的数据长度，然后单击 **"确定"**。  
  
        2.  某些类型需要目标数据类型的 **精度** 和 **小数位数**。 如果需要，请在 " **替换为** " 框中输入新的精度和小数位数，然后单击 **"确定"**。  
  
-   **若要编辑类型映射，请执行以下操作：**  
  
    1.  在 "类型映射" 窗格中，单击 " **编辑**"。  
  
    2.  在 "类型映射列表" 对话框中的 " **源类型**" 下，选择要映射的 MySQL 数据类型。  
  
    3.  如果类型需要长度，请通过选择 " **从** " 和 " **到** " 复选框，然后输入值来指定映射的最小和最大数据长度。  
  
    这使您可以为相同数据类型的较小值和较大值自定义数据映射。 在 " **目标类型**" 下，选择目标 SQL Server 或 SQL Azure 数据类型。  
  
    -  某些类型需要目标数据类型长度。 如果需要，请在 " **替换为** " 框中输入新的数据长度，然后单击 **"确定"**。  
  
    -  某些类型需要目标数据类型的 **精度** 和 **小数位数**。 如果需要，请在 " **替换为** " 框中输入新的精度和小数位数，然后单击 **"确定"**。  
  
-   **若要删除数据类型映射，请执行以下操作：**  
  
    1.  在 "类型映射" 窗格中，选择 "类型映射" 列表中包含要删除的数据类型映射的行。  
  
    2.  单击 **“删除”** 。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [创建一个评估报表](assessing-mysql-databases-for-conversion-mysqltosql.md) ，或 [将 MySQL 数据库对象转换为 SQL Server 或 SQL Azure 语法](converting-mysql-databases-mysqltosql.md)。 如果创建报表，则在评估期间会自动转换 MySQL 对象。  
  
## <a name="see-also"></a>另请参阅  
[将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
