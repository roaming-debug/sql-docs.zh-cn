---
title: SSVARIANT 结构（OLE DB 驱动程序）
description: OLE DB Driver for SQL Server 中的 SSVARIANT 结构
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SSVARIANT
helpviewer_keywords:
- SSVARIANT struct
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7095942d695c57667e47d32bb6c60a97c51fbd59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752017"
---
# <a name="ssvariant-structure"></a>SSVARIANT 结构
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  SSVARIANT 是在 msoledbsql.h 中定义的，  ，它对应于 OLE DB Driver for SQL Server 中的 DBTYPE_SQLVARIANT 值。  
  
 SSVARIANT  是一个分类化的联合。 根据 vt 成员的值，使用者可以确定要读取的成员。 vt 值与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型相对应。 因此，SSVARIANT 结构可以具有任何 SQL Server 类型  。 有关标准 OLE DB 类型的数据结构的详细信息，请参阅[类型指示器](/previous-versions/windows/desktop/ms711251(v=vs.85))。  
  
## <a name="remarks"></a>备注  
 如果 DataTypeCompat==80，几个 SSVARIANT 子类型都将成为字符串  。 例如，以下 vt 值将在 SSVARIANT 中显示为 VT_SS_WVARSTRING  ：  
  
-   VT_SS_DATETIMEOFFSET  
  
-   VT_SS_DATETIME2  
  
-   VT_SS_TIME2  
  
-   VT_SS_DATE  
  
 如果 DateTypeCompat == 0，这些类型将按它们原来的形式显示。  
  
 有关 SSPROP_INIT_DATATYPECOMPATIBILITY 的详细信息，请参阅[将连接字符串关键字用于 OLE DB Driver for SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)。  
  
 msoledbsql.h 文件包含变量访问宏，可以简化 SSVARIANT 结构中成员类型的取消引用操作  。 例如 V_SS_DATETIMEOFFSET，您可以按如下方式使用它：  
  
```  
memcpy(&V_SS_DATETIMEOFFSET(pssVar).tsoDateTimeOffsetVal, pDTO, cbNative);  
V_SS_DATETIMEOFFSET(pssVar).bScale = bScale;  
```  
  
 有关 SSVARIANT  结构中每个成员的完整访问宏集，请参考 msoledbsql.h 文件。  
  
 下表介绍了 SSVARIANT 结构的成员  ：  
  
