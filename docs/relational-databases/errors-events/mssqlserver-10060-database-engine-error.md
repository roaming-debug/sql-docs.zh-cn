---
title: MSSQLSERVER_10060 | Microsoft Docs
description: SQL Server 客户端无法连接到服务器。 查看错误 10060 的说明和可能的解决方法。
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
f1_keywords:
- "10060"
helpviewer_keywords:
- 10060 (Database Engine error)
ms.assetid: 28c21277-cad8-406c-a955-07933a56982a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cb6a706a664adc8dbc86760b6d5e7b519b8a192a
ms.sourcegitcommit: 36fe62a3ccf34979bfde3e192cfa778505add465
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94521204"
---
# <a name="mssqlserver_10060"></a>MSSQLSERVER_10060
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|10060|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称||  
|消息正文|在建立与服务器的连接时出错。  在连接到 SQL Server 时，在默认的设置下 SQL Server 不允许远程连接可能会导致此失败。 （提供程序：TCP 提供程序，错误:0 - 由于被连接方在一段时间后未正确响应，或者连接的主机无法响应，连接尝试失败。)(Microsoft SQL Server，错误:10060)|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端无法连接到服务器。 发生此错误的原因可能是服务器上的防火墙拒绝该连接或服务器未配置为接受远程连接。  
  
## <a name="user-action"></a>用户操作  
若要解决此错误，请尝试执行下列操作之一：  
  
-   确保计算机上的防火墙已配置，以便此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可以接受连接。  
  
-   使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器工具使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 接受远程连接。  
  
## <a name="see-also"></a>另请参阅  
[为数据库引擎访问配置 Windows 防火墙](~/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)  
[配置客户端协议](~/database-engine/configure-windows/configure-client-protocols.md)  
[网络协议和网络库](~/sql-server/install/network-protocols-and-network-libraries.md)  
[客户端网络配置](~/database-engine/configure-windows/client-network-configuration.md)  
[配置客户端协议](~/database-engine/configure-windows/configure-client-protocols.md)  
[启用或禁用服务器网络协议](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
