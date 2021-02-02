---
description: 删除基于策略的管理条件
title: 删除基于策略的管理条件 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, delete policy conditions
ms.assetid: e04148b8-f6dd-4c50-a5ef-c650b71dbf4d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: fb34d9ab423008e8b3fe2c8e926f0f397c14b5cb
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049174"
---
# <a name="delete-a-policy-based-management-condition"></a>删除基于策略的管理条件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中删除基于策略的管理条件。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要删除条件，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-delete-a-condition"></a>删除条件  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要删除的条件的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”**。  
  
4.  单击加号以便展开 **“条件”** 文件夹。  
  
5.  右键单击要删除的条件，然后选择“删除”。  
  
6.  在 **“删除对象”** 对话框中，确保已选择正确的条件，然后单击 **“确定”**。  

