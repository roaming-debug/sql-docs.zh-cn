---
description: 'SQL Server Native Client (OLE DB 中的大型 CLR User-Defined 类型) '
title: 大型 CLR 用户定义类型 (OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- large CLR user-defined types [OLE DB]
ms.assetid: 4bf12058-0534-42ca-a5ba-b1c23b24d90f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95552d965ab09f34c134ad82dd7d84966ee65fd6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467598"
---
# <a name="large-clr-user-defined-types-in-sql-server-native-client-ole-db"></a>SQL Server Native Client (OLE DB 中的大型 CLR User-Defined 类型) 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中为支持大型公共语言运行时 (CLR) 用户定义类型 (UDT) 而对 OLE DB 进行的更改。  
  
 有关 Native Client 中对大型 CLR Udt 的支持的详细信息 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，请参阅 [大型 Clr User-Defined 类型](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)。 有关示例，请参阅[使用大型 CLR UDT (OLE DB)](../../../relational-databases/native-client-ole-db-how-to/use-large-clr-udts-ole-db.md)。  
  
## <a name="data-format"></a>数据格式  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用 ~0 表示其大小对于大型对象 (LOB) 类型不受限制的值的长度。 ~0 还表示大于 8,000 个字节的 CLR UDT 的大小。  
  
 下表显示了参数和行集中的数据类型映射：  
  
|SQL Server 数据类型|OLE DB 数据类型|内存布局|值|  
|--------------------------|----------------------|-------------------|-----------|  
|CLR UDT|DBTYPE_UDT|BYTE[]（字节数组\)|132 (oledb.h)|  
  
 UDT 值表示为字节数组。 支持与十六进制字符串之间的转换。 文字值表示为带有“0x”前缀的十六进制字符串。 十六进制字符串是以 16 为基数的二进制数据的文本表示形式。 例如，从服务器类型 varbinary(10) 转换为 DBTYPE_STR，这将产生一个 20 个字符的十六进制表示形式，其中每对字符表示一个字节  。  
  
## <a name="parameter-properties"></a>参数属性  
 DBPROPSET_SQLSERVERPARAMETER 属性集通过 OLE DB 支持 UDT。 有关详细信息，请参阅[使用用户定义类型](~/relational-databases/native-client/features/using-user-defined-types.md)。  
  
## <a name="column-properties"></a>列属性  
 DBPROPSET_SQLSERVERCOLUMN 属性集通过 OLE DB 支持表的创建。 有关详细信息，请参阅[使用用户定义类型](~/relational-databases/native-client/features/using-user-defined-types.md)。  
  
## <a name="data-type-mapping-in-itabledefinitioncreatetable"></a>ITableDefinition::CreateTable 中的数据类型映射  
 需要 UDT 列时，在 ITableDefinition::CreateTable 所用的 DBCOLUMNDESC 结构中使用下列信息  ：  
  
|OLE DB 数据类型 (wType  )|pwszTypeName |SQL Server 数据类型|rgPropertySets |  
|----------------------------------|--------------------|--------------------------|----------------------|  
|DBTYPE_UDT|忽略|UDT|必须包括 DBPROPSET_SQLSERVERCOLUMN 属性集。|  
  
## <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 通过 prgParamInfo 在 DBPARAMINFO 结构中返回的信息如下所示  ：  
  
|参数类型|wType |ulParamSize |bPrecision |bScale |dwFlags  DBPARAMFLAGS_ISLONG|  
|--------------------|-------------|-------------------|------------------|--------------|------------------------------------|  
|DBTYPE_UDT<br /><br /> （长度小于或等于 8,000 个字节）|"DBTYPE_UDT"|*n*|未定义|未定义|clear|  
|DBTYPE_UDT<br /><br /> （长度大于 8000 个字节）|"DBTYPE_UDT"|~0|未定义|未定义|set|  
  
## <a name="icommandwithparameterssetparameterinfo"></a>ICommandWithParameters::SetParameterInfo  
 在 DBPARAMBINDINFO 结构中提供的信息必须符合以下规定：  
  
|参数类型|pwszDataSourceType |ulParamSize |bPrecision |bScale |dwFlags  DBPARAMFLAGS_ISLONG|  
|--------------------|--------------------------|-------------------|------------------|--------------|------------------------------------|  
|DBTYPE_UDT<br /><br /> （长度小于或等于 8,000 个字节）|DBTYPE_UDT|*n*|已忽略|已忽略|如果将使用 DBTYPE_IUNKNOWN 传递参数，则必须进行设置。|  
|DBTYPE_UDT<br /><br /> （长度大于 8000 个字节）|DBTYPE_UDT|~0|已忽略|已忽略|已忽略|  
  
## <a name="isscommandwithparameters"></a>ISSCommandWithParameters  
 应用程序使用 ISSCommandWithParameters 获取和设置在“参数属性”部分中定义的参数属性  。  
  
## <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 返回的列如下所示：  
  
