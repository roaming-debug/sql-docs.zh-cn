---
description: FTP 连接管理器
title: FTP 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftpconnectionmanager.f1
helpviewer_keywords:
- FTP connection manager
- connections [Integration Services], FTP
- connection managers [Integration Services], FTP
ms.assetid: c4f43455-29ca-44ba-ac7f-ea729b1daf93
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 854f975c64f5622c53d04c51651929d83745864a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91728038"
---
# <a name="ftp-connection-manager"></a>FTP 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  FTP 连接管理器使得包可以连接到文件传输协议 (FTP) 服务器。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含的 FTP 任务使用此连接管理器。  
  
 将 FTP 连接管理器添加到包时， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建可以在运行时决定 FTP 连接的连接管理器，设置该连接管理器的属性，并将该连接管理器添加到包中的 **Connections** 集合。  
  
 该连接管理器的 **ConnectionManagerType** 属性设置为 **FTP**。  
  
 可以按照下列方式配置 FTP 连接管理器：  
  
-   指定服务器名称和服务器端口。  
  
-   指定采用匿名访问，或者提供用户名和密码以进行基本身份验证。  
  
    > [!IMPORTANT]  
    >  FTP 连接管理器仅支持匿名身份验证和基本身份验证， 而不支持 Windows 身份验证。  
  
-   设置超时值、重试次数和可以一次复制的数据量。  
  
-   指示 FTP 连接管理器使用被动还是主动模式。  
  
 取决于与 FTP 连接管理器连接的 FTP 站点的配置，可能需要更改连接管理器的以下默认值：  
  
-   服务器端口设置为 21。 应当指定 FTP 站点侦听的端口。  
  
-   用户名设置为“anonymous”。 应当提供 FTP 站点需要的凭据。  
  
## <a name="activepassive-modes"></a>主动/被动模式  
 FTP 连接管理器可以使用主动模式或被动模式发送和接收文件。 在主动模式下，由服务器初始化数据连接；而在被动模式下，由客户端初始化数据连接。  
  
## <a name="configuration-of-the-ftp-connection-manager"></a>FTP 连接管理器的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的信息，请参阅 [TP 连接管理器编辑器]()。  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)项目。  
  
## <a name="ftp-connection-manager-editor"></a>FTP 连接管理器编辑器
  使用 **“FTP 连接管理器编辑器”** 对话框可以指定用于连接到 FTP 服务器的属性。  
  
> [!IMPORTANT]  
>  FTP 连接管理器仅支持匿名身份验证和基本身份验证， 而不支持 Windows 身份验证。  
  
 若要了解有关 FTP 连接管理器的详细信息，请参阅 [FTP Connection Manager](../../integration-services/connection-manager/ftp-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **服务器名称**  
 提供 FTP 服务器的名称。  
  
 **服务器端口**  
 指定用来连接的 FTP 服务器的端口号。 此属性的默认值为 **21**。  
  
 **用户名**  
 提供用于访问 FTP 服务器的用户名。 此属性的默认值为“匿名”。   
  
 **密码**  
 提供用于访问 FTP 服务器的密码。  
  
 **超时值(秒)**  
 指定任务超时之前经过的秒数。如果值为 **0** ，则表示不限制时间。 此属性的默认值为 **60**。  
  
 **使用被动模式**  
 指定是由服务器还是由客户端启动连接。 服务器使用主动模式启动连接，客户端使用被动模式启动连接。 此属性的默认值为“主动模式”。   
  
 **重试**  
 指定任务尝试连接的次数。 如果值为 **0** ，则表示不限制尝试次数。  
  
 **块区大小(KB)**  
 提供传输数据的块区大小 (KB)。  
  
 **测试连接**  
 在配置 FTP 连接管理器后，请通过单击“测试连接”确认该连接是否正常。  
  
## <a name="see-also"></a>另请参阅  
 [FTP 任务](../../integration-services/control-flow/ftp-task.md)   
 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