|成员|OLE DB 类型指示器|OLE DB C 数据类型|vt 值|注释|  
|------------|---------------------------|------------------------|--------------|--------------|  
|vt|SSVARTYPE|||指定 SSVARIANT 结构中包含的值类型  。|  
|bTinyIntVal|DBTYPE_UI1|**BYTE**|**VT_SS_UI1**|支持 tinyint  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|sShortIntVal|DBTYPE_I2|**SHORT**|**VT_SS_I2**|支持 smallint  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|lIntVal|DBTYPE_I4|**LONG**|**VT_SS_I4**|支持 int  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|llBigIntVal|DBTYPE_I8|**LARGE_INTEGER**|**VT_SS_I8**|支持 bigint  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|fltRealVal|DBTYPE_R4|**float**|**VT_SS_R4**|支持 real  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|dblFloatVal|DBTYPE_R8|**double**|**VT_SS_R8**|支持 float  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|cyMoneyVal|DBTYPE_CY|**LARGE_INTEGER**|**VT_SS_MONEY VT_SS_SMALLMONEY**|支持 money  和 smallmoney  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|fBitVal|DBTYPE_BOOL|**VARIANT_BOOL**|**VT_SS_BIT**|支持 bit  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|rgbGuidVal|DBTYPE_GUID|**GUID**|**VT_SS_GUID**|支持 uniqueidentifier  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|numNumericVal|DBTYPE_NUMERIC|**DB_NUMERIC**|**VT_SS_NUMERIC**|支持 numeric  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|dDateVal|DBTYPE_DATE|**DBDATE**|**VT_SS_DATE**|支持 date **数据类型**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。|  
|tsDateTimeVal|DBTYPE_DBTIMESTAMP|**DBTIMESTAMP**|**VT_SS_SMALLDATETIME VT_SS_DATETIME VT_SS_DATETIME2**|支持 smalldatetime  、datetime  和 datetime2  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。|  
|Time2Val|DBTYPE_DBTIME2|**DBTIME2**|**VT_SS_TIME2**|支持 time  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> 包括以下成员：<br /><br /> tTime2Val  (DBTIME2  )<br /><br /> bScale  (BYTE  ) 指定 tTime2Val  值的小数位数。|  
|DateTimeVal|DBTYPE_DBTIMESTAMP|**DBTIMESTAMP**|**VT_SS_DATETIME2**|支持 datetime2  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> 包括以下成员：<br /><br /> tsDataTimeVal  (DBTIMESTAMP)<br /><br /> bScale  (BYTE  ) 指定 tsDataTimeVal  值的小数位数。|  
|DateTimeOffsetVal|DBTYPE_DBTIMESTAMPOFSET|**DBTIMESTAMPOFFSET**|**VT_SS_DATETIMEOFFSET**|支持 datetimeoffset  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> 包括以下成员：<br /><br /> tsoDateTimeOffsetVal  (DBTIMESTAMPOFFSET  )<br /><br /> bScale  (BYTE  ) 指定 tsoDateTimeOffsetVal  值的小数位数。|  
|NCharVal|没有对应的 OLE DB 类型指示器。|**struct _NCharVal**|**VT_SS_WVARSTRING、**<br /><br /> **VT_SS_WSTRING**|支持 nchar  和 nvarchar  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> 包括以下成员：<br /><br /> sActualLength  (SHORT  ) 指定 pwchNCharVal  指向的字符串的实际长度。 不包括尾零。<br /><br /> sMaxLength  (SHORT  ) 指定 pwchNCharVal  指向的字符串的最大长度。<br /><br /> 指向字符串的 pwchNCharVal  (WCHAR  \*) 指针。<br /><br /> rgbReserved (**BYTE[5]** ) 指定排序规则信息。<br /><br />  不使用的成员：dwReserved 和 pwchReserved。|  
|CharVal|没有对应的 OLE DB 类型指示器。|**struct _CharVal**|**VT_SS_STRING、**<br /><br /> **VT_SS_VARSTRING**|支持 char 和 varchar[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> 包括以下成员：<br /><br /> sActualLength (SHORT) 指定 pchCharVal 指向的字符串的实际长度。 不包括尾零。<br /><br /> sMaxLength (SHORT) 指定 pchCharVal 指向的字符串的最大长度。<br /><br /> 指向字符串的 pchCharVal (CHAR \*) 指针。<br /><br /> rgbReserved (**BYTE[5]** ) 指定排序规则信息。<br /><br /> 不使用的成员：<br /><br />  dwReserved 和 pwchReserved。|  
|BinaryVal|没有对应的 OLE DB 类型指示器。|**struct _BinaryVal**|**VT_SS_VARBINARY、**<br /><br /> **VT_SS_BINARY**|支持 binary 和 varbinary[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> 包括以下成员：<br /><br /> sActualLength (SHORT) 指定 prgbBinaryVal 指向的数据的实际长度。<br /><br /> sMaxLength (SHORT) 指定 prgbBinaryVal 指向的数据的最大长度。<br /><br /> 指向二进制数据的 prgbBinaryVal (BYTE \*) 指针。<br /><br /> 不使用的成员：dwReserved。|  
|UnknownType|不使用|不使用|不使用|不使用|  
|BLOBType|不使用|不使用|不使用|不使用|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="known-issues"></a>已知问题
### <a name="possible-narrow-string-data-corruption"></a>可能的窄字符串数据损坏
在 OLE DB 驱动程序版本 18.4 之前，如果满足以下所有条件，则插入到 `sql_variant` 列可能导致服务器上的数据损坏：
- 客户端计算机代码页与数据库排序规则代码页不匹配。
- 要插入的客户端缓冲区包含客户端代码页中编码的非 ASCII 窄字符串字符。
- 以下任一条件成立：
  - `DBPARAMBINDINFO` 结构中描述与 `sql_variant` 列对应的参数的 `pwszDataSourceType` 字段设置为 `L"DBTYPE_SQLVARIANT"`、`L"DBTYPE_VARIANT"` 或 `L"sql_variant"`。 有关详细信息，请参阅：[ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)).

    *or*
  - 准备了用于插入的参数化 SQL 查询。

更具体地说，OLE DB 驱动程序在插入数据之前不会将其转换为数据库排序规则代码页。 但是，驱动程序错误地向服务器指出在数据库排序规则代码页中对数据进行了编码。 此行为导致数据与其存储在 `sql_variant` 列的相应代码页不匹配。

同样在检索相同的值时，OLE DB 驱动程序未将字符串转换为客户端代码页。 不过，由于插入的数据已位于客户端代码页（参见上述段落），客户端应用程序可以正确地解读数据。 尽管如此，使用其他驱动程序的应用程序将以损坏的格式检索这些值。 出现损坏的原因是其他驱动程序解读了数据库排序规则代码页中的字符串，并尝试将其转换为客户端代码页。

从版本 18.4 开始，OLE DB 驱动程序在插入之前将窄字符串转换为数据库排序规则代码页。 同样，驱动程序在检索时将数据转换回客户端代码页。 因此，依赖上述 bug 的客户端应用程序在检索使用早期版本 OLE DB 驱动程序插入的数据时可能遇到问题。 下面的[恢复过程](#recovery-procedure)旨在提供解决这些问题的指导。

### <a name="recovery-procedure"></a>恢复过程
> [!IMPORTANT]  
> 执行以下恢复步骤之前，确保备份现有数据。

如果切换到 OLE DB 驱动程序 18.4 版后应用程序从 `sql_variant` 列检索数据时遇到问题，则需要修改损坏的数据使其与存储数据的数据库具有相同的排序规则。 以下脚本可用于从 `sql_variant` 列中恢复单个值。 该脚本是一个模板，需要对其进行调整以满足具体需要。

> [!IMPORTANT]  
> 由于未存储数据的原始代码页，需要告知服务器最初是如何对数据进行编码的。 为此，执行脚本的数据库所具有的代码页应与最初插入数据的客户端的代码页相同。 例如，如果从使用代码页 `932` 配置的客户端插入损坏的数据，则需要在具有日语排序规则（例如 `Japanese_XJIS_100_CS_AI`）的数据库上下文中执行以下脚本。

```sql
/*
    Description:
        Template that can be used to recover the corrupted value inserted into the sql_variant column.

    Scenario:
        The database is named [YourDatabase] and it contains a table named [YourTable], which contains the corrupted value.
        Schema is named [dbo].
        The corrupted value is stored in a column of type sql_variant named [YourColumn].
        The corrupted value is sql_variant of BaseType char. For details on sql_variant properties, see:
            https://docs.microsoft.com/sql/t-sql/functions/sql-variant-property-transact-sql
*/

-- Base type in sql_variant can hold a maximum of 8000 bytes
-- For details see: 
--  https://docs.microsoft.com/sql/t-sql/data-types/sql-variant-transact-sql#remarks
DECLARE @bin VARBINARY(8000)

-- In the following lines we convert the sql_variant base type to binary.
-- <FilterExpression>
--      Is a placeholder and must be replaced with an expression that filters a single corrupted value to be recovered.
--      Therefore, the expression must result in a single value being returned only.
SET @bin = (SELECT CAST([YourColumn] AS VARBINARY(8000)) FROM [YourDatabase].[dbo].[YourTable] WHERE <FilterExpression>)

-- In the following lines we store the binary value in char(59) (a fixed-size character data type).
-- IMPORTANT NOTE: 
--      This example assumes the corrupted sql_variant's base type is char(59).
--      You MUST adjust the type (that is, char/varchar) and size to match your scenario exactly.
DECLARE @char CHAR(59)
SET @char = CAST((@bin) AS CHAR(59))
DECLARE @sqlvariant sql_variant

-- The following lines recover the corrupted value by translating the value to the collation of the database.
-- <DBCollation>
--      Must be replaced with the collation (for example, Latin1_General_100_CI_AS_SC_UTF8) of the database holding the data.
SET @sqlvariant = @char collate <DBCollation>

-- Finally, we update the corrupted value with the recovered value.
-- "<FilterExpression>"
--      Is a placeholder and must be replaced with an expression that filters a single corrupted value to be recovered.
--      Therefore, the expression must result in a single value being returned only.
UPDATE [YourDatabase].[dbo].[YourTable] SET [YourColumn] = @sqlvariant WHERE <FilterExpression>
```

## <a name="see-also"></a>另请参阅  
 [数据类型 (OLE DB)](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  
