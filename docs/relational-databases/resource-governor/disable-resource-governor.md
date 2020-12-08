---
title: 禁用资源调控器 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 禁用 Resource Governor。 你必须具有 CONTROL SERVER 权限。
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, disabling
ms.assetid: 2c2d2db0-34a5-4f50-b783-17693e3ce3f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5c3d7c13a76f0b6cb36aab11d7beb4c1b162044a
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504802"
---
# <a name="disable-resource-governor"></a>禁用资源调控器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 Transact-SQL 禁用资源调控器。  
  
-   **开始之前：** [限制和局限](#LimitationsRestrictions)、[权限](#Permissions)  
  
-   **若要禁用 Resource Governor，请使用：** [对象资源管理器](#RGOffObjEx)、[Resource Governor 属性](#RGOffProp)、[Transact-SQL](#RGOffTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 禁用资源调控器会产生下列结果：  
  
-   不运行分类器函数。  
  
-   所有新连接被自动分类到默认工作负荷组中。  
  
-   系统发起的请求被分类到内部工作负荷组中。  
  
-   所有现有的工作负荷组和资源池设置被重置为其默认值。 在这种情况下，到达限制时不触发任何事件。  
  
-   正常的系统监视不受影响。  
  
-   可以进行配置更改，但是这些更改直到启用资源调控器之后才会生效。  
  
-   在重新启动 SQL Server 时，资源调控器不会加载其配置，而是只具有默认的和内部的工作负荷组和资源池。  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 在用户事务中时，您不能使用 **ALTER RESOURCE GOVERNOR** 语句禁用资源调控器。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 禁用资源调控器需要 CONTROL SERVER 权限。  
  
##  <a name="disable-resource-governor-using-object-explorer"></a><a name="RGOffObjEx"></a> 使用对象资源管理器禁用资源调控器  
 **使用对象资源管理器禁用资源调控器**  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中打开对象资源管理器，并依次逐步展开 **“管理”** 节点直至 **“资源调控器”** 。  
  
2.  右键单击“资源调控器”，再单击“禁用”。  

##  <a name="disable-resource-governor-using-resource-governor-properties"></a><a name="RGOffProp"></a> 使用资源调控器属性禁用资源调控器  
 **使用“资源调控器属性”页禁用资源调控器**  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中打开对象资源管理器，并依次逐步展开 **“管理”** 节点直至 **“资源调控器”** 。  
  
2.  右键单击“资源调控器”  ，然后单击“属性” ，这将打开“资源调控器属性”页  。  
  
3.  单击 **“启用资源调控器”** 复选框，确保未选中该框，然后单击 **“确定”** 。  
  
##  <a name="disable-resource-governor-using-transact-sql"></a><a name="RGOffTSQL"></a> 使用 Transact-SQL 禁用资源调控器  
 **使用 Transact-SQL 禁用资源调控器**  
  
1.  运行 **ALTER RESOURCE GOVERNOR DISABLE** 语句。  
  
### <a name="example-transact-sql"></a>示例 (Transact-SQL)  
 以下示例启用资源调控器。  
  
```  
ALTER RESOURCE GOVERNOR DISABLE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [启用资源调控器](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [资源调控器分类器函数](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
