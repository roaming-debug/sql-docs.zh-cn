---
title: SQL Server 架构集合
description: 描述 Microsoft SqlClient Data Provider for SQL Server 支持的其他架构集合。
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3d757c09717d885dbf9bf35dc6826234ef4ea59e
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771509"
---
# <a name="sql-server-schema-collections"></a>SQL Server 架构集合

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

除了通用架构集合，Microsoft SqlClient Data Provider for SQL Server 还支持其他架构集合。 架构集合因使用的 SQL Server 的版本而稍有不同。 要确定支持的架构集合列表，请以无参数的形式或使用架构集合名称“MetaDataCollections”调用 GetSchema 方法。 此时将返回 <xref:System.Data.DataTable>，包含支持的架构集合列表、每个架构集合支持的限制数以及所使用的标识符部分数。  

## <a name="databases"></a>数据库  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|database_name|字符串|数据库的名称。|  
|dbid|Int16|数据库 ID。|  
|create_date|DateTime|数据库的创建日期。|  

## <a name="foreign-keys"></a>Foreign Keys  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|CONSTRAINT_CATALOG|字符串|约束所属的编录。|  
|CONSTRAINT_SCHEMA|字符串|包含约束的架构。|  
|CONSTRAINT_NAME|字符串|名称：|  
|TABLE_CATALOG|字符串|约束所属的表名。|  
|TABLE_SCHEMA|字符串|包含表的架构。|  
|TABLE_NAME|字符串|表名称|  
|CONSTRAINT_TYPE|字符串|约束的类型。 只允许“FOREIGN KEY”。|  
|IS_DEFERRABLE|字符串|指定约束是否可以推迟。 返回 NO。|  
|INITIALLY_DEFERRED|字符串|指定约束最初是否可以推迟。 返回 NO。|  

## <a name="indexes"></a>索引  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|constraint_catalog|字符串|索引所属的编录。|  
|constraint_schema|字符串|包含索引的架构。|  
|constraint_name|字符串|索引的名称。|  
|table_catalog|字符串|索引关联的表名。|  
|table_schema|字符串|包含索引关联的表的架构。|  
|table_name|字符串|表名。|  
|index_name|字符串|索引名称。|  
|type_desc|字符串|索引类型可以为以下值之一：<br /><br /> -   HEAP<br />-   CLUSTERED<br />-   NONCLUSTERED<br />-   XML<br />-   SPATIAL|  

## <a name="indexcolumns"></a>IndexColumns  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|constraint_catalog|字符串|索引所属的编录。|  
|constraint_schema|字符串|包含索引的架构。|  
|constraint_name|字符串|索引的名称。|  
|table_catalog|字符串|索引关联的表名。|  
|table_schema|字符串|包含索引关联的表的架构。|  
|table_name|字符串|表名。|  
|column_name|字符串|索引关联的列名。|  
|ordinal_position|Int32|列序号位置。|  
|KeyType|Byte|对象的类型。|  
|index_name|字符串|索引名称。| 

## <a name="procedures"></a>过程  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|字符串|编录的特定名称。|  
|SPECIFIC_SCHEMA|字符串|特定的架构名称。|  
|SPECIFIC_NAME|字符串|特定的目录名称。|  
|ROUTINE_CATALOG|字符串|存储过程所属的编录。|  
|ROUTINE_SCHEMA|字符串|包含存储过程的架构。|  
|ROUTINE_NAME|字符串|存储过程的名称。|  
|ROUTINE_TYPE|字符串|对于存储过程，返回 PROCEDURE，对于函数，返回 FUNCTION。|  
|CREATED|DateTime|过程的创建时间。|  
|LAST_ALTERED|DateTime|上次修改过程的时间。| 

