---
title: sp_data_source_table_columns
description: 'sp_data_source_table_columns (Transact-sql) '
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_table_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
author: MikeRayMSFT
ms.author: mikeray
ms.custom: ''
ms.date: 11/10/2020
ms.openlocfilehash: 31b0dedb8544cc6752eac8a9934aa92d74d424ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081868"
---
# <a name="sp_data_source_table_columns-transact-sql"></a>sp_data_source_table_columns (Transact-sql) 

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

返回外部数据源表中列的列表。
  
> [!NOTE]
> 此过程在 [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5)中引入。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sqlsyntax
sp_data_source_table_columns
         [ @data_source = ] 'data_source'
       , [ @table_location = ] 'table_location'
[ ; ]
```  

## <a name="arguments"></a>参数

`[ @data_source = ] 'data_source'`   
要从中获取元数据的外部数据源的名称。 类型为 `sysname` 。

`[ @table_location = ] 'table_location'`   
用于标识表的表位置字符串。 `table_location` 类型为 `nvarchar(max)` 。

## <a name="returns"></a>返回

该存储过程将返回以下信息：

|列名 |数据类型 |说明|
|---|---|---|
|名称|nvarchar(max)|列的名称。
|TYPE|nvarchar(200)|SQL Server 类型名称
|LENGTH|int|列长度
|PRECISION|int|列精度
|SCALE|int|列的小数位数
|COLLATION|nvarchar(200)|列 SQL Server 排序规则
|IS_NULLABLE|bit|此列是否可以为 null。
|SOURCE_TYPE_NAME|nvarchar(max)|后端特定的类型名称。 主要用于调试。 对于 ODBC 源，这将对应于 SQLColumns ( # A1 的 TYPE_NAME 结果列。
|REMARKS|nvarchar(max)|列的常规注释或说明。 当前始终为 NULL。|

## <a name="permissions"></a>权限  

需要 ALTER ANY EXTERNAL DATA SOURCE 权限。
  
## <a name="remarks"></a>备注  

SQL Server 实例必须安装  [PolyBase](../../relational-databases/polybase/polybase-guide.md) 功能。

此存储过程支持的连接器：

- SQL Server
- Oracle
- Teradata
- MongoDB
- CosmosDB

该存储过程不支持通用 ODBC dta 源连接器。

空与非空的概念与 ODBC 驱动程序和[ `SQLTables` 函数](../native-client-odbc-api/sqltables.md)的行为相关。 非空表示对象包含表而非行。 例如，空架构在 SQL Server 中不包含任何表。 空数据库在 Teradata 中不包含任何表。结果是后端架构的 SQL Server 表示形式，由 PolyBase 连接器用于后端。 此处的区别在于，不只是将 ODBC 调用的结果传递给后端，结果基于 PolyBase 类型映射代码的结果。

使用 [`sp_data_source_objects`](sp-data-source-objects.md) 和 `sp_data_source_table_columns` 来发现外部对象。 这些系统存储过程返回可虚拟化的表的架构。 Azure Data Studio 使用这两个存储过程来支持 [数据虚拟化](../../azure-data-studio/extensions/data-virtualization-extension.md)。 使用 `sp_data_source_table_columns` 发现 SQL Server 数据类型中表示的外部表架构。

由于 Hadoop 源数据中的排序规则与 SQL Server 2019 中支持的排序规则不同，因此，外部表中的 varchar 数据类型列的建议数据类型长度可能会远远大于预期。 这是设计的结果。

## <a name="example"></a>示例  

下面的示例返回名为的 SQL Server 中的外部表的表列，该名称 `server` 属于名为的架构 `schema` 。
  
```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @table_location NVARCHAR(400) = N'[database].[schema].[table]';
EXEC sp_data_source_table_columns @data_source, @table_location
```  
  
## <a name="see-also"></a>另请参阅

- [PolyBase 入门](../polybase/polybase-guide.md)
- [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)