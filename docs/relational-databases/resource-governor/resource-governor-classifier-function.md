---
title: 资源调控器分类器函数 | Microsoft Docs
description: SQL Server 资源调控器分类过程会根据传入会话的特征将其分配给工作负荷组。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, classifier function
- user-defined functions [SQL Server], classifier function
- classifier function [SQL Server]
- classifier function [SQL Server], overview
ms.assetid: 64c25012-7068-476f-afa2-0b4f3adde9a4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 78992e730afb36dc1c44fac297da0951cff80ee3
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504803"
---
# <a name="resource-governor-classifier-function"></a>Resource Governor Classifier Function
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源调控器分类过程会根据传入会话的特征将其分配给工作负荷组。 您可以通过编写用户定义函数（称为分类器函数）来定制分类逻辑。  
  
## <a name="classification"></a>分类  
 资源调控器支持对传入会话的分类。 分类基于函数中包含的一组用户编写的条件。 函数逻辑的结果使资源调控器可以将会话归入现有工作负荷组类。  
  
> [!NOTE]  
>  内部工作负荷组中填入的是仅供内部使用的请求。 您不能更改用于路由这些请求的标准，也不能将请求归入内部工作负荷组类。  
  
 您可以编写一个标量函数，在其中包含用于将传入会话分配给工作负荷组的逻辑。 必须先完成下列操作，才能使用此函数：  
  
-   使用 ALTER RESOURCE GOVERNOR 语句创建并注册此函数。 有关详细信息，请参阅 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)。  
  
-   使用带 RECONFIGURE 参数的 ALTER RESOURCE GOVERNOR 语句更新资源调控器配置。  
  
 创建此函数并应用配置更改后，资源调控器分类器将使用此函数返回的工作负荷组名称将新请求发送到相应的工作负荷组。  
  
> [!IMPORTANT]  
>  如果分类函数没有在指定的登录超时内完成，则客户端会话将超时。 登录超时是客户端属性，因此服务器意识不到超时。长时间运行的分类器函数可使服务器长期保留孤立连接。 创建在连接超时之前完成其执行的分类器函数非常重要。  
  
 用户定义的函数具有以下特征和行为：  
  
-   将针对每个新会话评估用户定义函数，即使在启用了连接池的情况下也会这样做。  
  
-   用户定义的函数为会话提供工作负荷组上下文。 确定组成员身份后，此会话将在会话的整个生存期内一直绑定到工作负荷组。  
  
-   如果用户定义的函数返回默认值 NULL 或不存在的组名称，则会话将拥有默认工作负荷组上下文。 如果由于任何原因而导致函数失败，则会话也将拥有默认上下文。  
  
-   函数应在服务器范围（master 数据库）内定义。  
  
-   分类器用户定义的函数的指定只在执行 ALTER RESOURCE GOVERNOR RECONFIGURE 后生效。  
  
-   一次只能将一个用户定义的函数指定为分类器。  
  
-   除非删除分类器用户定义函数的分类器状态，否则不能删除或更改分类器用户定义的函数。  
  
-   如果缺少分类器用户定义的函数，则将所有会话归入默认组类。  
  
-   分类器函数返回的工作负荷组位于架构绑定限制的作用域之外。 例如，不能删除表，但可以删除工作负荷组。  
  
> [!IMPORTANT]  
>  建议在服务器上启用专用管理员连接 (DAC)。 DAC 不进行资源调控器分类，因此可用于监视分类器函数并排除其故障。 有关详细信息，请参阅 [用于数据库管理员的诊断连接](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)。 如果没有 DAC 可用来进行故障排除，则另一个选择是在单用户模式下重新启动系统。 虽然单用户模式不进行分类，但是它使您无法在资源调控器分类运行时对它进行诊断。  
  
### <a name="classification-process"></a>分类过程  
 在资源调控器上下文中，会话的登录过程包含下列步骤：  
  
1.  登录身份验证  
  
2.  LOGON 触发器执行（仅当实例中存在登录触发器时才发生。）
  
3.  分类  

 分类启动时，资源调控器执行分类器函数，并使用函数返回的值将请求发送到相应的工作负荷组。  
  
> [!NOTE]  
>  在 [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) 和 [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)中公开有关分类器函数和 LOGON 触发器的执行信息。  
  
## <a name="classification-function-tasks"></a>分类函数任务  
  
|任务说明|主题|  
|----------------------|-----------|  
|说明如何创建和测试分类器用户定义函数。|[创建和测试分类器用户定义函数](../../relational-databases/resource-governor/create-and-test-a-classifier-user-defined-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [启用资源调控器](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [资源调控器工作负荷组](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [使用模板配置资源调控器](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [查看资源调控器属性](../../relational-databases/resource-governor/view-resource-governor-properties.md)  
  
  