|列类型|DBCOLUMN_TYPE|DBCOLUMN_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE|DBCOLUMN_FLAGS_ISLONG|DBCOLUMNS_ISSEARCHABLE|DBCOLUMN_OCTETLENGTH|  
|-----------------|--------------------|--------------------------|-------------------------|---------------------|-----------------------------|-----------------------------|---------------------------|  
|DBTYPE_UDT<br /><br /> （长度小于或等于 8,000 个字节）|DBTYPE_UDT|*n*|Null|Null|Clear|DB_ALL_EXCEPT_LIKE|n|  
|DBTYPE_UDT<br /><br /> （长度大于 8000 个字节）|DBTYPE_UDT|~0|Null|Null|Set|DB_ALL_EXCEPT_LIKE|0|  
  
 对于 UDT，还会定义以下列：  
  
|列标识符|类型|说明|  
|-----------------------|----------|-----------------|  
|DBCOLUMN_UDT_CATALOGNAME|DBTYPE_WSTR|对于 UDT 列，为在其中定义了 UDT 的目录的名称。|  
|DBCOLUMN_UDT_SCHEMANAME|DBTYPE_WSTR|对于 UDT 列，为在其中定义了 UDT 的架构的名称。|  
|DBCOLUMN_UDT_NAME|DBTYPE_WSTR|对于 UDT 列，为仅由一个部分组成的 UDT 名称。|  
|DBCOLUMN_ASSEMBLY_TYPENAME|DBTYPE_WSTR|对于 UDT 列，是 UDT 的完整类型名称。 通过程序集类型的完全限定名称，可以使用 Type.GetType 方法实例化该类型的对象。|  
  
## <a name="icolumnsinfogetcolumninfo"></a>IColumnsInfo::GetColumnInfo  
 在 DBCOLUMNINFO 结构中返回的信息如下所示：  
  
|参数类型|wType |ulColumnSize |bPrecision |bScale |dwFlags <br /><br /> DBCOLUMNFLAGS_ISLONG|  
|--------------------|-------------|--------------------|------------------|--------------|-----------------------------------------|  
|DBTYPE_UDT<br /><br /> （长度小于或等于 8,000 个字节）|DBTYPE_UDT|*n*|~0|~0|Clear|  
|DBTYPE_UDT<br /><br /> （长度大于 8000 个字节）|DBTYPE_UDT|~0|~0|~0|Set|  
  
## <a name="columns-rowset-schema-rowsets"></a>COLUMNS 行集（架构行集）  
 对于 UDT 类型，返回以下列值：  
  
|列类型|DATA_TYPE|COLUMN_FLAGS, DBCOLUMFLAGS_ISLONG|CHARACTER_OCTET_LENGTH|  
|-----------------|----------------|-----------------------------------------|------------------------------|  
|DBTYPE_UDT<br /><br /> （长度小于或等于 8,000 个字节）|DBTYPE_UDT|Clear|*n*|  
|DBTYPE_UDT<br /><br /> （长度大于 8000 个字节）|DBTYPE_UDT|Set|0|  
  
 对于 UDT，还会定义以下列：  
  
|列标识符|类型|说明|  
|-----------------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|对于 UDT 列，为在其中定义了 UDT 的目录的名称。|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|对于 UDT 列，为在其中定义了 UDT 的架构的名称。|  
|SS_UDT_NAME|DBTYPE_WSTR|对于 UDT 列，为仅由一个部分组成的 UDT 名称。|  
|SS_ASSEMBLY_TYPENAME|DBTYPE_WSTR|对于 UDT 列，为 UDT 的完整类型名称。 通过程序集类型的完全限定名称，可以使用 Type.GetType 方法实例化该类型的对象。|  
  
 对于 PROCEDURE_PARAMETERS 行集，DATA_TYPE 包含与 COLUMNS 架构行集相同的值，并且 TYPE_NAME 包含 UDT。 对于该行集，也会定义相同的附加列。  
  
 用户定义类型不会出现在 PROVIDER_TYPES 架构行集中。  
  
## <a name="bindings-and-conversions"></a>绑定和转换  
  
|绑定数据类型|UDT 到服务器|非 UDT 到服务器|UDT 来自服务器|非 UDT 来自服务器|  
|----------------------|-------------------|------------------------|---------------------|--------------------------|  
|DBTYPE_UDT|支持 (5)|错误 (1)|支持 (5)|错误 (4)|  
|DBTYPE_BYTES|支持 (5)|空值|支持 (5)|空值|  
|DBTYPE_WSTR|支持 (2)、(5)|空值|支持 (3)、(5)、(6)|空值|  
|DBTYPE_BSTR|支持 (2)、(5)|空值|支持 (3)、(5)|空值|  
|DBTYPE_STR|支持 (2)、(5)|空值|支持 (3)、(5)|空值|  
|DBTYPE_IUNKNOWN|支持 (6)|空值|支持 (6)|空值|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|支持 (5)|空值|支持 (3)、(5)|空值|  
|DBTYPE_VARIANT (VT_BSTR)|支持 (2)、(5)|空值|空值|空值|  
  
### <a name="key-to-symbols"></a>符号含义  
  
