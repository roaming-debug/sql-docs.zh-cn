---
description: 修改唯一约束
title: 修改唯一约束 | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- modifying constraints
- UNIQUE constraints [SQL Server], modifying
- constraints [SQL Server], modifying
- constraints [SQL Server], unique
ms.assetid: fddbdc9e-958b-4614-8e88-6ca205d64a4e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0546bd8ed476891a177f5246c0ff65859835a0fe
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472618"
---
# <a name="modify-unique-constraints"></a>修改唯一约束

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中修改唯一约束。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **使用以下工具修改唯一约束：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-a-unique-constraint"></a>修改唯一约束  
  
1.  在“对象资源管理器”中，右键单击包含唯一约束的表，然后选择“设计”。  
  
2.  在“表设计器”菜单上，单击“索引/键...” 。  
  
3.  在“索引/键”对话框中的“选定的主/唯一键或索引”下，选择要编辑的约束。  
  
4.  完成下表中的相应操作：  
  
    |功能|需要遵循的步骤|  
    |--------|------------------------|  
    |更改与约束关联的列|1) 在“(常规)”下的网格中，单击“列”，再单击属性右侧的省略号 (…)。<br /><br /> 2) 在“索引列”对话框中，为索引指定新列和/或排序顺序。|  
    |重命名约束|在 **“标识”** 下的网格中，在 **“名称”** 框中键入新名称。 确保新名称不与“选定的主/唯一键或索引”列表中的名称重复。|  
    |设置聚集选项|在“表设计器”下的网格中，选择“创建为群集索引”，并从下拉列表中选择“是”创建群集索引，或选择“否”创建非聚集索引。 对于每个表，只允许存在一个聚集索引。 如果此表中已经存在聚集索引，则您必须首先对原始索引清除此设置。|  
    |定义填充因子|在 **“表设计器”** 下的网格中，展开 **“填充规范”** 类别，然后在 **“填充因子”** 框中键入一个 0 到 100 之间的整数。|  
  
5.  在“文件”菜单上，单击“保存表名称”。  
  
##  <a name="to-modify-a-unique-constraint"></a><a name="TsqlProcedure"></a> **修改唯一约束**  
  
 若要使用 Transact-SQL 修改 UNIQUE 约束，必须首先删除现有的 UNIQUE 约束，然后用新定义重新创建。 有关详细信息，请参阅 [Delete Unique Constraints](../../relational-databases/tables/delete-unique-constraints.md) 和 [Create Unique Constraints](../../relational-databases/tables/create-unique-constraints.md)。  
  
###  <a name="TsqlExample"></a>  
