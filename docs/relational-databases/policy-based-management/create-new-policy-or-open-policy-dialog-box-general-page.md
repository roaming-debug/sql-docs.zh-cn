---
description: “创建新策略”或“打开策略”对话框，“常规”页
title: “新建策略”或“打开策略”对话框，“常规”页面
descripton: Describes the 'General Page' of the 'Create New Policy' and 'Open Policy' dialog boxes for Policy-Based Management in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.policy.f1
- sql13.swb.dmf.policy.filter.f1
- sql13.swb.dmf.newgroup.f1
ms.assetid: c00bebd0-d04b-4c64-840e-8b7a2c603436
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 60997f5a657db79dc9e31c17c47f02bfc12b3cad
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127899"
---
# <a name="create-new-policy-or-open-policy-dialog-box-general-page"></a>“创建新策略”或“打开策略”对话框，“常规”页
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此对话框可创建新的基于策略的管理策略，或者修改现有策略。 可以将 **“针对目标”** 和 **“服务器限制”** 区域作为筛选器，将策略限制为所有可能的目标的子集。 对于要用作目标筛选器的条件，必须在物理方面中对其进行定义，并且不能包含函数和 LIKE 运算符。 在系统计算某一策略的对象集时，默认情况下将排除系统对象。  例如，如果该策略的对象集引用所有表，则该策略将不适用于系统表。 如果用户想要评估针对系统对象的策略，可以显式向对象集添加系统对象。 但是，尽管 **“按计划检查”** 评估模式支持所有策略，但出于性能原因， **“更改时检查”** 并不支持具有任意对象集的所有策略。 有关详细信息，请参阅 [https://docs.microsoft.com/archive/blogs/sqlpbm/policy-evaluation-modes](/archive/blogs/sqlpbm/policy-evaluation-modes)  
  
## <a name="options"></a>选项  
 **名称**  
 对于新策略，请键入新策略的名称。 对于现有策略，将显示其名称。  
  
 **已启用**  
 选中“已启用”复选框可启用策略。 清除 **“已启用”** 复选框可禁用策略。 **“已启用”** 框适用于策略自动化。 它将创建或删除策略的自动化系统。 自动化使用以下机制：  
  
 **更改时: 禁止**  
 数据库触发器强制要求符合策略。  
  
 **更改时: 仅记录**  
 通知服务事件检查是否符合策略。  
  
 **按计划**  
 创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业以定期检查是否符合策略。  
  
 使用 **“按需”** 评估模式运行的策略不使用此复选框。  
  
 **检查条件**  
 选择此策略使用的基于策略的管理条件。 将列出服务器上关联的基于策略的管理方面的所有条件。 单击 **“新建条件”** 可创建新的条件。 单击省略号“(…)”按钮可修改条件。  
  
 **“针对目标”**  
 选择此方面完成筛选表达式时可使用的目标类型。  
  
 **评估模式**  
 为策略选择评估模式。 可以检查某些策略，但不能强制实施这些策略。 评估模式包括：  
  
 **按需**  
 只能从“评估”对话框中运行策略。  
  
 **按计划**  
 定期评估策略、记录不符合要求的策略的日志条目并创建报告。 启用 **“计划”** 框。  
  
 **更改时: 仅记录**  
 当尝试进行更改时，此选项不会禁止不符合策略的更改，但会记录违反策略的情况。  
  
 **更改时: 禁止**  
 当尝试进行更改时，此选项禁止违反策略的更改。  
  
 **计划**  
 此选项在选择了“按计划”评估模式的情况下显示。 键入计划的名称，单击 **“选取”** ，从列表中选择一个计划，或者单击 **“新建”** 以创建新计划。 若要启用计划区域，必须选择 **“按计划”** 。  
  
 **“服务器限制”**  
 选择适用于此策略的服务器类型。 选项为 **“无”** ，或者选择一个条件以筛选可能的服务器。  
  
## <a name="see-also"></a>另请参阅  
 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
