---
description: DBCC CHECKCATALOG (Transact-SQL)
title: DBCC CHECKCATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC_CHECKCATALOG_TSQL
- DBCC CHECKCATALOG
- CHECKCATALOG_TSQL
- CHECKCATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- catalogs [SQL Server], consistency checks
- checking catalog consistency
- DBCC CHECKCATALOG statement
- integrity [SQL Server], catalogs
- consistency [SQL Server], catalogs
ms.assetid: 8076eb4e-f049-44bf-9a35-45cdd6ef0105
author: pmasl
ms.author: umajay
ms.openlocfilehash: fe40df4a8593b3ae00bb6733d30a4091a6179ef5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96119655"
---
# <a name="dbcc-checkcatalog-transact-sql"></a>DBCC CHECKCATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  检查指定数据库内的目录一致性。 数据库必须联机。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
DBCC CHECKCATALOG   
[   
    (   
    database_name | database_id | 0  
    )  
]  
    [ WITH NO_INFOMSGS ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 database_name | database_id | 0  
 要检查其目录一致性的数据库的名称和 ID。 如果未指定，或者指定为 0，则使用当前数据库。 数据库名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
 WITH NO_INFOMSGS  
 取消显示所有信息性消息。  
  
## <a name="remarks"></a>备注  
DBCC CATALOG 命令完成后，会将一条消息写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志。 如果 DBCC 命令成功执行，则消息指示成功完成以及命令运行的时间。 如果 DBCC 命令在完成检查之前由于错误而停止，则消息将指示命令已终止，并指示状态值和命令运行的时间。 下表列出并说明了此消息中可包含的状态值。
  
|状态|描述|  
|-----------|-----------------|  
|0|出现错误号 8930。 这指示导致 DBCC 命令终止的元数据损坏。|  
|1|出现错误号 8967。 存在一个内部 DBCC 错误。|  
|2|在紧急模式数据库修复过程中出错。|  
|3|这指示导致 DBCC 命令终止的元数据损坏。|  
|4|检测到断言或访问违规。|  
|5|出现终止了 DBCC 命令的未知错误。|  
  
DBCC CHECKCATALOG 在系统元数据表之间执行各种一致性检查。 DBCC CHECKCATALOG 使用内部数据库快照来提供需要执行这些检查的事务一致性。 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) 以及 [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md) 中的“DBCC 内部数据库快照使用情况”部分。
如果无法创建快照，则 DBCC CHECKCATALOG 将获取一个排他数据库锁以获得要求的一致性。 如果检测到任何不一致，则无法修复这些不一致问题，必须使用备份来还原数据库。
  
> [!NOTE]  
> 对 tempdb 运行 DBCC CHECKCATALOG 不会执行任何检查。 这是因为，为了提高性能，不允许对 tempdb 使用数据库快照。 这意味着，无法获得所需的事务一致性。 回收服务器以解析任何 tempdb 元数据问题。  
  
> [!NOTE]  
> DBCC CHECKCATALOG 不会检查 FILESTREAM 数据。 FILESTREAM 在文件系统中存储二进制大型对象 (BLOB)。  
  
DBCC CHECKCATALOG 也作为 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 的一部分运行。
  
## <a name="result-sets"></a>结果集  
如果未指定数据库，则 DBCC CHECKCATALOG 返回以下内容：
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
如果将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 指定为数据库名，则 DBCC CHECKCATALOG 返回以下内容：
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色或 db_owner 固定数据库角色的成员身份 。  
  
## <a name="examples"></a>示例  
以下示例将检查当前数据库和 `AdventureWorks` 数据库中的目录完整性。
  
```sql
-- Check the current database.  
DBCC CHECKCATALOG;  
GO  
-- Check the AdventureWorks2012 database.  
DBCC CHECKCATALOG (AdventureWorks2012);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)
  
