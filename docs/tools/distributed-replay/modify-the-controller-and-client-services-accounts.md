---
title: 修改控制器和客户端服务帐户
titleSuffix: SQL Server Distributed Replay
description: 了解如何修改 Distributed Replay 控制器和客户端服务帐户，然后重新应用访问控制列表。
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 44a73ddb-18ad-415c-bfbe-126ab2e3290b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 22d126dae4356bd6830c801369375b8705e37b37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349521"
---
# <a name="modify-the-controller-and-client-services-accounts"></a>修改控制器和客户端服务帐户

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在本主题中，您将了解如何修改 Distributed Replay 控制器和客户端服务帐户，然后重新应用访问控制列表 (ACL)。  
  
### <a name="to-start-or-stop-the-distributed-replay-services-using-computer-management"></a>使用“计算机管理”启动或停止 Distributed Replay 服务  
  
1.  在安装有 Distributed Replay 服务的计算机上，在命令提示符下键入 **dcomcnfg**。  
  
2.  双击“服务”，向下滚动并右键单击“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Distributed Replay \<service name>”，然后单击“开始”或“停止”   。  
  
### <a name="to-modify-the-distributed-replay-controller-service"></a>修改 Distributed Replay 控制器服务  
  
1.  在控制器计算机上，停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器服务。  
  
2.  在“服务”  下，右键单击  “[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播控制器”，然后选择“属性”  。  
  
3.  在 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器属性”** 窗口中的 **“登录”** 选项卡上，选择 **“本帐户”** ，键入或单击 **“浏览”** 以输入新的登录帐户，然后单击 **“确定”** 。  
  
     **重要说明**：配置 Distributed Replay 控制器时，可以指定一个或多个用于运行 Distributed Replay 客户端服务的用户帐户。 下面是支持的帐户的列表：  
  
    -   域用户帐户  
  
    -   用户创建的本地用户帐户  
  
    -   管理员  
  
    -   虚拟帐户和 MSA（托管服务帐户）  
  
    -   Network Services、Local Services 和 System  
  
     不接受组帐户（本地或域）和其他内置帐户（如 Everyone）。  
  
4.  启动 Distributed Replay 控制器服务。  
  
### <a name="to-modify-the-distributed-replay-client-service"></a>修改 Distributed Replay 客户端服务  
  
1.  在修改 Distributed Replay 客户端服务之前，请确保在安装期间指定了您要更改的客户端服务帐户（在控制器计算机上的 CTLRUSERS 参数中）。 如果在安装期间未指定要更改的客户端服务帐户，必须先执行以下步骤：  
  
    1.  停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器服务。  
  
    2.  在安装有控制器服务的控制器计算机上，在命令提示符下键入 **dcomcnfg**。  
  
    3.  在“组件服务”  窗口中，导航到“控制台根节点 -> 组件服务 -> 计算机 -> 我的电脑 -> DCOM Config ->DReplayController”  。  
  
    4.  右键单击“DReplayController”  ，然后单击“属性”  。  
  
    5.  在 **“DReplayController 属性”** 窗口中的 **“安全性”** 选项卡上，单击 **“启动和激活权限”** 部分的 **“编辑”** 。  
  
    6.  为新的客户端服务登录帐户授予 **“本地和远程激活”** 权限，然后单击 **“确定”** 。  
  
    7.  单击 **“访问权限”** 部分的 **“编辑”** ，并为新的客户端服务登录帐户授予 **“本地和远程访问”** 权限，然后单击 **“确定”** 。  
  
    8.  单击 **“确定”** 以关闭 **“DReplayController 属性”** 窗口。  
  
    9. 在控制器计算机上，将更改后的客户端服务登录帐户添加到 **“分布式 COM 用户”** 组中。  
  
    10. 启动 SQL Server 分布式重播控制器服务。  
  
2.  停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端服务。  
  
3.  在 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端属性”** 窗口中的 **“登录”** 选项卡上，选择 **“本帐户”** ，键入或单击 **“浏览”** 以输入新的登录帐户，然后单击 **“确定”** 。  
  
4.  启动 Distributed Replay 客户端服务。  
  
  
