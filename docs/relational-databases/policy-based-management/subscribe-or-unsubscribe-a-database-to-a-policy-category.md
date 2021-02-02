---
title: 将数据库设置为订阅或取消订阅某个策略类别
description: 介绍如何使用 SQL Server Management Studio 和 Transact-SQL (T-SQL) 将数据库设置为订阅或取消订阅某个策略类别，以进行基于策略的管理。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.groupsubscription.f1
ms.assetid: d2c31769-7098-428e-ad9c-ef56541b7c52
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: deb83a831430dd6ad57bf50eca26b9e30c49f22c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076209"
---
# <a name="subscribe-or-unsubscribe-a-database--to-a-policy-category"></a>建立数据库对某个策略类别的订阅或取消数据库对该类别的订阅
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中将数据库设置为订阅或取消订阅某个策略类别。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **建立数据库对某个策略类别的订阅或取消数据库对该类别的订阅，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 db_owner 固定数据库角色中的成员资格。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-subscribe-or-unsubscribe-a-database-to-a-policy-category"></a>建立数据库对某个策略类别的订阅或取消数据库对该类别的订阅  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含您要管理类别订阅的数据库的服务器。  
  
2.  单击加号以展开 **“数据库”** 文件夹。  
  
3.  右键单击要管理类别订阅的数据库，指向“策略”  ，然后选择“类别”   
  
     在 **“类别”** 对话框中提供了以下选项：  
  
     展开列  
     单击可展开策略类别。 这会列出此类别中包括的所有策略。  
  
     **名称**  
     策略类别的名称。  
  
     **已订阅**  
     指示目标是否已订阅此策略类别。 如果禁用此复选框，则为 **“托管数据库订阅”** 设置此策略类别。 这意味着该策略类别应用于服务器上的所有数据库。  
  
     **策略**  
     展开策略组将显示策略类别中包括的策略。  
  
     **已启用**  
     指示是启用还是禁用了策略。  
  
     **执行模式**  
     显示策略的执行模式。  
  
     **History**  
     单击“查看历史记录”超链接可打开日志文件查看器，从而可以查看策略历史记录。  
  
4.  若要订阅某个基于策略的管理类别，请选中“已订阅”  列下该类别的复选框。 若要从类别中取消订阅，请清除该复选框。  
  
5.  完成后，单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-subscribe-a-database-to-a-policy-category"></a>建立数据库对某个策略类别的订阅  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    USE AdventureWorks2012;  
    -- Adds a subscription to the 'Finance' policy category for the AdventureWorks2012 database.  
    EXEC sys.sp_syspolicy_subscribe_to_policy_category @policy_category = N'Finance';  
    GO  
    ```  
  
 有关详细信息，请参阅 [sp_syspolicy_subscribe_to_policy_category (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syspolicy-subscribe-to-policy-category-transact-sql.md)。  
  
#### <a name="to-unsubscribe-a-database-to-a-policy-category"></a>取消数据库对某个策略类别的订阅  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    USE AdventureWorks2012;  
    -- Deletes a subscription to the 'Finance' policy category for the AdventureWorks2012 database.  
    EXEC sys.sp_syspolicy_unsubscribe_from_policy_category @policy_category = N'Finance';  
    GO  
    ```  
  
 有关详细信息，请参阅 [sp_syspolicy_unsubscribe_from_policy_category (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md)。  
  
  
