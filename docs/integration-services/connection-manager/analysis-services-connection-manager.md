---
description: Analysis Services 连接管理器
title: Analysis Services 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 01/25/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- connections [Integration Services], Analysis Services
- connection managers [Integration Services], Analysis Services
- Analysis Services connection manager
ms.assetid: 9f9cadad-a1d0-4db5-98f5-df5dbbec1be4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 633dd8288a9168422d0e5187caa1265615911a8a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130617"
---
# <a name="analysis-services-connection-manager"></a>Analysis Services 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器使包能够连接到运行 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库的服务器，或连接到用于访问多维数据集和维度数据的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目。 在 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 中开发包时，仅可连接到 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]项目。 在运行时，包会连接到您已部署 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目的服务器和数据库。  
  
 任务（如 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 执行 DDL 任务和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 处理任务）和目标（如数据挖掘模型定型目标）都使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器。  
  
 有关 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库的详细信息，请参阅[多维模型数据库 (SSAS)](/analysis-services/multidimensional-models/multidimensional-model-databases-ssas)。  
  
## <a name="configuration-of-the-analysis-services-connection-manager"></a>Analysis Services 连接管理器的配置  
 将 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器添加到包时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建在运行时作为 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接进行解析的连接管理器，同时还会设置该连接管理器的属性，并将该连接管理器添加到包的 Connections 集合。 该连接管理器的 **ConnectionManagerType** 属性设置为 **MSOLAP100**。  
  
 可以通过下列方式配置 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器：  
  
-   提供配置为符合 Microsoft OLE Provider for Analysis Services 访问接口要求的连接字符串。  
  
-   指定要连接到的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的实例或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目。  
  
-   如果要连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的实例，则应指定身份验证模式。  

> [!NOTE]    
>  如果在 Azure 数据工厂 (ADF) 中使用 SSIS 并希望连接到 Azure Analysis Services (AAS) 实例，则不能使用已启用多重身份验证 (MFA) 的帐户，而必须使用不需要任何交互性/MFA 的账户或服务主体。 如要使用后者，请参阅[此处](/azure/analysis-services/analysis-services-service-principal)创建一个并为其分配服务器管理员角色，然后选择“使用指定用户名和密码”以登录到连接管理器中的服务器，最后输入 `User name: app:YourApplicationID` 和 `Password: YourAuthorizationKey`。
  
-   指示是否在运行时保留从连接管理器中创建的连接。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题之一：  
  
-   [“添加 Analysis Services 连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)项目。  
  