## <a name="procedure-parameters"></a>过程参数  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|字符串|此参数所属的过程的编录名称。|  
|SPECIFIC_SCHEMA|字符串|包含此参数所属的过程的架构。|  
|SPECIFIC_NAME|字符串|此参数所属的过程的名称。|  
|ORDINAL_POSITION|Int32|参数的序号位置从 1 开始。 对于过程的返回值，此位置为 0。|  
|PARAMETER_MODE|字符串|如果是输入参数，则返回 IN；如果是输出参数，则返回 OUT；如果是输入/输出参数，则返回 INOUT。|  
|IS_RESULT|字符串|如果指示属于函数的过程的结果，返回 YES。 否则，返回 NO。|  
|AS_LOCATOR|字符串|如果声明为定位器，则返回 YES。 否则，返回 NO。|  
|PARAMETER_NAME|字符串|参数的名称。 如果该名称对应于函数的返回值，则为 NULL。|  
|DATA_TYPE|字符串|系统提供的数据类型。|  
|CHARACTER_MAXIMUM_LENGTH|Int32|二进制或字符数据类型的最大长度（字符）。 否则，返回 NULL。|  
|CHARACTER_OCTET_LENGTH|Int32|二进制或字符数据类型的最大长度（字节）。 否则，返回 NULL。|  
|COLLATION_CATALOG|字符串|参数排序规则的编录名称。 如果不是一种字符类型，则返回 NULL。|  
|COLLATION_SCHEMA|字符串|始终返回 NULL。|  
|COLLATION_NAME|字符串|参数排序规则的名称。 如果不是一种字符类型，则返回 NULL。|  
|CHARACTER_SET_CATALOG|字符串|参数字符集的编录名称。 如果不是一种字符类型，则返回 NULL。|  
|CHARACTER_SET_SCHEMA|字符串|始终返回 NULL。|  
|CHARACTER_SET_NAME|字符串|参数字符集的名称。 如果不是一种字符类型，则返回 NULL。|  
|NUMERIC_PRECISION|Byte|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|NUMERIC_PRECISION_RADIX|Int16|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|NUMERIC_SCALE|Int32|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|DATETIME_PRECISION|Int16|如果参数类型为 datetime 或 smalldatetime，则为小数秒数的精度。 否则，返回 NULL。|  
|INTERVAL_TYPE|字符串|NULL。 保留供 SQL Server 以后使用。|  
|INTERVAL_PRECISION|Int16|NULL。 保留供 SQL Server 以后使用。| 

## <a name="tables"></a>表  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|字符串|表的编录。|  
|TABLE_SCHEMA|字符串|包含表的架构。|  
|TABLE_NAME|字符串|表名。|  
|TABLE_TYPE|字符串|表的类型。 可以是 VIEW 或 BASE TABLE。|  

## <a name="columns"></a>列  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|字符串|表的编录。|  
|TABLE_SCHEMA|字符串|包含表的架构。|  
|TABLE_NAME|字符串|表名。|  
|COLUMN_NAME|字符串|列名称。|  
|ORDINAL_POSITION|Int32|列标识号。|  
|COLUMN_DEFAULT|字符串|列的默认值。|  
|IS_NULLABLE|字符串|列的为空性。 如果此列允许 NULL，此列将返回 YES。 否则，返回 No。|  
|DATA_TYPE|字符串|系统提供的数据类型。|  
|CHARACTER_MAXIMUM_LENGTH|Int32 – Sql8、Int16 – Sql7|二进制数据、字符数据或文本和图像数据的最大长度（字符）。 否则，返回 NULL。|  
|CHARACTER_OCTET_LENGTH|Int32 – SQL8、Int16 – Sql7|二进制数据、字符数据或文本和图像数据的最大长度（字节）。 否则，返回 NULL。|  
|NUMERIC_PRECISION|Unsigned Byte|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|NUMERIC_PRECISION_RADIX|Int16|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|NUMERIC_SCALE|Int32|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|DATETIME_PRECISION|Int16|datetime 及 SQL-92 interval 数据类型的子类型代码。 对于其他数据类型，返回 NULL。|  
|CHARACTER_SET_CATALOG|字符串|如果列为字符数据或文本数据类型，则返回 master，指示字符集所处的数据库。 否则，返回 NULL。|  
|CHARACTER_SET_SCHEMA|字符串|始终返回 NULL。|  
|CHARACTER_SET_NAME|字符串|如果此列为字符数据或文本数据类型，则返回字符集的唯一名称。 否则，返回 NULL。|  
|COLLATION_CATALOG|字符串|如果列为字符数据或文本数据类型，则返回 master，指示定义分页的数据库。 否则，此列为 NULL。|  
|IS_FILESTREAM|字符串|YES       FILESTREAM    <br /><br /> NO，如果列不具有 FILESTREAM 属性。|  
|IS_SPARSE|字符串|YES，如果列是稀疏列。<br /><br /> NO，如果列不是稀疏列。|  
|IS_COLUMN_SET|字符串|YES，如果列是一个列集列。<br /><br /> NO            |  

