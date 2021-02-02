---
title: 查看基于策略的管理 Facet 的属性
description: 了解如何使用 SQL Server Management Studio (SSMS) 查看基于策略的管理 Facet 的属性。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view facet properties
ms.assetid: 022a244c-c2e7-4467-b9a2-c7a27859be22
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 97f5cbfccd7cc086163b73642d0c6ec3e928427a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076189"
---
# <a name="view-the-properties-of-a-policy-based-management-facet"></a>查看基于策略的管理方面的属性
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中查看基于策略的管理方面的属性。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要查看某个方面的属性，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-a-facet"></a>查看方面的属性  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开您要查看其属性的方面所在的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”** 。  
  
4.  单击加号以便展开 **“方面”** 文件夹。  
  
5.  右键单击要查看其属性的方面，然后选择  “属性”。 有关“Facet 属性 -facet_name”对话框中可用选项的详细信息，请参阅[“Facet 属性”对话框、“常规”页](../../relational-databases/policy-based-management/facet-properties-dialog-box-general-page.md)[“Facet 属性”对话框、“依赖策略”页](../../relational-databases/policy-based-management/facet-properties-dialog-box-dependent-policies-page.md)和[“Facet 属性”对话框、“依赖条件”页](../../relational-databases/policy-based-management/facet-properties-dialog-box-dependent-conditions-page.md)。  
  
6.   完成后，单击“关闭”。  

