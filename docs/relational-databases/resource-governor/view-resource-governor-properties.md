---
title: 查看资源调控器属性 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 中的“资源调控器属性”页创建或配置资源调控器实体。
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.rg.properties.f1
helpviewer_keywords:
- Resource Governor, properties
ms.assetid: de3510df-f792-4a9d-80fa-f198fd36cdc8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d89ade42fe716e2d457ce0cd5db200773f8a413c
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504781"
---
# <a name="view-resource-governor-properties"></a>查看 Resource Governor 属性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的“资源调控器属性”页创建或配置资源调控器实体（如资源池和工作负荷组）。  
  
 ##  <a name="related-topics"></a><a name="BeforeYouBegin"></a> 相关主题 
 除了查看资源调控器实体的属性外，还可以使用 **“资源调控器属性”** 页执行多个配置任务。 有关详细信息，请参阅以下主题：  
  
-   [启用资源调控器](../../relational-databases/resource-governor/enable-resource-governor.md)  
  
-   [禁用资源调控器](../../relational-databases/resource-governor/disable-resource-governor.md)  
  
-   [创建资源池](../../relational-databases/resource-governor/create-a-resource-pool.md)  
  
-   [创建工作负荷组](../../relational-databases/resource-governor/create-a-workload-group.md)  
  
-   [更改资源池设置](../../relational-databases/resource-governor/change-resource-pool-settings.md)  
  
-   [更改工作负荷组设置](../../relational-databases/resource-governor/change-workload-group-settings.md)  
  
-   [移动工作负荷组](../../relational-databases/resource-governor/move-a-workload-group.md)  
  
 添加、删除或移动工作负荷组或资源池后，当单击 **“确定”** 时，将执行 ALTER RESOURCE GOVERNOR RECONFIGURE 语句。  
  
 如果创建或重新配置工作负荷组或资源池的操作失败，错误消息摘要将显示在属性页标题下方。 若要查看详细的错误消息，请单击错误信息上的向下箭头。  
  
 可以通过查询 [sys.dm_resource_governor_configuration](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md) 动态管理视图来获取 is_configuration_pending 的当前状态以确定是否存在配置挂起。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 查看资源调控器属性需要 VIEW SERVER STATER 权限。 资源调控器配置任务需要 CONTROL SERVER 权限。  
  
##  <a name="resource-governor-properties-page"></a><a name="ViewRGProp"></a> 资源调控器属性页  
 **若要查看资源调控器属性，请使用资源调控器属性页 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中打开对象资源管理器，并依次逐步展开 **“管理”** 节点直至 **“资源调控器”** 。  
  
2.  右键单击“资源调控器”  ，然后单击“属性” ，这将打开“资源调控器属性”页  。  
  
3.  有关该页中的字段的说明，请参阅 [资源调控器属性](#RGProp)。  
  
4.  若要保存任何更改，请单击 **“确定”** 。  

##  <a name="resource-governor-properties"></a><a name="RGProp"></a> Resource Governor properties  
 **分类器函数名称**  
 通过从列表中选择来指定分类器函数。  
  
 **启用资源调控器**  
 通过选中或清除复选框来启用或禁用资源调控器。  
  
 **资源池**  
 通过使用提供的网格来创建或更改资源池及外部资源池配置。 此网格使用预定义的内部和默认池的信息进行填充。 通过单击池中某行的第一列来选择要处理的池。 若要创建新的资源池，请单击带星号 (&#42;) 前缀的行。  
  
 **名称**  
 指定资源池的名称。  
  
 **最低 CPU %**  
 指定出现 CPU 争用时资源池中的所有请求可获得的有保证的平均 CPU 带宽。 范围从 0 到 100。  
  
 **最高 CPU %**  
 指定出现 CPU 争用时，此资源池中的所有请求可获得的最大平均 CPU 带宽。 范围从 0 到 100。 默认设置为 100。  
  
 **最低内存 %**  
 指定为此资源池保留的不与其他资源池共享的最小内存量。 范围从 0 到 100。  
  
 **最高内存 %**  
 指定此资源池中的请求可使用的总服务器内存。 范围从 0 到 100。 默认设置为 100。  
  
 有关详细信息，请参阅 [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md) 和 [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)。  
  
 **资源池的工作负荷组**  
 通过使用提供的网格创建或更改工作负荷组配置。 此网格使用预定义的内部和默认组的信息进行填充。 通过单击池中某行的第一列来选择要处理的组。 若要创建新的工作组，请单击带星号 (&#42;) 前缀的行。  
  
 **名称**  
 指定工作负荷组的名称。  
  
 **重要性**  
 指定工作负荷组中的请求的相对重要性。 可用设置分为低、中和高三个级别。  
  
 **最大请求数**  
 指定允许在工作负荷组中同时执行的请求的最大数。 必须是 0 或一个正整数。  
  
 **CPU 时间(秒)**  
 指定请求可使用的最大 CPU 时间量。 必须是 0 或一个正整数。 如果为 0，则时间没有限制。  
  
 **内存授予 %**  
 指定单个请求可从池中获取的最大内存量。 范围从 0 到 100。  
  
 **授予超时值(秒)**  
 指定查询在失败之前等待资源变为可用的最长时间。 必须是 0 或一个正整数。  
  
 **并行度**  
 指定并行请求的最大并行度 (DOP)。 范围从 0 到 64。  
  
 有关详细信息，请参阅 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)。  
  
## <a name="view-resource-governor-properties-using-transact-sql"></a>使用 Transact-SQL 查看资源调控器属性  
 **使用 Transact-SQL 查看资源调控器属性**  
  
1.  若要查看资源调控器实体的定义，请使用[资源调控器目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)。  
  
2.  若要查看资源调控器实体的当前配置，请使用[与资源调控器相关的动态管理视图 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)。  
  
## <a name="more-information"></a>详细信息
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [启用资源调控器](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [资源调控器分类器函数](../../relational-databases/resource-governor/resource-governor-classifier-function.md)  
  
  
