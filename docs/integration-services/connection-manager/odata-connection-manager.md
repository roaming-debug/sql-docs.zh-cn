---
description: OData 连接管理器
title: OData 连接管理器 | Microsoft Docs
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3caa4372-aff3-4c0f-9ecd-97870948b8d0
f1_keywords:
- sql13.dts.designer.odatasource.connectionmanager.f1
- sql13.dts.designer.odataconnectionmanager.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d02fbd8a7029a794b933053bec5f676646eced1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338420"
---
# <a name="odata-connection-manager"></a>OData 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 使用 OData 连接管理器连接到 OData 数据源。 OData 源组件使用 OData 连接管理器连接到 OData 数据源，并使用来自服务的数据。 有关详细信息，请参阅 [OData Source](../../integration-services/data-flow/odata-source.md)。  
  
## <a name="adding-an-odata-connection-manager-to-an-ssis-package"></a>向 SSIS 包添加 OData 连接管理器  
 可以通过三种方式向 SSIS 包添加新 OData 连接管理器：  
  
-   单击“OData 源编辑器”中的“新建…”按钮  
  
-   在“解决方案资源管理器”  中，右键单击“连接管理器” 文件夹，然后单击“新建连接管理器” 。 为 **“连接管理器类型”** 选择 **“ODATA”**。  
  
-   右键单击包设计器底部的“连接管理器”窗格，然后选择“新建连接”。 为 **“连接管理器类型”** 选择 **“ODATA”**。  
  
## <a name="connection-manager-authentication"></a>连接管理器身份验证  
 OData 连接管理器支持五种身份验证模式。  
  
-   Windows 身份验证  
  
-   基本身份验证（使用用户名和密码）  

-   Microsoft Dynamics AX Online（使用用户名和密码）
  
-   Microsoft Dynamics CRM Online （使用用户名和密码）
  
-   Microsoft Online Services（使用用户名和密码）  
  
对于匿名访问，请选择“Windows 身份验证”选项。  

要连接到 Microsoft Dynamics AX Online 或 Microsoft Dynamics CRM Online，不可使用 Microsoft Online Services 身份验证选项。 也不可使用任何为多重身份验证配置的选项。 当前不支持新式身份验证。 
  
### <a name="specifying-and-securing-credentials"></a>指定和保护凭据  
 如果 OData 服务需要基本身份验证，则可以在 [OData Connection Manager Editor]()中指定用户名和密码。 在编辑器中输入的值保留在包中。 密码值根据包保护级别进行加密。  
  
 有几种方法来参数化用户名和密码值或将其存储在包外。 例如，可以使用参数，或者在从 SQL Server Management Studio 运行包时直接设置连接管理器属性。  
  
## <a name="odata-connection-manager-properties"></a>OData 连接管理器属性  
 下表介绍 OData 连接管理器的属性。  
  
|属性|说明|  
|-|-|  
|Url|服务文档的 URL。|  
|UserName|用于进行身份验证的用户名（如果需要）。|  
|密码|用于进行身份验证的密码（如果需要）。|  
|ConnectionString|包括连接管理器的其他属性。|  
  
## <a name="odata-connection-manager-editor"></a>“OData 连接管理器编辑器”
  使用“OData 连接管理器编辑器”对话框可以添加与 OData 数据源的连接或者编辑现有连接。  
  
### <a name="options"></a>选项  
 **连接管理器名称**  
 连接管理器的名称。  
  
 **服务文档位置**  
 OData 服务的 URL。 例如： https://services.odata.org/V3/Northwind/Northwind.svc/。  
  
 **身份验证**  
选择以下选项之一：
-   **Windows 身份验证**。 对于匿名访问，请选择此选项。
-   **基本身份验证** 
-   **Microsoft Dynamics AX Online**，适用于 Dynamics AX Online
-   **Microsoft Dynamics CRM Online**，适用于 Dynamics CRM Online
-   **Microsoft Online Services**，适用于 Microsoft Online Services

如果选择 Windows 身份验证之外的选项，请输入用户名和密码。 

要连接到 Microsoft Dynamics AX Online 或 Microsoft Dynamics CRM Online，不可使用 Microsoft Online Services 身份验证选项。 也不可使用任何为多重身份验证配置的选项。

 **测试连接**  
 单击此按钮可测试与 OData 源的连接。