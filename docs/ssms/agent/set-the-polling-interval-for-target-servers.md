---
description: Set the Polling Interval for Target Servers
title: Set the Polling Interval for Target Servers
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- interval for polling [SQL Server]
- target servers [SQL Server], polling interval
- polling interval [SQL Server]
ms.assetid: 4ffbbefa-77fb-442e-a77c-cb8c6cab9f3c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: b62b0e833af56de2f7c575a08a7093615c81265e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472248"
---
# <a name="set-the-polling-interval-for-target-servers"></a>Set the Polling Interval for Target Servers
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何设置 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理刷新从主服务器到目标服务器的信息的频率。 作业是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理执行的一系列指定操作。 多服务器作业是主服务器在一台或多台目标服务器上运行的作业。  
  
-   **开始之前：** [安全性](#Security)  
  
-   **若要设置目标服务器的轮询间隔，请使用** [SQL Server Management Studio](#SSMS)、 [Transact-SQL](#TSQL)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
每个目标服务器一次只能运行一个相同作业的实例。 每台目标服务器会定期轮询主服务器，下载分配给目标服务器的任何新作业的一个副本，然后断开连接。 目标服务器在本地运行作业，然后重新连接到主服务器以上载作业结果状态。  
  
> [!NOTE]  
> 如果在目标服务器试图上载作业状态时无法访问主服务器，则作业将处于假脱机状态，直到可以访问主服务器。  
  
### <a name="security"></a><a name="Security"></a>安全性  
有关详细信息，请参阅 [Implement SQL Server Agent Security](../../ssms/agent/implement-sql-server-agent-security.md) 和 [Choose the Right SQL Server Agent Service Account for Multiserver Environments](../../ssms/agent/choose-the-right-sql-server-agent-service-account-for-multiserver-environments.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
**设置目标服务器的轮询间隔**  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  右键单击“SQL Server 代理”，指向“多服务器管理”，再单击“管理目标服务器”。  
  
3.  在 **“目标服务器状态”** 选项卡上，单击 **“发布指令”**。  
  
4.  在 **“指令类型”** 列表中，选择 **“设置轮询间隔”**。  
  
5.  在 **“轮询间隔”** 框中，输入介于 10 到 28,800 之间的秒数，目标服务器必须在经过该秒数后才会轮询主服务器。  
  
6.  在 **“收件人”** 下，执行以下操作之一：  
  
    1.  如果所有目标服务器共享同一轮询间隔，则单击 **“所有目标服务器”** 。  
  
    2.  如果并非所有目标服务器都共享同一轮询间隔，则单击 **“以下目标服务器”** ，然后选择将使用此轮询间隔的每台目标服务器。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
**设置目标服务器的轮询间隔**  
  
1.  在对象资源管理器中，连接到数据库引擎实例，然后展开该实例。  
  
2.  在工具栏上，单击 **“新建查询”** 。  
  
3.  在查询窗口中，使用 [sp_post_msx_operation (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-post-msx-operation-transact-sql.md) 系统存储过程设置目标服务器的轮询间隔。  
  
## <a name="see-also"></a>另请参阅  
[sysdownloadlist](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)  
