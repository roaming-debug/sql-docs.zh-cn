---
description: 处理多个作业步骤
title: 处理多个作业步骤
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- ordering job steps [SQL Server]
- multiple job steps
- SQL Server Agent jobs, job steps
- control of flow for jobs [SQL Server]
ms.assetid: 7aba19ff-72b3-45f6-8e54-23f4988d63a8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1bb3a13c67e00fd9da9b9ca48bab95b21e46769a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474448"
---
# <a name="handle-multiple-job-steps"></a>处理多个作业步骤
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

如果作业有多个作业步骤，必须指定这些作业步骤的运行顺序。 我们称之为“流控制”。 您可以随时添加新的作业步骤并重排作业步骤流，更改在下次运行作业时生效。 下图说明了一个数据库备份作业的流控制。  
  
![SQL Server 代理作业步骤的流控制](../../ssms/agent/media/dbflow01.gif "SQL Server 代理作业步骤的流控制")  
  
第一步是备份数据库。 如果这一步失败， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将把失败报告给定义为接收通知的操作员。 如果备份数据库步骤成功，则作业将继续进行下一步，即“清理”客户端数据。 如果这一步失败， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将向前跳至还原数据库。 如果“清理”客户端数据成功，则作业将继续进行下一步，即更新统计信息，如此继续，直至最后报告成功或报告失败。  
  
为每个作业步骤的成功和失败都定义一个流控制操作。 必须指定作业步骤成功执行的操作和作业步骤失败时执行的操作。 还可以定义对失败的作业步骤进行重试的次数以及重试时间间隔。  
  
> [!NOTE]  
> 当使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理图形用户界面 (GUI) 并从多步骤作业中删除一个或多个步骤时，GUI 将删除所有作业步骤，然后使用正确的成功时或失败时引用重新添加剩余的步骤。 例如，假设您有一个包含五个步骤的作业，第一步配置为在顺利完成时即跳到第 4 步。 如果删除第 3 步，则 GUI 将删除此作业的所有步骤，然后使用正确的引用添加剩余的四个步骤（1、2、4 和 5）。 在此示例中，第 1 步中的引用将重新配置为在第 1 步顺利完成时即跳到第 3 步。  
  
作业步骤必须自包含。 也就是说，作业不能在作业步骤之间传递布尔值、数据或数值。 但可以使用永久表或全局临时表在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤之间传递值。 可以使用文件在运行可执行程序的作业步骤之间传递值。 例如，一个作业步骤运行的可执行程序向一个文件写入数据，后续作业步骤运行的可执行程序读取该文件。  
  
> [!NOTE]  
> 如果创建循环作业步骤（作业步骤 1 后面跟着作业步骤 2，然后作业步骤 2 又返回到作业步骤 1），则在使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]创建作业时会出现警告消息。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在作业历史记录中记录作业和作业步骤信息。  
  
## <a name="see-also"></a>另请参阅  
[sp_add_job](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
[sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
[sysjobs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
[sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
[执行作业](../../ssms/agent/implement-jobs.md)  
[管理作业步骤](../../ssms/agent/manage-job-steps.md)  
