---
description: Format Pager Addresses for Alerts
title: Format Pager Addresses for Alerts
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- pager addresses [SQL Server]
- SQL Server Agent, alerts
- formats [SQL Server], pager addresses
- pager notifications [SQL Server]
- addresses [SQL Server]
- alerts [SQL Server], pager addresses
ms.assetid: a9797d01-1050-442c-9038-ed4bfee1e76a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 32f019897d12ff2cc8620302a48734fc1fbde3a6
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251064"
---
# <a name="format-pager-addresses-for-alerts"></a>设置警报寻呼地址的格式
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中设置 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报的寻呼地址的格式。  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
默认情况下，只有 **sysadmin** 固定服务器角色的成员才能查看有关警报的信息。 其他用户必须被授予 **msdb** 数据库中的 **SQLAgentOperatorRole** 固定数据库角色的权限。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-format-pager-addresses"></a>设置寻呼地址格式  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要发送到寻呼程序的警报的服务器。  
  
2.  右键单击“SQL Server 代理”，然后选择“属性”  
  
3.  在 **“选择页”** 下，选择 **“警报系统”**。  
  
4.  在“寻呼电子邮件的地址格式”字段的“收件人行”和“抄送行”框中，输入寻呼地址的前缀或后缀。 发送通知时，将插入操作员的实际寻呼地址。  
  
5.  在 **“主题”** 框中，输入主题行的前缀或后缀。  
  
6.  选择“在通知页中包含电子邮件的正文部分”复选框，以便在寻呼消息中包含完整电子邮件（与仅包含主题行相对）。  
  
7.  完成后，单击 **“确定”** 。  
