---
description: 启动或停止收集组
title: 启动或停止收集组 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- collection sets [SQL Server], stopping
- collection sets [SQL Server], starting
ms.assetid: 48a7b2fe-6bc3-4278-a7ec-1babc1290345
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1d62c72e43d9495874881ba3c029640626f129c8
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88471381"
---
# <a name="start-or-stop-a-collection-set"></a>启动或停止收集组
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中启动或停止收集组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [先决条件](#Prerequisites)  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **若要启动或停止收集组，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   数据收集器存储过程和目录视图存储在 **msdb** 数据库中。  
  
-   与常规存储过程不同的是，数据收集器存储过程的参数已严格类型化，不支持自动的数据类型转换。 如果这些参数不是使用正确的输入参数数据类型（正如参数说明中指定的一样）调用的，则存储过程会返回错误。  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   必须启动 SQL Server 代理。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   若要获取有关收集组的信息，请查询 [syscollector_collection_sets](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md) 目录视图。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 **dc_operator** 固定数据库角色的成员身份。 如果收集组没有代理帐户，则需要具有 **sysadmin** 固定服务器角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-start-a-collection-set"></a>启动收集组  
  
1.  在对象资源管理器中，依次展开 **“管理”** 节点、 **“数据收集”** 和 **“系统数据收集组”** 。  
  
2.  右键单击要启动的收集组，然后单击“启动数据收集组”。  

     将出现一个消息框，显示此操作的结果，收集组图标上的绿色箭头指示收集组已经启动。  
  
#### <a name="to-stop-a-collection-set"></a>停止收集组  
  
1.  在对象资源管理器中，依次展开 **“管理”** 节点、 **“数据收集”** 和 **“系统数据收集组”** 。  
  
2.  右键单击要停止的收集组，然后单击 **“停止数据收集组”** 。  
  
     将出现一个消息框，显示此操作的结果，收集组图标上的红色圆圈指示收集组已经停止。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-start-a-collection-set"></a>启动收集组  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例使用 [sp_syscollector_start_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-start-collection-set-transact-sql.md) 启动 ID 为 `1`的收集组。  
  
```sql  
USE msdb;  
GO  
EXEC sp_syscollector_start_collection_set @collection_set_id = 1;  
```  
  
#### <a name="to-stop-a-collection-set"></a>停止收集组  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例使用 [sp_syscollector_stop_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-stop-collection-set-transact-sql.md) 停止 ID 为 `1`的收集组。  
  
```sql  
USE msdb;  
GO  
EXEC sp_syscollector_stop_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据收集器视图 (Transact-SQL)](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  
