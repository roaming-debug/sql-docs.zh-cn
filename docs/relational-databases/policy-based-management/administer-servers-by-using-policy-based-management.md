---
title: 使用基于策略的管理来管理服务器
description: 了解如何使用基于策略的管理来管理 SQL Server 的一个或多个实例。
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- facet See facets
- Declarative Management Framework See Policy-Based Management
- surface area configuration [SQL Server], Policy-Based Management
- Policy-Based Management
- facets [Policy-Based Management]
- Policy-Based Management, administering
- conditions [Policy-Based Management]
- facets [Policy-Based Management], about facets
- PolicyAdministratorRole role
ms.assetid: ef2a7b3b-614b-405d-a04a-2464a019df40
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e60a5d8bdb987e3be2016b8b0395f05e83f9f7a9
ms.sourcegitcommit: 0f484f32709a414f05562bbaafeca9a9fc57c9ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2020
ms.locfileid: "94631753"
---
# <a name="administer-servers-by-using-policy-based-management"></a>使用基于策略的管理来管理服务器
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   基于策略的管理是一个基于策略的系统，用于管理一个或多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 用于创建包含条件表达式的条件。 然后创建一些策略，将这些条件应用于数据库目标对象。  

例如，作为数据库管理员，你可能希望确保某些服务器没有启用数据库邮件，因此，可创建设置该服务器选项的条件和策略。 
   
 > **重要说明!!** 策略会影响某些功能的工作方式。 例如，变更数据捕捉和事务复制都使用没有索引的 systranschemas 表。 如果启用所有表都必须有索引的策略，则强制策略的兼容会导致这些功能都失效。  
  
 使用 SQL Server Management Studio 创建和管理策略：
  
1.  选择一个基于策略的管理方面，其中包含要配置的属性。  
  
2.  定义一个条件以指定管理方面状态。  
  
3.  定义一个策略，其中包含该条件、用于筛选目标集的附加条件以及评估模式。  
  
4.  检查 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是否符合该策略。  
  
 对于失败的策略，对象资源管理器以红色图标的形式指示严重运行状态警告，此图标位于该目标以及对象资源管理器树中此目标上面的节点旁边。  
  
> **注意：** 在系统计算某一策略的对象集时，默认情况下将排除系统对象。  例如，如果该策略的对象集引用所有表，则该策略将不适用于系统表。 如果用户想要评估针对系统对象的策略，可以显式向对象集添加系统对象。 但是，尽管 **“按计划检查”** 评估模式支持所有策略，但出于性能原因， **“更改时检查”** 并不支持具有任意对象集的所有策略。 有关详细信息，请参阅 [https://docs.microsoft.com/archive/blogs/sqlpbm/policy-evaluation-modes](/archive/blogs/sqlpbm/policy-evaluation-modes)  
  
## <a name="three-policy-based-management-components"></a>三个基于策略的管理组件  
 基于策略的管理包含以下三个组件：  
  
-   策略管理。 策略管理员创建策略。  
  
-   显式管理。 管理员选择一个或多个管理目标，并显式地检查这些目标是否符合特定策略，或者显式地使这些目标符合策略。  
  
-   评估模式。 共有四种评估模式；其中三种模式可以自动执行：  
  
    -   **按需**。 当此模式由用户直接指定时，将对策略进行评估。  
  
    -   **更改时: 禁止**。 这种自动模式使用 DDL 触发器来防止违反策略。  
  
        > **重要说明！** 如果禁用了嵌套触发器服务器配置选项，  “更改时: 禁止”将不会正常工作。 基于策略的管理依靠 DDL 触发器来检测和回滚不符合使用此评估模式的策略的 DDL 操作。 删除基于策略的管理 DDL 触发器或禁用嵌套触发器将导致此评估模式失败或意外执行。  
  
    -   **更改时: 仅记录**。 当做出相关更改时，这种自动模式使用事件通知对策略进行评估。  
  
    -   **按计划**。 这种自动模式使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业定期对策略进行评估。  
  
     在未启用自动策略时，基于策略的管理不会影响系统性能。  
  
## <a name="terms"></a>术语  
 **基于策略的管理目标** 基于策略的管理所管理的实体，例如，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例、数据库、表或索引。 服务器实例中的所有目标构成了目标层次结构。 目标集是将一组目标筛选器应用于目标层次结构后得到的一组目标，例如，HumanResources 架构所拥有的数据库中的所有表。  
  
 **基于策略的管理方面** 一组逻辑属性，用于模拟某些类型的管理目标的行为或特征。 这些属性的数量和特征内置在方面中，只能由方面创建者添加或删除这些内容。 一种目标类型可以实现一个或多个管理方面，而一个管理方面可以由一种或多种目标类型进行实现。 方面的某些属性仅适用于特定版本。  
  
 **基于策略的管理条件**  
 一个布尔表达式，用于针对管理方面指定基于策略的管理托管目标的一组允许状态。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在评估条件时尝试遵守排序规则。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则与 Windows 排序规则不完全匹配时，请测试您的条件，以确定该算法如何解决冲突。  
  
 **基于策略的管理策略**  
 基于策略的管理条件和预期行为，例如，评估模式、目标筛选器以及计划。 每个策略只能包含一个条件。 您可以启用或禁用策略。 策略存储在 msdb 数据库中。  
  
 **基于策略的管理策略类别**  
 用于帮助管理策略的用户定义类别。 用户可以将策略划分到不同的策略类别。 每个策略属于一个策略类别，且仅属于一个策略类别。 策略类别应用于数据库和服务器。 在数据库级别，应用以下条件：  
  
-   数据库所有者可以为数据库订阅一组策略类别。  
  
-   仅订阅类别中的策略能够控制数据库。  
  
-   所有数据库隐式订阅默认策略类别。  
  
 在服务器级别，策略类别可以应用于所有数据库。  
  
 **有效策略**  
 目标的有效策略是那些控制此目标的策略。 仅当满足所有以下条件时，策略才对目标有效：  
  
-   已启用此策略。  
  
-   目标属于此策略的目标集。  
  
-   目标或目标祖先之一订阅了包含此策略的策略组。  
  
## <a name="links-to-specific-tasks"></a>特定任务的链接 

 - [存储基于策略的管理策略。](policy-based-management-storage.md)
 - [配置警报以通知策略管理员策略失败情况](../../relational-databases/policy-based-management/configure-alerts-to-notify-policy-administrators-of-policy-failures.md)
 - [创建新的基于策略的管理条件](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md)
 - [删除基于策略的管理条件](../../relational-databases/policy-based-management/delete-a-policy-based-management-condition.md)
 - [查看或修改基于策略的管理条件的属性](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md)
 - [导出基于策略的管理策略](../../relational-databases/policy-based-management/export-a-policy-based-management-policy.md)
 - [导入基于策略的管理策略](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md)
 - [评估来自对象的基于策略的管理策略](../../relational-databases/policy-based-management/evaluate-a-policy-based-management-policy-from-an-object.md)
 - [处理基于策略的管理方面](../../relational-databases/policy-based-management/working-with-policy-based-management-facets.md)
 - [使用基于策略的管理来监视和强制执行最佳实践](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)

## <a name="see-also"></a>另请参阅  
 
 - [教程：创建并应用 off-by-default 策略](lesson-1-create-and-apply-an-off-by-default-policy.md)
 - [教程：创建并应用命名标准策略](lesson-2-create-and-apply-a-naming-standards-policy.md)
 - [基于策略的管理视图 (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)