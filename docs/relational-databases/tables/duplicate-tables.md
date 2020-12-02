---
description: 创建不含行数据的表的重复副本。
title: 复制表 | Microsoft Docs
ms.custom: ''
ms.date: 10/05/2020
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- copying tables
- tables [SQL Server], duplicating
- duplicating tables
- duplicating table structures
- table copying [SQL Server]
ms.assetid: c6b07423-d1e5-4e5e-8681-5088921f5df3
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a8faa1aab3237152934f0ce9eb4cb9ce541d5d3d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128675"
---
# <a name="duplicate-tables"></a>复制表
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，通过创建新表后从现有表复制列信息，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中复制现有表。  
  
> [!IMPORTANT]  
>  此操作仅复制表的结构，不复制任何表行。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **使用以下工具复制表：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 在目标数据库中要求 CREATE TABLE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-duplicate-a-table"></a>复制表  
  
1.  请确保您已经连接到要在其中创建表的数据库并在对象资源管理器中选中该数据库。  
  
2.  在对象资源管理器中，右键单击“表”，再单击“新建表”。  
  
3.  在对象资源管理器中，右键单击要复制的表，再单击“设计”。  
  
4.  在现有表中选择列，在 **“编辑”** 菜单上单击 **“复制”**。  
  
5.  切换回新表并选择第一行。  
  
6.  在 **“编辑”** 菜单上，单击 **“粘贴”**。  
  
7.  从“文件”菜单上，单击“保存”表格名称。  
  
8.  在 **“选择名称”** 对话框中，键入新表的名称，然后单击 **“确定”**。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-duplicate-a-table-in-query-editor"></a>在查询编辑器中复制表  
  
1.  请确保您已经连接到要在其中创建表的数据库并在对象资源管理器中选中该数据库。  
  
2.  右键单击要复制的表，指向“编写表脚本为”，然后指向“CREATE 到”，再选择“新查询编辑器窗口”。  
  
3.  更改表的名称。  
  
4.  删除新表中不需要的列。  
  
5.  单击“执行” 。  
