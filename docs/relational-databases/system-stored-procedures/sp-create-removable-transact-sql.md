---
description: sp_create_removable (Transact-SQL)
title: sp_create_removable (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_removable
- sp_create_removable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_removable
ms.assetid: 06e36ae5-f70d-4a26-9a7f-ee4b9360b355
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7ffe2273f7bd2ca63143f77a4d0c5726709d8440
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205198"
---
# <a name="sp_create_removable-transact-sql"></a>sp_create_removable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建可移动介质数据库。 创建三个或更多文件（一为系统目录表，一为事务日志，其余文件为数据表）并将数据库置于这些文件之中。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 建议改用 [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_create_removable   
   [ @dbname = ] 'dbname',   
   [ @syslogical= ] 'syslogical',   
   [ @sysphysical = ] 'sysphysical',   
   [ @syssize = ] syssize,   
   [ @loglogical = ] 'loglogical',   
   [ @logphysical = ] 'logphysical',   
   [ @logsize = ] logsize,   
   [ @datalogical1 = ] 'datalogical1',   
   [ @dataphysical1 = ] 'dataphysical1',   
   [ @datasize1 = ] datasize1 ,   
   [ @datalogical16 = ] 'datalogical16',   
   [ @dataphysical16 = ] 'dataphysical16',   
   [ @datasize16 = ] datasize16 ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'dbname'` 要创建以便在可移动介质上使用的数据库的名称。 *dbname* 为 **sysname**。  
  
`[ @syslogical = ] 'syslogical'` 包含系统目录表的文件的逻辑名称。 *syslogical* 为 **sysname**。  
  
`[ @sysphysical = ] 'sysphysical'` 物理名称。 其中包含存放系统目录表的文件的完全限定路径。 *sysphysical* 是 **nvarchar (260)**。  
  
`[ @syssize = ] syssize` 包含系统目录表的文件的大小（以 mb 为单位）。 *syssize* 为 **int**。最小 *syssize* 为1。  
  
`[ @loglogical = ] 'loglogical'` 包含事务日志的文件的逻辑名称。 *loglogical* 为 **sysname**。  
  
`[ @logphysical = ] 'logphysical'` 物理名称。 其中包含存放事务日志的文件的完全限定路径。 *logphysical* 是 **nvarchar (260)**。  
  
`[ @logsize = ] logsize` 包含事务日志的文件的大小（以 mb 为单位）。 *logsize* 为 **int**。最小 *logsize* 为1。  
  
`[ @datalogical1 = ] 'datalogical'` 包含数据表的文件的逻辑名称。 *datalogical* 为 **sysname**。  
  
 必须有 1 到 16 个数据文件。 通常，如果预计数据库很大，必须分布在多个磁盘上，则创建多个数据文件。  
  
`[ @dataphysical1 = ] 'dataphysical'` 物理名称。 其中包括包含数据表的文件的完全限定路径。 *dataphysical* 是 **nvarchar (260)**。  
  
`[ @datasize1 = ] 'datasize'` 包含数据表的文件的大小（以 mb 为单位）。 *datasize* 为 **int**。最小 *datasize* 为1。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 如果要在可移动介质（如光盘）上制作数据库的副本，并将该数据库分发给其他用户，则可使用此存储过程。  
  
## <a name="permissions"></a>权限  
 要求具有 CREATE DATABASE、CREATE ANY DATABASE 或 ALTER ANY DATABASE 权限。  
  
 为了控制对运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的计算机上的磁盘使用，通常只有少数登录帐户才有创建数据库的权限。  
  
### <a name="permissions-on-data-and-log-files"></a>对数据文件和日志文件的权限  
 在对数据库执行某些操作时，将对其数据和日志文件设置相应的权限。 如果这些文件位于具有打开权限的目录中，那么以上权限可以防止文件被意外篡改。  
  
|针对数据库的操作|针对文件的权限集|  
|---------------------------|------------------------------|  
|修改以添加新文件|创建|  
|备份|附加|  
|还原|分离|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不设置数据文件和日志文件权限。  
  
## <a name="examples"></a>示例  
 以下示例创建作为可移动数据库的数据库 `inventory`。  
  
```  
EXEC sp_create_removable 'inventory',   
   'invsys',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invsys.mdf'  
, 2,   
   'invlog',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invlog.ldf', 4,  
   'invdata',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invdata.ndf',   
10;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_certify_removable &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-certify-removable-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [sp_detach_db &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_helpfilegroup &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
