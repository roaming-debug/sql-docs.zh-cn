---
title: 删除工作负荷组 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 删除工作负载组或资源池。 必须具有 CONTROL SERVER 权限。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- workload groups [SQL Server], delete
- Resource Governor, workload group delete
ms.assetid: d5902c46-5c28-4ac1-8b56-cb4ca2b072d0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27708e9a4b5a6d0a2863595e7ee25dccf77e5d2f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506593"
---
# <a name="delete-a-workload-group"></a>删除工作负荷组
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 Transact-SQL 删除工作负荷组或资源池。  
  
-   **开始之前：** [限制和局限](#LimitationsRestrictions)、[权限](#Permissions)  
  
-   **若要删除工作负荷组，请使用：** [对象资源管理器](#DelWGObjEx)、[Resource Governor 属性](#DelWGRGProp)、[Transact-SQL](#DelWGTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 如果工作负荷组中包含活动会话，则不能删除该组。  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 如果工作负荷组包含活动会话，则在调用 ALTER RESOURCE GOVERNOR RECONFIGURE 语句应用更改时，删除工作负荷组或将其移至其他资源池中会失败。 若要避免此问题，可以执行以下操作之一：  
  
-   等待受影响组的所有会话均断开连接，然后重新运行 ALTER RESOURCE GOVERNOR RECONFIGURE 语句。  
  
-   使用 KILL 命令显式停止受影响的组中的会话，然后重新运行 ALTER RESOURCE GOVERNOR RECONFIGURE 语句。 如果决定不打算在使用“删除”之后同时在停止活动会话之前显式停止会话，请使用原始名称重新创建组并将组移至原始资源池。  
  
-   重新启动服务器。 完成重新启动过程后，将不会创建已删除的组，并且已移动的组将使用新分配的资源池。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 删除工作负荷组需要 CONTROL SERVER 权限。  
  
##  <a name="delete-a-workload-group-using-object-explorer"></a><a name="DelWGObjEx"></a> 使用对象资源管理器删除工作负荷组  
 **使用对象资源管理器删除工作负荷组**  
  
1.  在[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，打开对象资源管理器，并依次逐步展开 **“管理”** 节点直至其中包含 **“资源池”** 。  
  
2.  在包含要删除的工作负荷组的资源池中，依次逐步展开 **“资源池”** 节点直至其中包含 **“工作负荷组”** 节点。  
  
3.  右键单击工作负荷组，然后单击“删除”。  
  
4.  在 **“删除对象”** 窗口的 **“要删除的对象”** 列表中，将列出工作负荷组。 若要删除工作负荷组，请单击 **“确定”** 。  
  
##  <a name="delete-a-workload-group-using-resource-governor-properties"></a><a name="DelWGRGProp"></a> 使用资源调控器属性删除工作负荷组  
 **使用“资源调控器属性”页删除工作负荷组**  
  
1.  在对象资源管理器中，依次向下展开 **“管理”** 节点直至其中包括 **“资源池”** 。  
  
2.  右键单击包含要删除的工作负荷组的资源池，然后单击“属性”。 这将打开 **“资源调控器属性”** 页。  
  
3.  在 **“资源池的工作负荷组”** 窗口中，单击要删除的工作负荷组所在的行，再右键单击该行左侧的向右箭头，然后单击“删除”。  
  
4.  若要删除工作负荷组，请单击 **“确定”** 。  
  
##  <a name="delete-a-workload-group-using-transact-sql"></a><a name="DelWGTSQL"></a> 使用 Transact-SQL 删除工作负荷组  
 **使用 Transact-SQL 删除工作负荷组**  
  
1.  运行 **DROP WORKLOAD GROUP** 语句，该语句指定要删除的工作负荷组的名称。  
  
2.  在发出 **ALTER RESOURCE GOVERNOR RECONFIGURE** 语句之前，请确认要删除的工作负荷组中没有活动请求。 如果有活动请求，则 **ALTER RESOURCE GOVERNOR** 将失败。 若要避免此问题，您可以执行下列操作之一：  
  
    -   等待工作负荷组中的所有会话都断开连接。  
  
    -   通过使用 **KILL** 命令显式停止工作负荷组中的会话。  
  
    -   重新启动服务器。 工作负荷组将不会重新创建。  
  
    -   在已发出 **DROP WORKLOAD GROUP** 语句但决定不打算显式停止会话以应用更改的情况下，你可以使用在发出 DROP 语句之前组所具有的名称来重新创建组，然后将该组移动到原始资源池。  
  
3.  运行 **ALTER RESOURCE GOVERNOR RECONFIGURE** 语句。  
  
### <a name="example-transact-sql"></a>示例 (Transact-SQL)  
 下面的示例删除名为 `groupAdhoc`的工作负荷组。  
  
```  
DROP WORKLOAD GROUP groupAdhoc;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [创建资源池](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [创建工作负荷组](../../relational-databases/resource-governor/create-a-workload-group.md)   
 [删除资源池](../../relational-databases/resource-governor/delete-a-resource-pool.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
