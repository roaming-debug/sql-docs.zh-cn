---
description: 指定事件转发服务器
title: 指定事件转发服务器
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- event forwarding servers [SQL Server]
- events [SQL Server], forwarding
- alerts [SQL Server], forwarded events
ms.assetid: 81dfcbe4-3000-4e77-99de-bf85fef63a12
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: ecb37153660b08e03170b2d0e8655eb6db20b42c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236373"
---
# <a name="designate-an-events-forwarding-server"></a>指定事件转发服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍了如何通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中为 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 指定接收转发事件的服务器。 请注意，事件转发适用于在服务器之间转发的事件，而不适用于在单个计算机上承载的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间转发的事件。 此外，还请注意，为了接收转发的事件，警报管理服务器必须是 SQL Server 的默认实例。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-designate-an-events-forwarding-server"></a>指定事件转发服务器  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开要将其事件转发到其他服务器的服务器。  
  
2.  右键单击“SQL Server 代理”，然后选择“属性”。  
  
3.  在“SQL Server 代理属性” –_server_name_ 对话框的“选择页”下，选择“高级”。  
  
4.  在 **“SQL Server 事件转发”** 下，选中 **“将事件转发到其他服务器”** 复选框。  
  
5.  在 **“服务器”** 列表中，选择一台服务器，然后在 **“事件”** 选择下列操作之一：  
  
    -   选择 **“未处理的事件”** 以仅转发本地警报未处理的事件。  
  
    -   选择 **“所有事件”** 以转发所有事件，无论本地警报是否已对事件进行处理。  
  
6.  在 **“如果事件的严重性不低于”** 列表中，单击将事件转发到所选服务器时的严重级别。  
  
7.  完成后，单击 **“确定”** 。  
