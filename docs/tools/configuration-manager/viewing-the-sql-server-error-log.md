---
title: 查看 SQL Server 错误日志
description: 通过查看当前错误日志或以前日志的备份来帮助检测 SQL Server 中的问题，以检查进程是否成功完成。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- cycling SQL Server error log
- viewing SQL Server error log
- errors [SQL Server], logs
- SQL Server error log
- displaying SQL Server error log
- logs [SQL Server], SQL Server error logs
ms.assetid: 6908c21a-65e3-458f-a272-fee256d86448
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 149ece24923ad8f4b46f9d24f9e6568d05960cec
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463718"
---
# <a name="viewing-the-sql-server-error-log"></a>查看 SQL Server 错误日志
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志可以确保进程（例如，备份和还原操作、批处理命令或其他脚本和进程）成功完成。 此功能可用于帮助检测任何当前或潜在的问题领域，包括自动恢复消息（尤其是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例已停止并重新启动时）、内核消息或其他服务器级错误消息。  
  
 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或任何文本编辑器可以查看 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 错误日志。 有关如何查看错误日志的详细信息，请参阅 [Open Log File Viewer](../../relational-databases/logs/open-log-file-viewer.md)。 默认情况下，错误日志位于 `Program Files\Microsoft SQL Server\MSSQL.`*n*`\MSSQL\LOG\ERRORLOG` 和 `ERRORLOG.`*n* 文件中。  
  
 每当启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，将创建新的错误日志，虽然 [sp_cycle_errorlog](../../relational-databases/system-stored-procedures/sp-cycle-errorlog-transact-sql.md) 系统存储过程可用于循环使用错误日志文件，而不必重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 通常， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 保留前六个日志的备份，并指定最近日志备份的扩展名为 .1、下一个最近日志备份的扩展名为 .2，依次类推。 当前的错误日志没有扩展名。  
  
 请注意，您还可以查看脱机或无法启动的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志。 有关详细信息，请参阅 [查看脱机日志文件](../../relational-databases/logs/view-offline-log-files.md)。  
  
  
