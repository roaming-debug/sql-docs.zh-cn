---
description: ODBC 连接管理器
title: ODBC 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.odbcconnection.f1
helpviewer_keywords:
- connections [Integration Services], ODBC
- ODBC connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], ODBC
ms.assetid: e8c77aa7-6772-485e-918e-cef9eeb18c58
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7fe63ad503b49a760823635902b3744a1ae2d85a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91719387"
---
# <a name="odbc-connection-manager"></a>ODBC 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  ODBC 连接管理器使得包能够使用开放式数据库连接规范 (ODBC) 连接到多种数据库管理系统。  
  
 将 ODBC 连接添加到包并设置连接管理器的属性时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将创建连接管理器并将该连接管理器添加到包的 Connections 集合中。 该连接管理器在运行时决定物理 ODBC 连接。  
  
 该连接管理器的 **ConnectionManagerType** 属性设置为 **ODBC**。  
  
 可以用下列方式配置 ODBC 连接管理器：  
  
-   提供引用用户名或系统数据源名称的连接字符串。  
  
-   指定要连接的服务器。  
  
-   指示在运行时是否保留连接。  
  
## <a name="configuration-of-the-odbc-connection-manager"></a>配置 ODBC 连接管理器  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题之一：  
  
-   [ODBC 连接管理器 UI 参考]()  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)项目。  
  
## <a name="odbc-connection-manager-ui-reference"></a>ODBC 连接管理器用户界面参考
  可以使用 **“配置 ODBC 连接管理器”** 对话框为 ODBC 数据源添加连接。  
  
 若要了解有关 ODBC 连接管理器的详细信息，请参阅 [ODBC Connection Manager](../../integration-services/connection-manager/odbc-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **数据连接**  
 从列表中选择现有的 ODBC 连接管理器。  
  
 **数据连接属性**  
 查看所选 ODBC 连接管理器的属性和值。  
  
 **新建**  
 使用“连接管理器”对话框创建 ODBC 连接管理器。 通过此对话框，您也可以在需要时创建新的 ODBC 数据源。  
  
 **删除**  
 选择某个连接，然后可以使用“删除”按钮将其删除。  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
