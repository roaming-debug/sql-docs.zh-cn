---
description: ALTER TABLE column_definition (Transact-SQL)
title: column_definition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- column_definition
- column_definition_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column_definition
- ALTER TABLE statement
- column properties [SQL Server]
- column definitions [SQL Server]
ms.assetid: a1742649-ca29-4d9b-9975-661cdbf18f78
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3c5c23b2cdccf464ec86d88f50073143a979ca8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237122"
---
# <a name="alter-table-column_definition-transact-sql"></a>ALTER TABLE column_definition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  指定使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 添加到表中的列的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
column_name <data_type>  
[ FILESTREAM ]  
[ COLLATE collation_name ]   
[ NULL | NOT NULL ]  
[   
    [ CONSTRAINT constraint_name ] DEFAULT constant_expression [ WITH VALUES ]   
    | IDENTITY [ ( seed , increment ) ] [ NOT FOR REPLICATION ]   
]  
[ ROWGUIDCOL ]   
[ SPARSE ]   
[ ENCRYPTED WITH  
  ( COLUMN_ENCRYPTION_KEY = key_name ,  
      ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,   
      ALGORITHM =  'AEAD_AES_256_CBC_HMAC_SHA_256'   
  ) ]  
[ MASKED WITH ( FUNCTION = ' mask_function ') ]  
[ <column_constraint> [ ...n ] ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
[ CONSTRAINT constraint_name ]   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH FILLFACTOR = fillfactor    
          | WITH ( < index_option > [ , ...n ] )   
        ]   
        [ ON { partition_scheme_name ( partition_column_name )   
            | filegroup | "default" } ]  
  | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 column_name  
 要更改、添加或删除的列的名称。 column_name 可以包含 1 到 128 个字符。 对于使用 timestamp 数据类型创建的新列，可以省略 column_name。 如果没有为 timestamp 数据类型的列指定 column_name，则使用名称 timestamp 。  
  
 [ type_schema_name. ] type_name  
 是添加的列的数据类型及其所属架构。  
  
 type_name 可以为：  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。  
  
-   基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的别名数据类型。 别名数据类型必须先使用 CREATE TYPE 进行创建，然后才能在表定义中使用。  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 用户定义类型及其所属架构。 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 用户定义类型必须先使用 CREATE TYPE 进行创建，然后才能在表定义中使用。  
  
 如果未指定 type_schema_name，则 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] 按照下列顺序引用 type_name ：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。  
  
-   当前数据库中当前用户的默认架构。  
  
-   当前数据库中的 **dbo** 架构。  
  