## <a name="allcolumns"></a>AllColumns  

 AllColumns 架构集合用于支持稀疏列。 AllColumns 与 Columns 架构集合具有相同的限制和生成的 DataTable 架构。 唯一的区别是 AllColumns 包含 Columns 架构集合中未包含的列集列。 下表介绍了这些列。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|字符串|表的编录。|  
|TABLE_SCHEMA|字符串|包含表的架构。|  
|TABLE_NAME|字符串|表名。|  
|COLUMN_NAME|字符串|列名称。|  
|ORDINAL_POSITION|Int32|列标识号。|  
|COLUMN_DEFAULT|字符串|列的默认值。|  
|IS_NULLABLE|字符串|列的为空性。 如果此列允许 NULL，此列将返回 YES。 否则，返回 NO。|  
|DATA_TYPE|字符串|系统提供的数据类型。|  
|CHARACTER_MAXIMUM_LENGTH|Int32|二进制数据、字符数据或文本和图像数据的最大长度（字符）。 否则，返回 NULL。|  
|CHARACTER_OCTET_LENGTH|Int32|二进制数据、字符数据或文本和图像数据的最大长度（字节）。 否则，返回 NULL。|  
|NUMERIC_PRECISION|Unsigned Byte|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|NUMERIC_PRECISION_RADIX|Int16|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|NUMERIC_SCALE|Int32|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|DATETIME_PRECISION|Int16|datetime 及 SQL-92 interval 数据类型的子类型代码。 对于其他数据类型，返回 NULL。|  
|CHARACTER_SET_CATALOG|字符串|如果列为字符数据或文本数据类型，则返回 master，指示字符集所处的数据库。 否则，返回 NULL。|  
|CHARACTER_SET_SCHEMA|字符串|始终返回 NULL。|  
|CHARACTER_SET_NAME|字符串|如果此列为字符数据或文本数据类型，则返回字符集的唯一名称。 否则，返回 NULL。|  
|COLLATION_CATALOG|字符串|如果列为字符数据或文本数据类型，则返回 master，指示定义分页的数据库。 否则，此列为 NULL。|  
|IS_FILESTREAM|字符串|YES       FILESTREAM    <br /><br /> NO，如果列不具有 FILESTREAM 属性。|  
|IS_SPARSE|字符串|YES，如果列是稀疏列。<br /><br /> NO，如果列不是稀疏列。|  
|IS_COLUMN_SET|字符串|YES，如果列是一个列集列。<br /><br /> NO            |  

## <a name="columnsetcolumns"></a>ColumnSetColumns

