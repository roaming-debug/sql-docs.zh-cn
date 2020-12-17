---
title: SQL Native Client 11.0 配置
description: 了解在 Microsoft SQL Server 配置管理器的“SQL Server Native Client 配置”对话框中所配置的设置。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- client configuration [SQL Server], SQL Server Native Client
ms.assetid: e73143e9-5e7b-4d0a-8827-ab900efdcb35
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: aa0eac24445ec24c4b86c54bad6fc1c94c7072d4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463758"
---
# <a name="sql-native-client-110-configuration"></a>SQL Native Client 11.0 配置
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  本节包含了按 F1 后看到的有关 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器中的“SQL Server Native Client 配置”对话框的帮助主题。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 是客户端计算机用于连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的网络库，与 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一起启动。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 配置中配置的设置将在运行客户端程序的计算机上使用。 在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的计算机上配置这些设置时，它们仅影响那些运行在服务器上的客户端程序。  
  
 这些设置不会影响连接到早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的客户端，除非它们使用与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]一起启动的客户端工具，例如 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [SQL Server Native Client 配置属性（“标志”选项卡）](../../tools/configuration-manager/sql-server-native-client-configuration-properties-flags-tab.md)  
  
-   [客户端协议（SQL Server 配置管理器）](../../tools/configuration-manager/client-protocols-sql-server-configuration-manager.md)  
  
    -   [客户端协议属性（“顺序”选项卡）](../../tools/configuration-manager/client-protocols-properties-order-tab.md)  
  
    -   [客户端协议 - Shared Memory 属性（“协议”选项卡）](../../tools/configuration-manager/client-protocols-shared-memory-properties-protocol-tab.md)  
  
    -   [客户端协议 - TCP/IP 属性（“协议”选项卡）](../../tools/configuration-manager/client-protocols-tcp-ip-properties-protocol-tab.md)  
  
    -   [客户端协议 - Named Pipes 属性（“协议”选项卡）](../../tools/configuration-manager/client-protocols-named-pipes-properties-protocol-tab.md)  
  
-   [别名（SQL Server 配置管理器）](../../tools/configuration-manager/aliases-sql-server-configuration-manager.md)  
  
    -   [新建别名（“别名”选项卡）](../../tools/configuration-manager/new-alias-alias-tab.md)  
  
    -   [<别名>属性（“别名”选项卡）](../../tools/configuration-manager/alias-properties-alias-tab.md)  
  
    -   [使用 Shared Memory 协议创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
    -   [使用 TCP IP 创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
    -   [使用命名管道创建有效的连接字符串](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
