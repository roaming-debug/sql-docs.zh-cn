---
title: 查看对象上基于策略的管理 Facet
description: 介绍如何查看应用于 SQL Server Management Studio (SSMS) 中特定 SQL Server 对象的所有基于策略的管理 Facet。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view facets
ms.assetid: 5f423b9f-a6c4-41a7-9d8d-8f4926ce1fb4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c30bbe5fdd1b88420ae22cde6053487869d126b2
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077025"
---
# <a name="view-policy-based-management-facets-on-a-sql-server-object"></a>查看 SQL Server 对象上基于策略的管理 Facet
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中查看应用到特定 SQL Server 对象的所有基于策略的管理方面。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要查看对象的所有方面，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-all-of-the-facets-in-an-object"></a>查看对象的所有方面  
  
1.  在对象资源管理器中，右键单击 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例、实例对象、数据库或数据库对象，然后单击“Facet”  。  
  
2.  在“查看 Facet - object_name”对话框的“方面”列表中，选择某一方面，查看其属性    。 有关此对话框上可用选项的详细信息，请参阅 [View Facets Dialog Box](../../relational-databases/policy-based-management/view-facets-dialog-box.md)。  
  
3.  完成后，单击 **“确定”** 。  
  
  