ColumnSetColumns 架构集合用于支持稀疏列。 ColumnSetColumns 架构集合返回列集中所有列的架构。 下表介绍了这些列。  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|字符串|表的编录。|  
|TABLE_SCHEMA|字符串|包含表的架构。|  
|TABLE_NAME|字符串|表名。|  
|COLUMN_NAME|字符串|列名称。|  
|ORDINAL_POSITION|Int32|列标识号。|  
|COLUMN_DEFAULT|字符串|列的默认值。|  
|IS_NULLABLE|字符串|列的为空性。 如果此列允许 NULL，此列将返回 YES。 否则，返回 NO。|  
|DATA_TYPE|字符串|系统提供的数据类型。|  
|CHARACTER_MAXIMUM_LENGTH|Int32|二进制数据、字符数据或文本和图像数据的最大长度（字符）。 否则，返回 NULL。|  
|CHARACTER_OCTET_LENGTH|Int32|二进制数据、字符数据或文本和图像数据的最大长度（字节）。 否则，返回 NULL。|  
|NUMERIC_PRECISION|Unsigned Byte|近似数字数据、精确数字数据、整数数据或货币数据的精度。 否则，返回 NULL。|  
|NUMERIC_PRECISION_RADIX|Int16|近似数字数据、精确数字数据、整数数据或货币数据的精度基数。 否则，返回 NULL。|  
|NUMERIC_SCALE|Int32|近似数字数据、精确数字数据、整数数据或货币数据的小数位数。 否则，返回 NULL。|  
|DATETIME_PRECISION|Int16|datetime 及 SQL-92 interval 数据类型的子类型代码。 对于其他数据类型，返回 NULL。|  
|CHARACTER_SET_CATALOG|字符串|如果列为字符数据或文本数据类型，则返回 master，指示字符集所处的数据库。 否则，返回 NULL。|  
|CHARACTER_SET_SCHEMA|字符串|始终返回 NULL。|  
|CHARACTER_SET_NAME|字符串|如果此列为字符数据或文本数据类型，则返回字符集的唯一名称。 否则，返回 NULL。|  
|COLLATION_CATALOG|字符串|如果列为字符数据或文本数据类型，则返回 master，指示定义分页的数据库。 否则，此列为 NULL。|  
|IS_FILESTREAM|字符串|YES       FILESTREAM    <br /><br /> NO，如果列不具有 FILESTREAM 属性。|  
|IS_SPARSE|字符串|YES，如果列是稀疏列。<br /><br /> NO，如果列不是稀疏列。|  
|IS_COLUMN_SET|字符串|YES，如果列是一个列集列。<br /><br /> NO            |  

## <a name="users"></a>用户  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|uid|Int16|用户 ID，在此数据库中是唯一的。 1 为数据库所有者。|  
|user_name|字符串|在此数据库中唯一的用户名和组名。|  
|createdate|DateTime|帐户的添加日期。|  
|updatedate|DateTime|帐户的上次更改日期。| 

## <a name="views"></a>视图  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|字符串|视图的编录。|  
|TABLE_SCHEMA|字符串|包含视图的架构。|  
|TABLE_NAME|字符串|视图名。|  
|CHECK_OPTION|字符串|WITH CHECK OPTION 的类型。 如果原始视图使用 WITH CHECK OPTION 创建，则为 CASCADE。 否则，返回 NONE。|  
|IS_UPDATABLE|字符串|指定视图是否可更新。 始终返回 NO。|  

## <a name="viewcolumns"></a>ViewColumns  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|VIEW_CATALOG|字符串|视图的编录。|  
|VIEW_SCHEMA|字符串|包含视图的架构。|  
|VIEW_NAME|字符串|视图名。|  
|TABLE_CATALOG|字符串|与此视图关联的表的编录。|  
|TABLE_SCHEMA|字符串|包含与此视图关联的表的架构。|  
|TABLE_NAME|字符串|与此视图关联的表的名称。 基表。|  
|COLUMN_NAME|字符串|列名称。|  

## <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|ColumnName|数据类型|说明|  
|----------------|--------------|-----------------|  
|assembly_name|字符串|程序集文件的名称。|  
|udt_name|字符串|程序集的类名。|  
|version_major|对象|主版本号。|  
|version_minor|对象|次版本号。|  
|version_build|对象|Build 号。|  
|version_revision|对象|修订号。|  
|culture_info|对象|与此 UDT 关联的区域性信息。|  
|public_key|对象|此程序集使用的公钥。|  
|is_fixed_length|Boolean|指定类型的长度是否始终与 max_length 相同。|  
|max_length|Int16|类型的最大长度（字节数）。|  
|Create_Date|DateTime|创建/注册程序集的日期。|  
|Permission_set_desc|字符串|程序集的权限集/安全级别的友好名称。|  

## <a name="see-also"></a>另请参阅

- [检索数据库架构信息](retrieving-database-schema-information.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
