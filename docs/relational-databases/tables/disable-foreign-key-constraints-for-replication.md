---
description: 对复制禁用外键约束
title: 对复制禁用外键约束 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- constraints [SQL Server], foreign keys
- foreign keys [SQL Server], disabling constraints
- disabling constraints
ms.assetid: 4211f2fd-d16a-4081-995c-43f1f0827f0b
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b443eb3facb44dccecbabc7e7fbde0527dd4850
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474628"
---
# <a name="disable-foreign-key-constraints-for-replication"></a>对复制禁用外键约束
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中对复制禁用外键约束。 如果正在从以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布数据，这很有用。  
  
> [!NOTE]  
>  如果表是使用复制发布的，则对于复制代理执行的操作将自动禁用外键约束。 当复制代理在订阅服务器上执行插入、更新或删除操作时，将不检查约束；如果用户执行插入、更新或删除操作，则检查约束。 由于最初插入、更新或删除数据时已经在发布服务器上检查过约束，所以对于复制代理将禁用该约束。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **对复制禁用外键约束，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-disable-a-foreign-key-constraint-for-replication"></a>对复制禁用外键约束  
  
1.  在 **“对象资源管理器”** 中，展开具有要修改的外键约束的表，再展开 **“键”** 文件夹。  
  
2.  右键单击外键约束，再单击“修改”。  
  
3.  在 **“外键关系”** 对话框，针对 **“强制用于复制”** 选择 **“否”** 值。  
  
4.  单击“关闭”。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-disable-a-foreign-key-constraint-for-replication"></a>对复制禁用外键约束  
  
1.  若要在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中执行此任务，请删除外键约束。 然后添加一个新外键约束，并指定 NOT FOR REPLICATION 选项。  
  
 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。  
  
###  <a name="TsqlExample"></a>  
