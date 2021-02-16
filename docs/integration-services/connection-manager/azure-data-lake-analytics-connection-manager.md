---
title: Azure Data Lake Analytics 连接管理器 | Microsoft Docs
description: SQL Server Integration Services (SSIS) 包可以使用 Azure Data Lake Analytics 连接管理器连接到 Data Lake Analytics 帐户。
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSCM.F1
- sql14.dts.designer.afpadlscm.f1
ms.assetid: f4c44553-0f08-4731-ac47-7534990b8c8d
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: ff7ad8e742469f7afea8770a7323352e99ee41ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338454"
---
# <a name="azure-data-lake-analytics-connection-manager"></a>Azure Data Lake Analytics 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



SQL Server Integration Services (SSIS) 包可使用 Azure Data Lake Analytics 连接管理器，通过下列两种身份验证类型之一连接到 Data Lake Analytics 帐户：
-   Azure Active Directory (Azure AD) 用户标识
-   Azure AD 服务标识 

Data Lake Analytics 连接管理器是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组成部分。
 
## <a name="configure-the-connection-manager"></a>配置连接管理器

1. 在“添加 SSIS 连接管理器”  对话框中，依次选择“AzureDataLakeAnalytics”   > “添加”  。 此时将打开“Azure Data Lake Analytics 连接管理器编辑器”  对话框。
  
2. 在“Azure Data Lake Analytics 连接管理器编辑器”  对话框中的“ADLA 帐户名”  字段内，输入 Data Lake Analytics 帐户名。 例如：myadlaaccountname。
  
3. 在“身份验证”  字段中，选择相应的身份验证类型，以用于访问 Data Lake Analytics 中的数据。

   a. 如果选择“Azure AD 用户标识”  身份验证选项，请执行以下操作：
   
      i. 为“用户名”  和“密码”  字段提供相应的值。    
      ii. 选择“测试连接”  以测试连接。 如果你或租户管理员之前禁止 SSIS 访问 Data Lake Analytics 帐户，请在出现提示时选择“接受”  。 若要深入了解此同意体验，请参阅 [Integrating applications with Azure Active Directory](/azure/active-directory/manage-apps/plan-an-application-integration#integrating-applications-with-azure-ad)（将应用程序与 Azure Active Directory 集成）。
    
   > [!NOTE] 
   > 如果选择了“Azure AD 用户标识”  身份验证选项，则不支持多重身份验证和 Microsoft 帐户身份验证。
    
   b.保留“数据库类型”设置，即设置为“共享”。 如果选择“Azure AD 服务标识”  身份验证选项，请执行以下操作：
   
      i. 创建 Azure AD 应用程序和服务主体，用于访问 Data Lake Analytics 帐户。 若要深入此身份验证选项，请参阅 [Use portal to create Active Directory application and service principal that can access resources](/azure/azure-resource-manager/resource-group-create-service-principal-portal)（使用门户创建可访问资源的 Active Directory 应用程序和服务主体）。    
      ii. 分配相应权限，让此 Azure AD 应用程序能够访问 Data Lake Analytics 帐户。 了解如何使用[“添加用户向导”](/azure/data-lake-analytics/data-lake-analytics-manage-use-portal#add-a-new-user)授予对 Data Lake Analytics 帐户的权限。    
      iii. 提供“应用程序 ID”  、“身份验证密钥”  和“租户 ID”  字段的值。    
      iv. 选择“测试连接”  以测试连接。  

4. 选择“确定”  以关闭“Azure Data Lake Analytics 连接管理器编辑器”  对话框。  

## <a name="view-the-properties-of-the-connection-manager"></a>查看连接管理器的属性
你可以看到你在“属性”  窗口中创建的连接管理器的属性。  
  
