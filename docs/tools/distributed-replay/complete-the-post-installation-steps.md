---
title: 完成安装后步骤
titleSuffix: SQL Server Distributed Replay
description: 安装 Distributed Replay 后，必须修改 Distributed Replay 控制器和客户端服务帐户。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 0a788a2a-9b4f-4bfc-b1b5-83eeb1ea9ab2
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 94cf69eb0a4159c4bfda5a0ef0b79a2b35bbfcb7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349546"
---
# <a name="complete-the-post-installation-steps"></a>完成安装后步骤

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

安装 Distributed Replay 后，您必须修改 Distributed Replay 控制器和客户端服务帐户。  
  
## <a name="to-complete-the-post-installation-steps"></a>完成安装后步骤  
  
1. **创建防火墙规则**：在控制器和客户端计算机上，必须允许相应服务的入站流量通过防火墙。 指定位于安装文件夹中的服务可执行文件的防火墙规则。  
  
    1. 对于控制器服务，请为位于安装文件夹中的 **DReplayController.exe** 创建规则。 例如，下面的命令会启用该规则，其中 `%InstallPath%` 是服务的安装文件夹：  
  
         `netsh advfirewall firewall add rule name="allow dreplay controller" dir=in program="%InstallPath%\DReplayController\DReplayController.exe" action=allow`  
  
    2. 对于客户端服务，请在每台客户端计算机上为位于安装文件夹中的 **DReplayClient.exe** 创建规则。 例如，下面的命令会启用该规则，其中 `%InstallPath%` 是服务的安装文件夹：  
  
         `netsh advfirewall firewall add rule name="allow dreplay client" dir=in program="%InstallPath%\DReplayClient\DReplayClient.exe" action=allow`  
  
2. **在目标服务器上授予每个客户端权限**：在客户端计算机上安装完客户端服务后，必须手动将客户端服务帐户添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标实例上的 sysadmin 角色中。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性

您必须具有管理权限才能安装任何 Distributed Replay 功能。 只有拥有 sysadmin 权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名才可以将客户端服务帐户添加到测试服务器的 sysadmin 服务器角色中。 有关分布式重播的安全注意事项的详细信息，请参阅 [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md)。