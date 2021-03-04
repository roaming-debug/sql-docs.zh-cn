---
title: 如何使用 SQLPS 启用 TCP 协议
description: 了解如何使用 SQLPS 启用 TCP 协议
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 08/06/2020
ms.openlocfilehash: 6aad2599996294679d5bd20d3a0e1a71a099b00b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837954"
---
# <a name="how-to-enable-the-tcp-protocol"></a>如何启用 TCP 协议

## <a name="how-to-enable-the-tcp-protocol-when-connected-to-the-console-with-sqlps"></a>如何在连接到控制台时使用 SQLPS 启用 TCP 协议。

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

1. 打开一个命令提示符窗口，并键入：

    ```console
    C:\> SQLPS.EXE
    ```

    > [!TIP]
    > 如果找不到 SQLPS，可能需要打开新的命令提示符，或直接注销并重新登录。

2. 在 PowerShell 命令提示符处键入：

    ```powershell
    # Instantiate a ManagedComputer object which exposes primitives to control the
    # installation of SQL Server on this machine.

    $wmi = New-Object 'Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer' localhost

    # Enable the TCP protocol on the default instance. If the instance is named, 
    # replace MSSQLSERVER with the instance name in the following line.

    $tcp = $wmi.ServerInstances['MSSQLSERVER'].ServerProtocols['Tcp']
    $tcp.IsEnabled = $true  
    $tcp.Alter()  

    # You need to restart SQL Server for the change to persist
    # -Force takes care of any dependent services, like SQL Agent.
    # Note: if the instance is named, replace MSSQLSERVER with MSSQL$ followed by
    # the name of the instance (e.g. MSSQL$MYINSTANCE)

    Restart-Service -Name MSSQLSERVER -Force
    ```

## <a name="how-to-enable-the-tcp-protocol-when-connected-to-the-console-not-using-sqlps"></a>如何在连接到控制台时不使用 SQLPS 启用 TCP 协议。

1. 打开一个命令提示符窗口，并键入：

    ```console
    C:\> PowerShell.exe
    ```

2. 在 PowerShell 命令提示符处键入：

    ```powershell
    # Get access to SqlWmiManagement DLL on the machine with SQL
    # we are on, which is where SQL Server was installed.
    # Note: this is installed in the GAC by SQL Server Setup.

    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SqlWmiManagement')

    # Instantiate a ManagedComputer object which exposes primitives to control the
    # installation of SQL Server on this machine.

    $wmi = New-Object 'Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer' localhost

    # Enable the TCP protocol on the default instance. If the instance is named, 
    # replace MSSQLSERVER with the instance name in the following line.

    $tcp = $wmi.ServerInstances['MSSQLSERVER'].ServerProtocols['Tcp']
    $tcp.IsEnabled = $true  
    $tcp.Alter()  

    # You need to restart SQL Server for the change to persist
    # -Force takes care of any dependent services, like SQL Agent.
    # Note: if the instance is named, replace MSSQLSERVER with MSSQL$ followed by
    # the name of the instance (e.g. MSSQL$MYINSTANCE)

    Restart-Service -Name MSSQLSERVER -Force
    ```

## <a name="next-steps"></a>后续步骤

- [安装 SQL Server PowerShell 模块](download-sql-server-ps-module.md)
- [SQL Server PowerShell](sql-server-powershell.md)
- [启用或禁用服务器网络协议](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
- [在 Server Core 安装上配置 SQL Server](../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md)