|符号|含义|  
|------------|-------------|  
|1|如果使用 ICommandWithParameters::SetParameterInfo 指定了非 DBTYPE_UDT 的服务器类型，而取值函数类型为 DBTYPE_UDT，则执行该语句时将出错  。  将出现 DB_E_ERRORSOCCURRED 错误，参数状态将为 DBSTATUS_E_BADACCESSOR。<br /><br /> 如为类型不是 UDT 的服务器参数指定 UDT 类型的参数，则会出现错误。|  
|2|数据从十六进制字符串转换为二进制数据。|  
|3|数据从二进制数据转换为十六进制字符串。|  
|4|在使用 CreateAccessor 或 GetNextRows 时进行验证   。 错误为 DB_E_ERRORSOCCURRED 错误。 绑定状态设置为 DBBINDSTATUS_UNSUPPORTEDCONVERSION。|  
|5|可能会使用 BY_REF。|  
|6|UDT 参数可以绑定为 DBBINDING 中的 DBTYPE_IUNKNOWN。 如果绑定到 DBTYPE_IUNKNOWN，则表示应用程序要使用 ISequentialStream 接口将数据作为流进行处理。 当使用者在绑定中将 *wType* 指定为类型 DBTYPE_IUNKNOWN，并且该存储过程的相应列或输出参数为 UDT 时，SQL Server Native Client 将返回 ISequentialStream。 对于输入参数，SQL Server Native Client 将查询 ISequentialStream 接口的。<br /><br /> 如果是大型 UDT，可以选择不绑定 UDT 数据的长度，而是使用 DBTYPE_IUNKNOWN 绑定。 但是，对于小型 UDT，必须绑定长度。 如果满足以下一个或多个条件，可以将 DBTYPE_UDT 参数指定为大型 UDT：<br />ulParamParamSize  为 ~0。<br />在 DBPARAMBINDINFO 结构中设置了 DBPARAMFLAGS_ISLONG。<br /><br /> 对于行数据，仅允许对大型 UDT 进行 DBTYPE_IUNKNOWN 绑定。 可以通过对 Rowset 或 Command 对象的 IColumnsInfo 接口使用 IColumnsInfo::GetColumnInfo 方法来了解列是否为大型 UDT 类型。 如果满足以下一个或多个条件，DBTYPE_UDT 列即为大型 UDT 列：<br />DBCOLUMNFLAGS_ISLONG 标记是在 DBCOLUMNINFO 结构的 dwFlags 成员上设置的  。 <br />DBCOLUMNINFO 的 ulColumnSize  成员为 ~0。|  
  
 可以绑定 DBTYPE_NULL 和 DBTYPE_EMPTY 用于输入参数，但不能用于输出参数或结果。 当绑定用于输入参数时，对于 DBTYPE_NULL，状态必须设置为 DBSTATUS_S_ISNULL，对于 DBTYPE_EMPTY 则必须设置为 DBSTATUS_S_DEFAULT。 DBTYPE_BYREF 不能与 DBTYPE_NULL 或 DBTYPE_EMPTY 一起使用。  
  
 DBTYPE_UDT 还可以转换为 DBTYPE_EMPTY 和 DBTYPE_NULL。 但是，DBTYPE_NULL 和 DBTYPE_EMPTY 无法转换为 DBTYPE_UDT。 这与 DBTYPE_BYTES 的情况是一致的。 ISSCommandWithParameters 用于将 UDT 处理为参数  。  
  
 OLE DB 核心服务 (IDataConvert) 提供的数据转换不适用于 DBTYPE_UDT  。  
  
 不支持其他绑定。  
  
## <a name="comparability-for-irowsetfind"></a>IRowsetFind 的可比性  
 对于 UDT 类型，仅支持进行以下比较：  
  
-   EQ  
  
-   NE  
  
-   IGNORE  
  
 如果试图进行其他任何比较，将返回 DB_E_BADCOMPAREOP 错误。  
  
## <a name="bcp-support-for-udts"></a>对 UDT 的 BCP 支持  
 UDT 值只能够作为字符值或二进制值导入和导出。  
  
## <a name="down-level-client-behavior-for-udts"></a>UDT 的下级客户端行为  
 对于下级客户端，UDT 会进行类型映射，如下所示：  
  
|客户端版本|DBTYPE_UDT<br /><br /> （长度小于或等于 8,000 个字节）|DBTYPE_UDT<br /><br /> （长度大于 8000 个字节）|  
|--------------------|------------------------------------------------------------------|---------------------------------------------------------|  
|SQL Server 2005|UDT|varbinary(max)|  
|SQL Server 2008 和更高版本|UDT|UDT|  
  
 如果 DataTypeCompatibility (SSPROP_INIT_DATATYPECOMPATIBILITY) 设置为“80”，则大型 UDT 类型按相同方式向客户端和下级客户端进行显示  。  
  
## <a name="see-also"></a>另请参阅  
 [大型 CLR 用户定义类型](~/relational-databases/native-client/features/large-clr-user-defined-types.md)  
  
  