*精度*  
 指定的数据类型的精度。 有关有效精度值的详细信息，请参阅[精度、小数位数和长度 (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。  
  
*scale*  
 是指定数据类型的小数位数。 有关有效小数位数值的详细信息，请参阅[精度、小数位数和长度 (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)。  
  
**max**  
 仅适用于 varchar、nvarchar 和 varbinary 数据类型  。 它们用于存储 2^31 个字节的字符和二进制数据，以及 2^30 个字节的 Unicode 数据。  
  
CONTENT  
 指定 column_name 中 xml 数据类型的每个实例都可包含多个顶级元素。 CONTENT 仅适用于 xml 数据类型，并且只有在同时指定了 xml_schema_collection 时才能指定 CONTENT。 如果未指定，则默认行为是 CONTENT。  
  
DOCUMENT  
 指定 column_name 中 xml 数据类型的每个实例只能包含一个顶级元素。 DOCUMENT 仅适用于 xml 数据类型，并且只有在同时指定了 xml_schema_collection 时才能指定 DOCUMENT。  
  
 xml_schema_collection  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 仅适用于 xml 数据类型，用于将 XML 架构集合与该类型相关联。 在架构中键入 xml 列之前，须先使用 [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) 在数据库中创建该架构。  
  
FILESTREAM  
 还可以为拥有数据类型为 varbinary(max) 的 type_name 的列指定 FILESTREAM 存储属性。  
  
 为列指定了 FILESTREAM 后，该表还必须有一个具有 ROWGUIDCOL 属性且数据类型为 uniqueidentifier 的列。 此列不得为空值且必须具有 UNIQUE 或 PRIMARY KEY 单列约束。 该列的 GUID 值必须在插入数据时由应用程序提供，或由使用 NEWID () 函数的 DEFAULT 约束提供。  
  
 如果为表定义了 FILESTREAM 列，则不能删除 ROWGUIDCOL 列并且不能更改相关的约束。 仅当删除了最后一个 FILESTREAM 列后，才能删除 ROWGUIDCOL 列。  
  
 当为某个列指定了 FILESTREAM 存储属性时，该列的所有值都将存储在文件系统上的 FILESTREAM 数据容器中。  
  
 有关如何使用列定义的示例，请参阅 [FILESTREAM (Transact-SQL)](../../relational-databases/blob/filestream-sql-server.md)。  
  
COLLATE collation_name  
 指定列的排序规则。 如果未指定，则为该列分配数据库的默认排序规则。 排序规则名称既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 如需获取列表和详细信息，请参阅 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) 和 [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。  
  
 COLLATE 子句只能用来指定数据类型为 char、varchar、nchar 和 nvarchar 的列的排序规则   。  
  
 有关 COLLATE 子句的详细信息，请参阅 [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md)。  
  
 NULL | NOT NULL  
 确定列中是否允许空值。 严格来讲，NULL 不是约束，但可以像指定 NOT NULL 那样指定它。  
  
[ CONSTRAINT constraint_name ]  
 指定 DEFAULT 值定义的开头。 为了与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本兼容，可以为 DEFAULT 分配约束名称。 除了不能以数字符号 (#) 开头以外，约束名称还必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 如果未指定 constraint_name，则 DEFAULT 定义使用系统生成的名称。  
  
DEFAULT  
 指定列的默认值的关键字。 DEFAULT 定义可用于为表中现有数据行的新列提供值。 DEFAULT 定义不能应用于 timestamp 列，或具有 IDENTITY 属性的列。 如果为用户定义类型列指定了默认值，则该类型必须支持从 constant_expression 到用户定义类型的隐式转换。  
  
constant_expression  
 用作默认列值的文字值、NULL 或者系统函数。 如果与定义为 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 用户定义类型的列结合使用，则该类型的实现必须支持从 constant_expression 到用户定义类型的隐式转换。  
  
WITH VALUES   
 添加列和 DEFAULT 约束时，如果列允许为空，那么对于现有行，使用 WITH VALUES 会将新列的值设置为 DEFAULT constant_expression 中给定的值。 如果要添加的列不允许为空，那么对于现有行，列值始终设置为 DEFAULT constant_expression 中给定的值。 自 SQL Server 2012 起，这可能是元数据操作 [adding-not-null-columns-as-an-online-operation](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation)。
如果在没有同时添加相关列的情况下使用它，它将不起作用。
 
 指定 DEFAULT constant_expression 中给定的值将存储在添加到现有行的新列中。 如果所添加的列允许 Null 值且指定了 WITH VALUES，则默认值将存储在添加到现有行的新列中。 如果没有对允许为空的列指定 WITH VALUES，那么现有行对应的新列中存储的是值 NULL。 如果新列不允许 Null 值，那么不论是否指定 WITH VALUES，都将在新行中存储默认值。  
  
IDENTITY  
 指定新列为标识列。 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]为该列提供唯一的增量值。 当您向现有表中添加标识符列时，还会将标识号添加到具有种子值和增量值的现有表行中。 无法保证行的更新顺序。 也会为添加的任何新行生成标识号。  
  
 标识列通常与 PRIMARY KEY 约束一起使用，作为表的唯一行标识符。 可以将 IDENTITY 属性分配到 tinyint、smallint、int、bigint、decimal(p,0) 或 numeric(p,0) 列     。 每个表只能创建一个标识列。 DEFAULT 关键字和绑定默认值不能用于标识列。 要么同时指定种子和增量，要么都不指定。 如果二者都未指定，则取默认值 (1,1)。  
  
> [!NOTE]  
>  不能修改现有的表列以添加 IDENTITY 属性。  
  
 不支持向已发布的表添加标识列，因为将列复制到订阅服务器时，这会导致无法收敛。 发布服务器的标识列中的值取决于受影响的表中行的物理存储顺序。 行在订阅服务器中的存储顺序可能会有所不同；因此对于相同的行，标识列的值可能会不同。  
  
 若要通过允许显式插入值来禁用某列的 IDENTITY 属性，请使用 [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md)。  
  
seed  
 用于表中所加载的第一行的值。  
  
increment  
 增加到上一个加载行的标识值的增量值。  
  
NOT FOR REPLICATION  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 可以为 IDENTITY 属性指定该子句。 如果为 IDENTITY 属性指定了该子句，则当复制代理执行插入操作时，标识列中的值不会增加。  
  
ROWGUIDCOL  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指定该列是一个行全局唯一标识符列。 只能为 uniqueidentifier 列分配 ROWGUIDCOL，并且每个表中只有一个 uniqueidentifier 列能指定为 ROWGUIDCOL 列 。 不能为用户定义数据类型分配 ROWGUIDCOL。  
  
 ROWGUIDCOL 并不强制列中所存储值的唯一性。 另外，该属性也不会为插入到表中的新行自动生成值。 若要为每列生成唯一值，则可以在 INSERT 语句中使用 NEWID 函数，也可以将 NEWID 函数指定为列的默认值。 有关详细信息，请参阅 [NEWID (Transact-SQL)](../../t-sql/functions/newid-transact-sql.md) 和 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)。  
  
SPARSE  
 指示列为稀疏列。 稀疏列已针对 NULL 值进行了存储优化。 不能将稀疏列指定为 NOT NULL。 有关稀疏列的其他限制和详细信息，请参阅[使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。  
  
\<column_constraint>  
 有关列约束参数的定义，请参阅 [column_constraint (Transact-SQL)](../../t-sql/statements/alter-table-column-constraint-transact-sql.md)。  
  
 ENCRYPTED WITH  
 使用 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) 功能指定加密列。  
  
 COLUMN_ENCRYPTION_KEY = key_name  
 指定列加密密钥。 有关详细信息，请参阅 [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/create-column-encryption-key-transact-sql.md)。  
  
ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }  
 **确定性加密** 使用一种对任何给定的纯文本值始终生成相同的加密值的方法。 使用确定性加密可基于加密值进行下列操作：使用相等性比较进行搜索、分组、使用相等性联接联接各表，此外，还允许未经授权的用户通过检查加密列中的模式来猜测加密值的相关信息。 只有使用相同的列加密密钥对两列进行加密，才能在已进行确定性加密的该列上联结两个表。 确定性加密必须使用具有字符列的 binary2 排序顺序的列排序规则。  
  
 **随机加密** 使用一种以更不可预测地方式加密数据的方法。 随机加密更为安全，但会阻止对加密列进行任何计算和索引编制，除非你的 SQL Server 实例支持[具有安全 enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。
  
 如果使用的是 Always Encrypted（不带安全 enclave），请对要使用参数或分组参数搜索的列使用确定性加密，例如政府 ID 号。 对以下数据使用随机加密，即未与其他记录分组的数据、未用于联结表的数据、因使用其他列（例如事务号）查找包含已加密的兴趣列，而不用于搜索的数据。  

 如果使用的是具有安全 enclave 的 Always Encrypted，则随机加密是推荐的加密类型。  
  
 列必须是符合条件的数据类型。  
  
ALGORITHM  
**适用于**：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]。  
须是“AEAD_AES_256_CBC_HMAC_SHA_256”。  
  
 有关包括功能约束在内的详细信息，请参阅 [Always Encrypted (Transact-SQL)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。  
  
   
ADD MASKED WITH ( FUNCTION = ' mask_function ')  
 **适用于**：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 指定动态数据掩码。 mask_function 是具有相应参数的掩码函数的名称。 可用函数包括：  
  
-   default()  
  
-   email()  
  
-   partial()  
  
-   random()  
  
 有关函数参数的信息，请参阅[动态数据掩码](../../relational-databases/security/dynamic-data-masking.md)。  
  
## <a name="remarks"></a>备注  
 如果添加的列具有 uniqueidentifier 数据类型，则可以通过使用一个使用 NEWID() 函数的默认值对该列进行定义，以向表中的每个现有行的新列提供唯一标识符值。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 不强制在列定义中指定 DEFAULT、IDENTITY、ROWGUIDCOL 或列约束的顺序。  
  
 如果添加列导致数据行大小超过 8060 字节，ALTER TABLE 语句将失败。  
  
## <a name="examples"></a>示例  
 有关示例，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
