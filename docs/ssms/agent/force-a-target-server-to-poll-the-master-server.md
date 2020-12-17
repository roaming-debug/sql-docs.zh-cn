---
description: Force a Target Server to Poll the Master Server
title: Force a Target Server to Poll the Master Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- forcing master server polling
- polling master servers [SQL Server]
- master servers [SQL Server], polling
- target servers [SQL Server], polling the master server
ms.assetid: f1189a47-5ac3-45e2-9c5f-847810672279
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4aaac1b02996efdf950e4a0fecac054b25a9055c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474458"
---
# <a name="force-a-target-server-to-poll-the-master-server"></a>强制目标服务器轮询主服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何强制目标服务器轮询主服务器。 目标服务器必须是主服务器上的已注册服务器。  
  
作业是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理执行的一系列指定操作。 多服务器作业是主服务器在一台或多台目标服务器上运行的作业。 每个目标服务器一次只能运行一个相同作业的实例。 每台目标服务器会定期轮询主服务器，下载分配给目标服务器的任何新作业的一个副本，然后断开连接。 目标服务器在本地运行作业，然后重新连接到主服务器以上载作业结果状态。  
  
> [!NOTE]  
> 如果在目标服务器试图上载作业状态时无法访问主服务器，则作业将处于假脱机状态，直到可以访问主服务器。  
  
-   **开始之前：** [限制和局限](#Restrictions)、[安全性](#Security)  
  
-   **若要强制目标服务器轮询主服务器，使用：** [SQL Server Management Studio](#SSMS)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
目标服务器必须是主服务器上的已注册服务器。 您必须从主服务器中运行本主题中给出的说明。  
  
### <a name="security"></a><a name="Security"></a>安全性  
有关详细信息，请参阅 [Implement SQL Server Agent Security](../../ssms/agent/implement-sql-server-agent-security.md) 和 [Choose the Right SQL Server Agent Service Account for Multiserver Environments](../../ssms/agent/choose-the-right-sql-server-agent-service-account-for-multiserver-environments.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
**强制目标服务器轮询主服务器**  
  
1.  在 **对象资源管理器** 中，展开主服务器。  
  
2.  右键单击“SQL Server 代理”，指向“多服务器管理”，再单击“管理目标服务器”。  
  
3.  单击目标服务器，再单击 **“强制轮询”**。  
