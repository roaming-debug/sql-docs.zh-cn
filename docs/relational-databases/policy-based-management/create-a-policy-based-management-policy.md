---
description: 创建基于策略的管理策略
title: 创建基于策略的管理策略 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policies
ms.assetid: b28bf963-89f9-4941-b6c1-6004fec347f1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8a27cb717d8a54de804469d210e0eaaf3b622dd9
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127920"
---
# <a name="create-a-policy-based-management-policy"></a>创建基于策略的管理策略
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中创建基于策略的管理策略。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要创建策略，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-policy"></a>创建策略  
  
1.  在“对象资源管理器”中，单击加号以展开你要在其中创建新的基于策略的管理策略的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”**。  
  
4.  右键单击“策略”文件夹，然后选择“新建策略”。  
  
5.  在 **“创建新策略”** 对话框的 **“名称”** 框中，键入新策略的名称。  
  
6.  如果希望在创建策略后立即启用该策略，请选中 **“已启用”** 复选框。 如果评估模式为 **“按需”**，则 **“已启用”** 复选框不可用。  
  
7.  在 **“检查条件”** 列表中，选择现有条件之一，或者选择 **“新建条件”**。 若要编辑某个条件，请选择该条件，然后单击省略号 ( **...** )。有关详细信息，请参阅 [创建新的基于策略的管理条件](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) 或 [查看或修改基于策略的管理条件的属性](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md)。  
  
8.  在 **“针对目标”** 框中，为此策略选择一种或多种目标类型。 有些条件和方面只能应用于某些类型的目标。 可用目标集显示在关联的框中。 展开 **“每个”** 可为某些类型的目标选择筛选条件。 如果此框中没有出现目标，则检查条件的作用域为服务器级别。  
  
9. 在 **“评估模式”** 框中，选择此策略的工作方式。 不同的条件可具有不同的有效评估模式。 有关有效评估模式的详细信息，请参阅 [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)。  
  
10. 如果要按计划评估策略，请将评估模式设置为 **“按计划”**，然后单击 **“选取”** 选择一个计划，或者单击 **“新建”** 创建一个新计划。  
  
11. 若要将策略限制为目标类型的子集，请在 **“服务器限制”** 框中选择限制条件，或者创建一个新条件。  
  
     有关 **“创建新策略”** 对话框中的可用选项的详细信息，请参阅 [“创建新策略”或“打开策略”对话框，“常规”页](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) 或 [“创建新策略”或“打开策略”对话框，“说明”页](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md)。  
  
12. 完成后，单击 **“确定”** 。  

