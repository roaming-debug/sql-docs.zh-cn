---
title: " (OracleToSQL) 映射 Oracle 和 SQL Server 数据类型 |Microsoft Docs"
description: 了解如何自定义 Oracle 数据类型与 SQL Server 的 Oracle 映射的 SSMA 或接受默认值。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Type Mapping Inheritance
ms.assetid: 05da1495-63b9-47b7-86e2-6746394a2d8a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8058123020571ad43e3142fccb920aa22a4b5af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080774"
---
# <a name="mapping-oracle-and-sql-server-data-types-oracletosql"></a>映射 Oracle 和 SQL Server 数据类型 (OracleToSQL)
Oracle 数据库类型与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库类型不同。 将 Oracle 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象时，必须指定如何将数据类型从 oracle 映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 你可以接受默认的数据类型映射，也可以自定义映射，如以下部分所示。  
  
## <a name="default-mappings"></a>默认映射  
SSMA 具有一组默认的数据类型映射。 有关默认映射的列表，请参阅 [项目设置 &#40;类型映射&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md)。  
  
## <a name="type-mapping-inheritance"></a>类型映射继承  
您可以在项目级别自定义类型映射、对象类别级别 (例如所有存储过程) 或对象级别）。 除非在较低级别上被重写，否则将从较高级别继承设置。 例如，如果在项目级别将 **smallmoney** 映射到 **money** ，则项目中的所有对象都将使用此映射，除非你在对象或类别级别自定义映射。  
  
查看 SSMA 中的 " **类型映射** " 选项卡时，背景会进行颜色编码，以显示继承了哪些类型的映射。 对于任何继承的类型映射，类型映射的背景为黄色; 对于在当前级别指定的任何映射，背景为白色。  
  
## <a name="customizing-data-type-mappings"></a>自定义数据类型映射  
下面的过程演示如何在项目、数据库或对象级别映射数据类型：  
  
**映射数据类型**  
  
1.  若要为整个项目自定义数据类型映射，请打开 " **项目设置** " 对话框：  
  
    1.  在 " **工具** " 菜单上，选择 " **项目设置**"。  
  
    2.  在左窗格中，选择 " **类型映射**"。  
  
        类型映射图表和按钮将显示在右窗格中。  
  
    或者，若要在数据库、表、视图或存储过程级别自定义数据类型映射，请在 Oracle 元数据资源管理器中选择数据库、对象类别或对象：  
  
    1.  在 Oracle 元数据资源管理器中，选择要自定义的文件夹或对象。  
  
    2.  在右侧窗格中，单击 " **类型映射** " 选项卡。  
  
2.  若要添加新映射，请执行以下操作：  
  
    1.  单击 **添加**。  
  
    2.  在 " **源类型**" 下，选择要映射的 Oracle 数据类型。  
  
    3.  如果类型需要长度，请在 " **从** " 框中指定映射的最小数据长度，在 " **到** " 框中指定最大数据长度。  
  
        这使您可以为相同数据类型的较小值和较大值自定义数据映射。  
  
    4.  在 " **目标类型**" 下，选择目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
        某些类型需要目标数据类型长度。 如果需要，请在 " **替换为** " 框中输入新的数据长度。  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  若要修改数据类型映射，请执行以下操作：  
  
    1.  单击 **“编辑”** 。  
  
    2.  在 " **源类型**" 下，选择要映射的 Oracle 数据类型。  
  
    3.  如果类型需要长度，请在 " **从** " 框中指定映射的最小数据长度，在 " **到** " 框中指定最大数据长度。  
  
        这使您可以为相同数据类型的较小值和较大值自定义数据映射。  
  
    4.  在 " **目标类型**" 下，选择目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
        某些类型需要目标数据类型长度。 如果需要，请在 " **替换为** " 框中输入新的数据长度，然后 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  若要删除自定义数据类型映射，请执行以下操作：  
  
    1.  在 "类型映射" 列表中选择包含要删除的数据类型映射的行。  
  
    2.  单击 **“删除”** 。  
  
        不能删除继承的映射。 但是，在特定对象或对象类别上，自定义映射会重写继承的映射。  
  
## <a name="next-steps"></a>后续步骤  
迁移过程的下一步是 [创建一个评估报表](assessing-oracle-schemas-for-conversion-oracletosql.md) 或 [将 Oracle 数据库对象转换为 SQL Server 语法](converting-oracle-schemas-oracletosql.md)。 如果创建评估报告，Oracle 对象会在评估期间自动转换。  
  
## <a name="see-also"></a>另请参阅  
[将 Oracle 数据库迁移到 SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
