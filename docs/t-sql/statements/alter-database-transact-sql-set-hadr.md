---
description: ALTER DATABASE SET HADR (Transact-SQL)
title: ALTER DATABASE SET HADR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET HADR
- SET_HADR_TSQL
- HADR_TSQL
- HADR
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE statement, AlwaysOn Availability Group
- ALTER DATABASE statement, SET HADR options
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
- Availability Groups [SQL Server], databases
ms.assetid: 20e6e803-d6d5-48d5-b626-d1e0a73d174c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 513b7850956ad0bd639edec3c8ec3b4aa876a03a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124277"
---
# <a name="alter-database-transact-sql-set-hadr"></a>ALTER DATABASE (Transact-SQL) SET HADR 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题包含与在辅助数据库中设置 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 选项有关的 ALTER DATABASE 语法。 每个 ALTER DATABASE 语句只允许使用一个 SET HADR 选项。 只有辅助副本支持这些选项。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER DATABASE database_name  
   SET HADR   
   {  
        { AVAILABILITY GROUP = group_name | OFF }  
   | { SUSPEND | RESUME }  
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *database_name*  
 要修改的辅助数据库的名称。  
  
 SET HADR  
 在指定的数据库上执行指定的 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 命令。  
  
 { AVAILABILITY GROUP =group_name | OFF }  
 在指定的可用性组中加入或删除可用性数据库，如下所示：  
  
 group_name  
 在您针对由 group_name 指定的可用性组执行命令的服务器实例所承载的辅助副本上，加入指定的数据库。  
  
 此操作的前提条件如下所示：  
  
-   数据库必须已经添加到主副本上的可用性组中。  
  
-   主副本必须处于活动状态。 有关如何解决非活动主副本问题的信息，请参阅[解决 Always On 可用性组配置问题 (SQL Server)](https://go.microsoft.com/fwlink/?LinkId=225834)。  
  
-   主副本必须处于联机状态，辅助副本必须连接到主副本。  
  
-   必须已使用 WITH NORECOVERY 从主数据库的最新数据库和日志备份中还原了辅助数据库，并以足够新的日志备份结尾，以便允许辅助数据库赶上主数据库。  
  
    > [!NOTE]  
    >  若要将数据库添加到可用性组，请连接到承载主副本的服务器实例，并使用 [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)group_name ADD DATABASE database_name 语句。  
  
 有关详细信息，请参阅 [将辅助数据库联接到可用性组 (SQL Server)](../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)。  
  
 OFF  
 从可用性组中删除指定的辅助数据库。  
  
 如果辅助数据库已远远落后于主数据库，并且您不想等待辅助数据库赶上主数据库，则删除辅助数据库可能有用。 在删除辅助数据库之后，可以通过用最新的日志备份（使用 RESTORE …）还原备份序列来更新此数据库WITH NORECOVERY).  
  
> [!IMPORTANT]  
>  若要从可用性组中完全删除可用性数据库，请连接到承载主副本的服务器实例，并使用 [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)group_name REMOVE DATABASE availability_database_name语句。 有关详细信息，请参阅[从可用性组中删除主数据库 (SQL Server)](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)。  
  
 SUSPEND  
 挂起辅助数据库的数据移动。 SUSPEND 命令只要被承载目标数据库的副本接受后就返回，但是实际上挂起数据库以异步方式发生。  
  
 影响的范围取决于您执行 ALTER DATABASE 语句的位置：  
  
-   如果您在辅助副本上挂起辅助数据库，则只挂起本地辅助数据库。 可读辅助副本上的现有连接会保持可用状态。 可读辅助副本上挂起数据库的新连接在数据移动恢复之前一直处于禁用状态。  
  
-   如果您在主副本上挂起数据库，则会在每个辅助副本上挂起将数据移到对应辅助数据库的过程。 可读辅助副本上的现有连接会保持可用状态，并且新的读意向连接将不会连接到可读辅助副本。  
  
-   如果数据移动因强制性手动故障转移而挂起，则与新的辅助副本的连接会在数据移动挂起之后进入禁用状态。  
  
 当挂起辅助副本上的数据库时，数据库和副本都变为未同步状态，并被标记为“未同步”。  
  
> [!IMPORTANT]  
>  当挂起辅助数据库时，对应主数据库的发送队列将累积未发送的事务日志记录。 与辅助副本的连接将返回在数据移动挂起时可用的数据。  
  
> [!NOTE]  
>  挂起和恢复 AlwaysOn 辅助数据库并不直接影响主数据库的可用性，但挂起辅助数据库可能会在恢复已挂起的辅助数据库之前影响主数据库的冗余和故障转移功能。 这与数据库镜像相反，在数据库镜像中，在恢复镜像前镜像状态在镜像数据库和主数据库上都挂起。 挂起 AlwaysOn 主数据库将挂起所有相应辅助数据库上的数据移动，并且在恢复主数据库前针对该数据库的冗余和故障转移功能将停止。  
  
 有关详细信息，请参阅本主题后面的[挂起可用性数据库 (SQL Server)](../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)。  
  
 RESUME  
 在指定的辅助数据库上恢复挂起的数据移动。 RESUME 命令只要被承载目标数据库的副本接受后就返回，但是实际上继续数据库以异步方式发生。  
  
 影响的范围取决于您执行 ALTER DATABASE 语句的位置：  
  
-   如果您在辅助副本上恢复辅助数据库，则只恢复本地辅助数据库。 除非数据库在主副本上也已暂停，否则将恢复数据移动。  
  
-   如果您在主副本上恢复数据库，则对尚未在本地挂起对应辅助数据库的每个辅助副本，将恢复数据移动。 若要恢复已在辅助副本上单独挂起的辅助数据库，请连接到承载辅助副本的服务器实例，然后在此恢复数据库。  
  
     在同步提交模式下，数据库的状态更改为“正在同步”。 如果当前没有其他数据库被挂起，副本的状态也会更改为“正在同步”。  
  
     有关详细信息，请参阅本主题后面的 [恢复可用性数据库 (SQL Server)](../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)中挂起可用性数据库。  
  
## <a name="database-states"></a>数据库状态  
 当辅助数据库加入可用性组后，本地辅助副本会将辅助数据库的状态从“正在还原”更改为“联机”。 当从可用性组中删除辅助数据库后，此数据库将由本地辅助副本设回为“正在还原”状态。 这使您能够将来自主数据库的后续日志备份应用于辅助数据库。  
  
## <a name="restrictions"></a>限制  
 在事务和批处理两者之外执行 ALTER DATABASE 语句。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要对数据库拥有 ALTER 权限。 将数据库联接到可用性组要求具有 db_owner 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例将辅助数据库 `AccountsDb1` 联接到 `AccountsAG` 可用性组的本地辅助副本。  
  
```sql  
ALTER DATABASE AccountsDb1 SET HADR AVAILABILITY GROUP = AccountsAG;  
```  
  
> [!NOTE]  
>  若要查看此用于上下文的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，请参阅[创建可用性组 (Transact-SQL)](../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) [解决 AlwaysOn 可用性组配置问题 (SQL Server)](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md) 
  
  
