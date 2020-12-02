---
description: 日志传送事务日志备份设置
title: 日志传送事务日志备份设置 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.logshipping.settings.tlogback.f1
ms.assetid: 9a6e6c16-7f71-412b-bba6-7bffac001277
author: stevestein
ms.author: sstein
ms.openlocfilehash: 7d4aca0ffc4cf4521123c03b71c7e2e4aae72038
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88465429"
---
# <a name="log-shipping-transaction-log-backup-settings"></a>日志传送事务日志备份设置
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此对话框可以配置和修改日志传送配置的事务日志备份设置。  
  
 有关日志传送概念的说明，请参阅 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)。  
  
## <a name="options"></a>选项  
 **备份文件夹的网络路径**  
 在此框中键入备份文件夹的网络共享位置。 用于保存事务日志备份的本地文件夹必须处于共享状态，以便日志传送复制作业可以将这些文件复制到辅助服务器。 必须向代理帐户授予此网络共享的读取权限，复制作业将在辅助服务器实例上的该帐户下运行。 默认情况下，这是辅助服务器实例的 SQLServerAgent 服务帐户，但是管理员可以为该作业选择另一个代理帐户。  
  
 **如果备份文件夹位于主服务器上，则键入该文件夹的本地路径**  
 如果备份文件夹位于主服务器上，则键入备份文件夹的本地驱动器号和路径。 如果备份文件夹不在主服务器上，则可以保留此选项为空白。  
  
 如果您在此处指定本地路径，则 BACKUP 命令将使用此路径来创建事务日志备份；否则，如果未指定本地路径，则 BACKUP 命令将使用在 **“备份文件夹的网络路径”** 框中指定的网络路径。  
  
> [!NOTE]  
>  如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户在主服务器上的本地系统帐户下运行，则必须在主服务器上创建备份文件夹，并在此处指定该文件夹的本地路径。 主服务器实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须对此文件夹具有读写权限。  
  
 **删除文件，如果其保留时间超过**  
 指定事务日志备份在删除前在备份目录中保留的时间。  
  
 **在以下时间内没有执行备份时报警**  
 指定在引发警报以报告未执行事务日志备份之前，希望日志传送等待的时间。  
  
 **作业名称**  
 显示 SQL Server 代理作业的名称，该作业用于为日志传送创建事务日志备份。 首次创建作业时，可以通过在该框中键入内容来修改名称。  
  
 **计划**  
 显示用来备份主数据库的事务日志的当前计划。 在创建备份作业之前，可以通过单击 **“计划...”** 来修改此计划。在创建备份作业之后，可以通过单击 **“编辑作业...”** 来修改此计划。  
  
### <a name="backup-job"></a>备份作业  
 **“计划...”**  
 修改在创建 SQL Server 代理作业时创建的计划。  
  
 **“编辑作业...”**  
 为在主数据库中执行事务日志备份的作业修改 SQL Server 代理作业参数。  
  
 **禁用此作业**  
 禁止 SQL Server 代理作业创建事务日志备份。  
  
### <a name="compression"></a>压缩  
 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] （或更高版本）支持 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。  
  
 **设置备份压缩**  
 在 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] （或更高版本）中，为此日志传送配置的日志备份选择以下其中一个备份压缩值。  
  
|||  
|-|-|  
|**使用默认服务器设置**|单击此选项可使用服务器级别默认值。<br /><br /> 此默认值可通过 **backup compression default** 服务器配置选项进行设置。 有关如何查看此选项当前设置的信息，请参阅 [查看或配置 backup compression default 服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。|  
|**压缩备份**|单击此选项可压缩备份，而不考虑服务器级别默认值。<br /><br /> **\*\* 重要提示 \*\*** 默认情况下，压缩会大大提高 CPU 使用率，但压缩进程占用的额外 CPU 可能会对并发操作造成不利影响。 因此，你可能需要在会话中创建低优先级的压缩备份，其 CPU 使用率受 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)限制。 有关详细信息，请参阅本主题后面的 [使用资源调控器限制备份压缩的 CPU 使用量 (Transact-SQL)](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)限制。|  
|**不压缩备份**|单击此选项可创建未压缩的备份，而不考虑服务器级别默认值。|  
  
## <a name="see-also"></a>另请参阅  
 [配置帐户以创建和管理 SQL Server 代理作业](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)   
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
  
