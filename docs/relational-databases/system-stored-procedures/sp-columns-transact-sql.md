---
description: sp_columns (Transact-SQL)
title: sp_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7257b24d2908ea17977c0b08f0517d65d2481979
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439482"
---
# <a name="sp_columns-transact-sql"></a>sp_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回在当前环境中可查询的指定对象的列信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>自变量  
`[ \@table_name = ] object` 用于返回目录信息的对象的名称。 *对象* 可以是表、视图或具有列（如表值函数）的其他对象。 *对象* 为 **nvarchar (384)**，无默认值。 支持通配符模式匹配。  
  
`[ \@table_owner = ] owner` 用于返回目录信息的对象的所有者。 *所有者* 为 **nvarchar (384)**，默认值为 NULL。 支持通配符模式匹配。 如果未指定 *owner* ，则应用基础 DBMS 的默认对象可见性规则。  
  
 如果当前用户拥有一个具有指定名称的对象，则返回该对象的列。 如果未指定 *owner* ，并且当前用户没有具有指定 *对象* 的对象， **sp_columns** 将查找具有数据库所有者拥有的指定 *对象* 的对象。 如果存在这样的对象，则返回该对象的列。  
  
`[ \@table_qualifier = ] qualifier` 对象限定符的名称。 *限定符* 的值为 **sysname**，默认值为 NULL。 各种 DBMS 产品支持三部分命名 (限定符的对象 **。**_所有者_**。**_名称_) 。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列表示数据库名称。 在某些产品中，它表示对象数据库环境的服务器名称。  
  
`[ \@column_name = ] column` 是单个列，只需要目录信息的一列时使用。 *列* **(384) 为 nvarchar**，默认值为 NULL。 如果未指定 *column* ，则返回所有列。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， *列* 表示 **syscolumns** 表中列出的列名称。 支持通配符模式匹配。 为了获得最大互操作性，网关客户端应只采用 SQL-92 标准模式匹配（% 和 _ 通配符）。  
  
`[ \@ODBCVer = ] ODBCVer` 所使用的 ODBC 的版本。 *ODBCVer* 的值为 **int**，默认值为2。 这指示 ODBC 版本 2。 有效值为 2 或 3。 有关版本2和3之间的行为差异，请参阅 ODBC **SQLColumns** 规范。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 **Sp_columns** 目录存储过程与 ODBC 中的 **SQLColumns** 等效。 返回的结果按 **TABLE_QUALIFIER**、 **TABLE_OWNER** 和 **TABLE_NAME** 排序。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|对象限定符名称。 此字段可以为 NULL。|  
|**TABLE_OWNER**|**sysname**|对象所有者名称。 此字段始终返回值。|  
|**TABLE_NAME**|**sysname**|对象名称。 此字段始终返回值。|  
|**COLUMN_NAME**|**sysname**|列名称，为返回的每个列的 **TABLE_NAME** 。 此字段始终返回值。|  
|**DATA_TYPE**|**smallint**|ODBC 数据类型的整数代码。 如果该数据类型无法映射到 ODBC 类型，则为 NULL。 本机数据类型名称在 **TYPE_NAME** 列中返回。|  
|**TYPE_NAME**|**sysname**|表示数据类型的字符串。 基础 DBMS 提供此数据类型的名称。|  
|**PRECISION**|**int**|有效数字位数。 **PRECISION** 列的返回值以10为底。|  
|**LENGTH**|**int**|数据的传输大小。<sup>1</sup>|  
|**纵向**|**smallint**|小数点右边的数字位数。|  
|**RADIX**|**smallint**|数值数据类型的基数。|  
|**可以为 NULL**|**smallint**|指定为 Null 性。<br /><br /> 1 = 可以为 NULL。<br /><br /> 0 = 不可以为 NULL。|  
|**备注**|**varchar (254)**|该字段总是返回 NULL。|  
|**COLUMN_DEF**|**nvarchar(4000)**|列的默认值。|  
|**SQL_DATA_TYPE**|**smallint**|SQL 数据类型在描述符的 TYPE 字段中显示的值。 此列与 **DATA_TYPE** 列相同， **datetime** 和 92 **间隔** 数据类型除外。 该列始终返回值。|  
|**SQL_DATETIME_SUB**|**smallint**|**Datetime** 和 SQL-92 **间隔** 数据类型的子类型代码。 对于其他数据类型，该列返回 NULL。|  
|**CHAR_OCTET_LENGTH**|**int**|字符或整数数据类型的列的最大长度（字节）。 对于所有其他数据类型，该列返回 NULL。|  
|**ORDINAL_POSITION**|**int**|列在对象中的序号位置。 对象中的第一列为 1。 该列始终返回值。|  
|**IS_NULLABLE**|**varchar (254)**|对象中列的是否可为 Null。 根据 ISO 规则确定为 Null 性。 遵从 ISO SQL 标准的 DBMS 不能返回空字符串。<br /><br /> YES = 列可以包含 NULL。<br /><br /> NO = 列不能包含 NULL。<br /><br /> 如果为 Null 性为未知，该列将返回零长度字符串。<br /><br /> 为此列返回的值与为 **可为 null** 的列返回的值不同。|  
|**SS_DATA_TYPE**|**tinyint**|扩展存储过程使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。|  
  
 <sup>1</sup> 有关详细信息，请参阅 Microsoft ODBC 文档。  
  
## <a name="permissions"></a>权限  
 要求对架构具有 SELECT 和 VIEW DEFINITION 权限。  
  
## <a name="remarks"></a>备注  
 **sp_columns** 遵循分隔标识符的要求。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
## <a name="examples"></a>示例  
 以下示例返回指定表的列信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例返回指定表的列信息。  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_tables &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [&#40;Transact-sql&#41;的目录存储过程 ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


