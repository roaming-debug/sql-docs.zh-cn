---
title: 可用性组的资源 DLL 运行状况诊断日志
description: 介绍 SQL Server 资源 DLL 如何监视 AlwaysOn 可用性组的运行状况。
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: c1862d8a-5f82-4647-a280-3e588b82a6dc
author: rothja
ms.author: jroth
ms.openlocfilehash: 72a023d4627d348421b392e8f787914edc986505
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643705"
---
# <a name="sql-server-resource-dll-health-diagnostic-logs-for-availability-groups"></a>可用性组的 SQL Server 资源 DLL 运行状况诊断日志
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  为了监视主要可用性副本的运行状况，Windows Server 故障转移群集 (WSFC) 群集运行的 SQL Server 资源 DLL 在名为 [sp_server_diagnostics](~/relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 的 SQL Server 实例中使用了一个存储过程。  
  
 SQL Server 资源 DLL 可维持与 SQL Server 实例的专用开放连接，通过该连接，SQL Server 实例可定期向 SQL Server 资源 DLL 发送详细的运行状况诊断。 群集结合使用运行状况诊断和在群集的可用性组中配置的故障转移策略（FailoverConditionLevel 属性）来确定是否重启或故障转移可用性组资源。 此存储过程是 SQL Server 2012 及更高版本实例对 WSFC 群集的“检测信号”，该检测信号比在 SQL Server 2008 R2 或更低版本（由查询 `SELECT @@SERVERNAME` 执行与实例的定期连接）中更精细，更可靠。 然后可以通过设置可用性组 FailureConditonLevel 属性来控制触发故障转移的条件。  
  
 **使用 SQL Server 故障转移群集诊断日志**
 
 从 sp_server_diagnostics are automatically 收到的所有运行状况诊断 SQL Server 资源 DLL 将自动保存在 SQL Server 实例的默认日志目录中 (%PROGRAMFILES%\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Log)。 这些日志称为 SQLDIAG 日志，以 XEL（扩展事件）文件格式保存。 SQL Server 日志目录下的这些文件采用以下格式：\<HOSTNAME>_\<INSTANCENAME>_SQLDIAG_X_XXXXXXXXX.xel。 通过查看 SQLDIAG 日志，也许能够确定可用性组资源故障或故障转移事件的根本原因。  
  
 要查看 SQLDIAG 日志，请将 .xel 文件拖到 SQL Server Management Studio。  
  
  
