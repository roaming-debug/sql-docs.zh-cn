---
title: 删除资源池 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 删除资源池。 你必须具有 CONTROL SERVER 权限。
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool delete
- resource pools [SQL Server], delete
ms.assetid: 3bdd348b-6582-4ffa-80ef-d49e50596ce5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2df6daca54fe31273d61226c210285fec305769d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504817"
---
# <a name="delete-a-resource-pool"></a>删除资源池
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 Transact-SQL 删除资源池。  
  
-   **开始之前：** [限制和局限](#LimitationsRestrictions)、[权限](#Permissions)  
  
-   **若要删除资源池，请使用：** [SQL Server Management Studio](#DelRPSSMS)、[Transact-SQL](#DelRPTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 如果资源池中包含工作负荷组，则不能删除该池。  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 不能删除资源调控器默认资源池或内部资源池。 如果资源池中包含工作负荷组，则不能删除该池。 有关详细信息，请参阅 [Delete a Workload Group](../../relational-databases/resource-governor/delete-a-workload-group.md)。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 删除资源池需要 CONTROL SERVER 权限。  
  
##  <a name="delete-a-resource-pool-using-object-explorer"></a><a name="DelRPSSMS"></a> 使用对象资源管理器删除资源池  
 **使用 SQL Server Management Studio 删除资源池**  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，打开对象资源管理器，并依次逐步展开 **“管理”** 节点直至其中并包含 **“资源调控器”** 。  
  
2.  右键单击要删除的资源池，然后单击“删除”。  
  
3.  在 **“删除对象”** 窗口的 **“要删除的对象”** 列表中，将列出资源池。 若要删除资源池，请单击 **“确定”** 。  

    > [!NOTE]  
    >  如果尝试删除的资源池中包含工作负荷组，则此操作将失败。  
  
##  <a name="delete-a-resource-pool-using-transact-sql"></a><a name="DelRPTSQL"></a> 使用 Transact-SQL 删除资源池  
 **使用 Transact-SQL 删除资源池**  
  
1.  运行 **DROP RESOURCE POOL** 或 **DROP EXTERNAL RESOURCE POOL** 语句，该语句指定要删除的资源池的名称。  
  
2.  运行 **ALTER RESOURCE GOVERNOR RECONFIGURE** 语句。  
  
### <a name="example-transact-sql"></a>示例 (Transact-SQL)  
 下面的示例删除名为 `poolAdhoc`的工作负荷组。  
  
```  
DROP RESOURCE POOL poolAdhoc;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [创建资源池](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [更改资源池设置](../../relational-databases/resource-governor/change-resource-pool-settings.md)   
 [资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [资源调控器分类器函数](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)  
  
  
