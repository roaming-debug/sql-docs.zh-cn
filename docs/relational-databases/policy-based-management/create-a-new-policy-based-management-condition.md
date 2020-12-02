---
description: 创建新的基于策略的管理条件
title: 创建新的基于策略的管理条件 | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policy conditions
ms.assetid: 8a612f7e-6c70-49db-a4de-48431e097cc5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 76c60d4561eacda47648d2cac710e64c2ccc1f4e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127914"
---
# <a name="create-a-new-policy-based-management-condition"></a>创建新的基于策略的管理条件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中创建基于策略的管理条件。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要创建条件，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-condition"></a>创建条件  
  
1.  在“对象资源管理器”中，单击加号以展开你要在其中创建基于策略的管理条件的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”**。  
  
4.  单击加号以便展开 **“方面”** 文件夹。  
  
5.  右键单击要在其中创建新条件的方面，然后选择“新建条件”。  
  
6.  在 **“创建新条件”** 对话框的 **“名称”** 框中，键入新条件的名称。  
  
7.  确认 **“方面”** 列表中的方面正确无误，或者选择一个不同的方面。  
  
8.  在 **“表达式”** 下，通过在 **“字段”** 框中选择一个方面属性及与其关联的运算符和值，构造条件表达式。 在添加多个表达式时，可使用 **And** 或 **Or** 来联接这些表达式。 有关此对话框中可用选项的详细信息，请参阅[“创建新条件”或“打开条件”对话框，“常规”页](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md)、[“创建新条件”或“打开条件”对话框，“说明”页](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md)和[“高级编辑(条件)”对话框](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md)。  
  
9. 完成后，单击 **“确定”** 。  
  
  
