---
description: ALTER PARTITION SCHEME (Transact-SQL)
title: ALTER PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER PARTITION SCHEME
- ALTER_PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PARTITION SCHEME statement
- partition schemes [SQL Server], modifying
- modifying partition schemes
- adding filegroups
- NEXT USED filegroups
ms.assetid: f01d6880-9800-4cfb-8d11-d4be21efc8ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 084263fab973e11c1c0a14e340f18ae952ca5f44
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094720"
---
# <a name="alter-partition-scheme-transact-sql"></a>ALTER PARTITION SCHEME (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  向分区方案中添加文件组或更改分区方案中 NEXT USED 文件组的指定。 

>[!NOTE]
>Azure SQL 数据库中仅支持主文件组。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER PARTITION SCHEME partition_scheme_name   
NEXT USED [ filegroup_name ] [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 partition_scheme_name  
 要更改的分区方案的名称。  
  
 filegroup_name  
 指定要由分区方案标记为 NEXT USED 的文件组。 这意味着文件组将接受使用 [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md) 语句创建的新分区。  
  
 在一个分区方案中，只能将一个文件组指定为 NEXT USED。 可以指定非空文件组。 如果指定了 filegroup_name 且当前没有文件组被标记为 NEXT USED，则将 filegroup_name 标记为 NEXT USED。 如果指定了 filegroup_name 且已经存在具有 NEXT USED 属性的文件组，则 NEXT USED 属性从现有的文件组转移到 filegroup_name。  
  
 如果没有指定 filegroup_name 且已经存在具有 NEXT USED 属性的文件组，则该文件组将失去其 NEXT USED 状态，因此 partition_scheme_name 中将没有 NEXT USED 文件组。  
  
 如果指定了 filegroup_name 且没有文件组被标记为 NEXT USED，则 ALTER PARTITION SCHEME 将返回一个警告。  
  
## <a name="remarks"></a>注解  
 受 ALTER PARTITION SCHEME 影响的所有文件组都必须处于联机状态。  
  
## <a name="permissions"></a>权限  
 以下权限可用于执行 ALTER PARTITION SCHEME：  
  
-   ALTER ANY DATASPACE 权限。 默认情况下，此权限授予 **sysadmin** 固定服务器角色和 **db_owner** 及 **db_ddladmin** 固定数据库角色的成员。  
  
-   对创建分区方案时所在数据库的 CONTROL 或 ALTER 权限。  
  
-   对承载了创建分区方案时所在数据库的服务器的 CONTROL SERVER 或 ALTER ANY DATABASE 权限。  
  
## <a name="examples"></a>示例  
 以下示例假设当前数据库中已存在分区方案 `MyRangePS1` 和文件组 `test5fg`。  
  
```sql  
ALTER PARTITION SCHEME MyRangePS1  
NEXT USED test5fg;  
```  
  
 作为 ALTER PARTITION FUNCTION 语句的结果，文件组 `test5fg` 将接收已分区表或索引的所有其他分区。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_schemes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
