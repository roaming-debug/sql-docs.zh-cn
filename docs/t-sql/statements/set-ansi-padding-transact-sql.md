---
description: SET ANSI_PADDING (Transact-SQL)
title: SET ANSI_PADDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ANSI_PADDING_TSQL
- ANSI_PADDING
- SET_ANSI_PADDING_TSQL
- SET ANSI_PADDING
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], short values
- ANSI_PADDING option
- short values [SQL Server]
- SET ANSI_PADDING statement
- trailing blanks
ms.assetid: 92bd29a3-9beb-410e-b7e0-7bc1dc1ae6d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e90a5092b02eaf3f646c1a12b22ad1601a01b63
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754307"
---
# <a name="set-ansi_padding-transact-sql"></a>SET ANSI_PADDING (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  对列存储长度小于列的定义大小的值以及在 **char**、 **varchar**、 **binary** 和 **varbinary** 数据中含有尾随空格的值的方式进行控制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法
 
### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>[!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)] 的语法 
```syntaxsql
SET ANSI_PADDING { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>[!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] 和 [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)] 的语法
```syntaxsql
SET ANSI_PADDING ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 使用 **char**、**varchar**、**binary** 和 **varbinary** 数据类型定义的列具有定义的大小。  
  
 此设置只影响新列的定义。 创建列后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会基于创建列时的设置存储这些值。 以后对此设置的更改不会影响现有的列。  
  
> [!NOTE]  
> ANSI_PADDING 应始终设置为 ON。  
  
 下表显示在将值插入数据类型为 **char**、**varchar**、**binary** 和 **varbinary** 的列时，SET ANSI_PADDING 设置的效果。  
  
|设置|char(n) NOT NULL 或 binary(n) NOT NULL|char(n) NULL 或 binary(n) NULL|varchar(n) 或 varbinary(n)|  
|-------------|----------------------------------------------------|--------------------------------------------|----------------------------------------|  
|ON|填充原始值（**char** 列具有尾随空格的值，**binary** 列具有尾随零的值），以达到列的长度。|如果 SET ANSI_PADDING 为 ON，则遵从与 char(_n_) 或 binary(_n_) NOT NULL 相同的规则。|不剪裁插入 **varchar** 列中的字符值的尾随空格。 不剪裁插入 **varbinary** 列中的二进制值的尾随零。 不将值填充到列的长度。|  
|OFF|填充原始值（**char** 列具有尾随空格的值，**binary** 列具有尾随零的值），以达到列的长度。|如果 SET ANSI_PADDING 为 OFF，则遵从与 **varchar** 或 **varbinary** 相同的规则。|剪裁插入 **varchar** 列中的字符值的尾随空格。 剪裁插入 **varbinary** 列中的二进制值的尾随零。|  
  
> [!NOTE]  
> 进行填充时，**char** 列用空格填充，**binary** 列用零填充。 进行剪裁时，**char** 列的尾随空格被剪裁，**binary** 列的尾随零被剪裁。  
  
在创建或更改计算列的索引或索引视图时，ANSI_PADDING 必须为 ON。 有关计算列的索引视图和索引需要的 SET 选项设置的详细信息，请参阅 [SET Statements (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md) 中的“使用 SET 语句时的注意事项”。  
  
SET ANSI_PADDING 的默认值为 ON。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序在连接时会自动将 ANSI_PADDING 设置为 ON。 在连接之前，您可以在应用程序的 ODBC 数据源、ODBC 连接特性或 OLE DB 连接属性集合中配置此设置。 对于来自 DB-Library 应用程序的连接，SET ANSI_PADDING 的默认设置为 OFF。  
  
 SET ANSI_PADDING 设置不会影响 **nchar**、**nvarchar**、**ntext**、**text**、**image**、**varbinary(max)**、**varchar(max)** 和 **nvarchar(max)** 数据类型。 它们始终显示 SET ANSI_PADDING ON 行为。 这表示不剪裁尾随空格和尾随零。  
  
如果 ANSI_DEFAULTS 为 ON，则启用 ANSI_PADDING。  
  
ANSI_PADDING 的设置是在执行或运行时定义的，而不是在分析时定义的。  
  
要查看此设置的当前设置，请运行以下查询。  
  
```sql  
DECLARE @ANSI_PADDING VARCHAR(3) = 'OFF';  
IF ( (16 & @@OPTIONS) = 16 ) SET @ANSI_PADDING = 'ON';  
SELECT @ANSI_PADDING AS ANSI_PADDING;  
```  
  
## <a name="permissions"></a>权限  
要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
下面的示例演示了该设置对上述每个数据类型的影响。  

将 ANSI_PADDING 设置为 ON 并对其进行测试。

```sql  
PRINT 'Testing with ANSI_PADDING ON'  
SET ANSI_PADDING ON;  
GO  
  
CREATE TABLE t1 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t1 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t1 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '\<',  
   varbinarycol  
FROM t1;  
GO  
```

现在将 ANSI_PADDING 设置为 OFF 并对其进行测试。

```sql
PRINT 'Testing with ANSI_PADDING OFF';  
SET ANSI_PADDING OFF;  
GO  
  
CREATE TABLE t2 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t2 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t2 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '<',  
   varbinarycol  
FROM t2;  
GO  
  
DROP TABLE t1;  
DROP TABLE t2;  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY (Transact-SQL)](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
