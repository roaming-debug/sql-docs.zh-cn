---
description: “执行 T-SQL 语句”任务（维护计划）
title: “执行 T-SQL 语句”任务（维护计划）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.tsql.f1
helpviewer_keywords:
- Execute T-SQL Statement Task dialog box
ms.assetid: fef3e259-0c47-4f6e-ade0-aee95e3d6c1a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: d90a9aaa1b038d4d890c66ceb5f5e7b9c4befc81
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130235"
---
# <a name="execute-t-sql-statement-task-maintenance-plan"></a>“执行 T-SQL 语句”任务（维护计划）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用“执行 T-SQL 语句任务”对话框，可以通过向此维护计划添加所选择的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句来自定义维护计划。  
  
## <a name="options"></a>选项  
 **Connection**  
 选择执行此任务时使用的服务器连接。  
  
 **新建**  
 创建一个新的服务器连接，在执行此任务时使用。 下面对 **“新建连接”** 对话框进行了介绍。  
  
 **执行超时值**  
 超时（终止任务）前等待任务完成的时间（秒）。  
  
 **T-SQL 语句**  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 要执行的语句。  
  
 **查看 T-SQL**  
 根据所选选项，查看针对此任务的服务器执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
> [!NOTE]  
>  当受影响的对象很多时，可能需要相当长的时间才可显示。  
  
## <a name="new-connection-dialog-box"></a>“新建连接”对话框  
 **连接名称**  
 输入新连接的名称。  
  
 **选择或输入服务器名称**  
 选择执行此任务时所要连接的服务器。  
  
 **“刷新”**  
 刷新可用服务器的列表。  
  
 **输入登录服务器所需的信息**  
 指定如何对服务器进行身份验证。  
  
 **使用 Windows 集成安全性**  
 使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证连接到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例。  
  
 **使用特定用户名和密码**  
 使用 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 身份验证连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 此选项不可用。  
  
 **用户名**  
 提供一个在进行身份验证时要使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 此选项不可用。  
  
 **密码**  
 提供一个在进行身份验证时要使用的密码。 此选项不可用。  
  
  
