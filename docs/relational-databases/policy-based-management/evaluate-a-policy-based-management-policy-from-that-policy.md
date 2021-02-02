---
title: 根据基于策略的管理策略评估该策略
description: 了解如何使用 SQL Server Management Studio (SSMS) 根据 SQL Server 中的策略评估策略。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, evaluate policy
ms.assetid: 0b3214bd-d0ab-45ab-9281-3d95507abe54
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d2df2f0ea2c9f582053c10330931ed88e92bd172
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049030"
---
# <a name="evaluate-a-policy-based-management-policy-from-that-policy"></a>从基于策略的管理策略评估该策略
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中使用策略来评估该策略。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要评估某个策略，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-evaluate-a-policy"></a>评估策略  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要评估的策略的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”** 。  
  
4.  单击加号以便展开 **“策略”** 文件夹。  
  
5.  右键单击要评估的策略，然后选择“评估”  。  
  
6.  在 **“评估结果”**  对话框中，可查看策略评估的结果。 对于不符合策略且具有可通过基于策略的管理进行重新配置的属性的目标，可以通过单击“应用”  强制符合策略。 有关 **“评估策略** 对话框中的可用选项的详细信息，请参阅 [Evaluate Policies Dialog Box, Policy Selection Page](../../relational-databases/policy-based-management/evaluate-policies-dialog-box-policy-selection-page.md) 和 [Evaluate Policies Dialog Box, Evaluation Results Page](../../relational-databases/policy-based-management/evaluate-policies-dialog-box-evaluation-results-page.md)。  
  
7.   完成后，单击“关闭”。  

