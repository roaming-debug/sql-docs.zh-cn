---
description: '映射源和目标数据类型 (AccessToSQL) '
title: 映射源和目标数据类型 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- customizing data type mappings
- data types, mapping
- mapping, data types
- source data types
- target data types
ms.assetid: b362a075-16e7-423f-b63f-e1e9f02844a9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f5a725d0e2c6d9651884c399caabe2a8833826f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059039"
---
# <a name="mapping-source-and-target-data-types-accesstosql"></a>映射源和目标数据类型 (AccessToSQL) 
Access 数据库类型与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库类型不同。 将 Access 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象时，必须指定如何将数据类型从访问映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以接受默认的数据类型映射，也可以自定义映射，如下面的过程所示。  
  
## <a name="default-mappings"></a>默认映射  
SSMA 具有一组默认的数据类型映射。 有关默认映射的列表，请参阅 [项目设置 (类型映射) ](./project-settings-type-mapping-accesstosql.md)。  
  
## <a name="customizing-data-type-mappings"></a>自定义数据类型映射  
通过使用 " **项目设置** " 对话框，可以自定义为项目中的所有数据库和数据库对象映射类型的方式。 项目的类型映射适用于没有自定义类型映射的所有数据库和数据库对象。  
  
您还可以在数据库或表级别自定义数据类型映射。  
  
下面的过程演示如何在项目、数据库或数据库对象级别映射数据类型。  
  
**映射数据类型**  
  
1.  若要为整个项目自定义数据类型映射，请打开 " **项目设置** " 对话框：  
  
    1.  在 " **工具** " 菜单上，选择 " **项目设置**"。  
  
    2.  在左窗格中，选择 " **类型映射**"。  
  
        类型映射图表和按钮将显示在右窗格中。  
  
    或者，若要在数据库级或表级自定义数据类型映射，请在 "访问元数据资源管理器" 窗格中选择数据库或表：  
  
    1.  在 "访问元数据资源管理器" 窗格中，展开 " **access-元数据库**"，然后展开 " **数据库**"。  
  
    2.  选择要为其自定义数据类型映射的数据库或表。  
  
    3.  在右侧窗格中，单击 " **类型映射**"。  
  
2.  若要添加新映射，请执行以下操作：  
  
    1.  在 "类型映射" 窗格中，单击 " **添加**"。  
  
    2.  在 " **新建类型映射** " 对话框中的 " **源类型**" 下，选择要映射的访问数据类型。  
  
    3.  如果类型需要长度，请通过选择 " **从** " 和 " **到** " 复选框，然后输入值来指定映射的最小和最大数据长度。  
  
        这使您可以为相同数据类型的较小值和较大值自定义数据映射。  
  
    4.  在 " **目标类型**" 下，选择目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
        某些类型需要目标数据类型长度。 如果需要，请在 " **替换为** " 框中输入新的数据长度，然后单击 **"确定"**。  
  
3.  若要编辑数据类型映射，请执行以下操作：  
  
    1.  在 "类型映射" 窗格中，单击 " **编辑**"。  
  
    2.  在 " **类型映射列表** " 对话框中的 " **源类型**" 下，选择要映射的访问数据类型。  
  
    3.  如果类型需要长度，请通过选择 " **从** " 和 " **到** " 复选框，然后输入值来指定映射的最小和最大数据长度。  
  
        这使您可以为相同数据类型的较小值和较大值自定义数据映射。  
  
    4.  在 " **目标类型**" 下，选择目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
        某些类型需要目标数据类型长度。 如果需要，请在 " **替换为** " 框中输入新的数据长度，然后单击 **"确定"**。  
  
4.  若要删除数据类型映射，请执行以下操作：  
  
    1.  在 "类型映射" 窗格中，选择 "类型映射" 列表中包含要删除的数据类型映射的行。  
  
    2.  单击 **“删除”** 。  
  
## <a name="next-steps"></a>后续步骤  
迁移过程的下一步是 [将 access 数据库对象转换为 SQL Server 对象](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
