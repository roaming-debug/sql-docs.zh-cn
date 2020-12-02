---
description: HTTP 连接管理器
title: HTTP 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.httpconnection.server.f1
- sql13.dts.designer.httpconnection.proxy.f1
helpviewer_keywords:
- HTTP connection manager
- Web site connections [Integration Services]
- connection managers [Integration Services], HTTP
- Web server connections [Integration Services]
- connections [Integration Services], HTTP
ms.assetid: 26b2b3e1-d02c-46ca-8d31-7aef2bbc3c53
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cd88a7f40787ecc33d99fe0c749c477f23b526a3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88496118"
---
# <a name="http-connection-manager"></a>HTTP 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  利用 HTTP 连接，包可以使用 HTTP 访问 Web 服务器以发送或接收文件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含的 Web 服务任务使用此连接管理器。  
  
 将 HTTP 连接管理器添加到包时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建将在运行时决定 HTTP 连接的连接管理器，设置该连接管理器的属性，并将该连接管理器添加到包的 **Connections** 集合。  
  
 该连接管理器的 **ConnectionManagerType** 属性设置为 **HTTP.**。  
  
 可以通过下列方式配置 HTTP 连接管理器：  
  
-   使用凭据。 如果连接管理器使用凭据，则其属性包含用户名、密码和域。  
  
    > [!IMPORTANT]  
    >  HTTP 连接管理器仅支持匿名身份验证和基本身份验证， 而不支持 Windows 身份验证。  
  
-   使用客户端证书。 如果连接管理器使用客户端证书，则其属性包含证书名称。  
  
-   提供连接到服务器的超时值和写入数据的块区大小。  
  
-   使用代理服务器。 也可以将代理服务器配置为使用凭据，并跳过代理服务器而使用本地地址。  
  
## <a name="configuration-of-the-http-connection-manager"></a>HTTP 连接管理器的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>。  
  
## <a name="http-connection-manager-editor-server-page"></a>HTTP 连接管理器编辑器（“服务器”页）
  使用 **“HTTP 连接管理器编辑器”** 对话框的 **“服务器”** 选项卡，可以通过指定 URL 和安全凭据等属性来配置 HTTP 连接管理器。 利用 HTTP 连接，包可以使用 HTTP 访问 Web 服务器以发送或接收文件。 配置 HTTP 连接管理器后，还可以测试该连接。  
  
> [!IMPORTANT]  
>  HTTP 连接管理器仅支持匿名身份验证和基本身份验证， 而不支持 Windows 身份验证。  
  
 若要了解有关 HTTP 连接管理器的详细信息，请参阅 [HTTP Connection Manager](../../integration-services/connection-manager/http-connection-manager.md)。 若要了解有关 HTTP 连接管理器的常见使用方案的详细信息，请参阅 [Web Service Task](../../integration-services/control-flow/web-service-task.md)。  
  
### <a name="options"></a>选项  
 **服务器 URL**  
 键入服务器的 URL。  
  
 如果您计划使用 **“Web 服务任务编辑器”** 的 **“常规”** 页上的 **“下载 WSDL”** 按钮来下载 WSDL 文件，请键入 WSDL 文件的 URL。 此 URL 要以“?wsdl”结尾。  
  
 **使用凭据**  
 指定是否希望 HTTP 连接管理器使用用户安全凭据进行身份验证。  
  
 **用户名**  
 如果 HTTP 连接管理器使用凭据，则必须指定用户名、密码和域。  
  
 **密码**  
 如果 HTTP 连接管理器使用凭据，则必须指定用户名、密码和域。  
  
 **Domain**  
 如果 HTTP 连接管理器使用凭据，则必须指定用户名、密码和域。  
  
 **使用客户端证书**  
 指定是否希望 HTTP 连接管理器使用客户端证书进行身份验证。  
  
 **证书**  
 使用“选择证书”对话框从列表中选择证书。 文本框显示与此证书关联的名称。  
  
 **超时值(秒)**  
 提供连接 Web 服务器时允许的超时值。 此属性的默认值为 30 秒。  
  
 **块区大小(KB)**  
 提供用于写入数据的块区大小。  
  
 **测试连接**  
 在配置 HTTP 连接管理器后，请通过单击“测试连接”确认该连接是否正常。  
  
## <a name="http-connection-manager-editor-proxy-page"></a>HTTP 连接管理器编辑器（“代理”页）
  可以使用 **“HTTP 连接管理器编辑器”** 对话框的 **“代理”** 选项卡配置 HTTP 连接管理器以使用代理服务器。 利用 HTTP 连接，包可以使用 HTTP 访问 Web 服务器以发送或接收文件。  
  
 若要了解有关 HTTP 连接管理器的详细信息，请参阅 [HTTP Connection Manager](../../integration-services/connection-manager/http-connection-manager.md)。 若要了解有关 HTTP 连接管理器的常见使用方案的详细信息，请参阅 [Web Service Task](../../integration-services/control-flow/web-service-task.md)。  
  
### <a name="options"></a>选项  
 **使用代理**  
 指定是否希望通过代理服务器连接 HTTP 连接管理器。  
  
 **代理 URL**  
 键入代理服务器的 URL。  
  
 **跳过本地代理**  
 指定是否希望 HTTP 连接管理器对于本地地址跳过代理服务器。  
  
 **使用凭据**  
 指定是否希望 HTTP 连接管理器对于代理服务器使用安全凭据。  
  
 **用户名**  
 如果 HTTP 连接管理器使用凭据，则必须指定用户名、密码和域。  
  
 **密码**  
 如果 HTTP 连接管理器使用凭据，则必须指定用户名、密码和域。  
  
 **Domain**  
 如果 HTTP 连接管理器使用凭据，则必须指定用户名、密码和域。  
  
 **代理跳过列表**  
 要跳过代理服务器的地址的列表。  
  
 **添加**  
 键入要跳过代理服务器的地址。  
  
 **删除**  
 选择某个地址，再单击“删除”即可将其删除。  
  
## <a name="see-also"></a>另请参阅  
 [Web 服务任务](../../integration-services/control-flow/web-service-task.md)   
 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
