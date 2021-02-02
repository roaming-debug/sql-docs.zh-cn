---
title: 查看或修改基于策略的管理条件的属性
description: 了解如何使用 SQL Server Management Studio (SSMS) 或 Transact-SQL (T-SQL) 查看或修改基于策略的管理条件的属性。
ms.custom: ''
ms.date: 10/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view policy conditions
- Policy-Based Management, modify policy conditions
ms.assetid: 890d7384-8444-4767-bb6f-f5debb155747
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ffdb11c75726469d61c54c9a2cc12ff18a31ca1a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076179"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-condition"></a>查看或修改基于策略的管理条件的属性
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中查看或修改基于策略的管理条件的属性。  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  

  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-or-modify-a-conditions-properties"></a>查看或修改条件的属性  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要查看或修改的条件的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”** 。  
  
4.  单击加号以便展开 **“条件”** 文件夹。  
  
5.  右键单击要查看或编辑的条件，然后选择“属性”  。 若要深入了解“打开条件 - condition_name”对话框中的可用选项，请参阅[“创建新条件”或“打开条件”对话框，“常规”页](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md)、[“打开条件”对话框，“依赖策略”页](../../relational-databases/policy-based-management/open-condition-dialog-box-dependent-policies-page.md)、[“创建新条件”或“打开条件”对话框，“说明”页](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md)和[“高级编辑”（条件）对话框](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md)。  
  
6.  完成后，单击 **“确定”** 。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-a-conditions-properties"></a>查看条件的属性  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       description,  
       facet,  
       expression,  
       is_name_condition,  
       obj_name  
    FROM syspolicy_conditions;  
    GO  
  
    ```  
  
 有关详细信息，请参阅 [syspolicy_conditions (Transact-SQL)](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md)。  
  
  
