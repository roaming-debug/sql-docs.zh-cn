---
description: 在 SQL Server Native Client 中使用 User-Defined 类型
title: 使用用户定义类型 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- DBPROPSET_DATASOURCEINFO property set
- UDTs [SQL Server Native Client]
- user-defined types [SQL Server], SQL Server Native Client
- SQL Server Native Client OLE DB provider, user-defined types
- DBPROPSET_SQLSERVERPARAMETER property set
- IColumnsRowset interface
- SQLNCLI, user-defined types
- SQL Server Native Client, user-defined types
- data access [SQL Server Native Client], user-defined types
- ISSCommandWithParameters interface
ms.assetid: e15d8169-3517-4323-9c9e-0f5c34aff7df
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2808e712491271fe5738ba4d20ad3e7e2133a451
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461948"
---
# <a name="using-user-defined-types-in-sql-server-native-client"></a>在 SQL Server Native Client 中使用 User-Defined 类型
[!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md](../../../includes/appliesto-ss-asdb-xxxx-pdw-md.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 介绍了用户定义类型 (UDT)。 UDT 可将对象和自定义数据结构存储在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中，从而扩展了 SQL 类型系统。 UDT 可以包含多种数据类型并且可具有行为，这使它们不同于由单一 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 系统数据类型构成的传统别名数据类型。 使用生成可验证代码的 .NET 公共语言运行时 (CLR) 所支持的任何语言都可以定义 UDT， 这些语言包括 Microsoft Visual C#<sup>®</sup> 和 Visual Basic<sup>®</sup> .NET。 数据公开为 .NET 类或结构的字段和属性，行为则由类或结构的方法来定义。  
  
 UDT 可用作表的列定义、[!INCLUDE[tsql](../../../includes/tsql-md.md)] 批处理中的变量，还可用作 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 函数或存储过程的参数。  
  
## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持将 udt 作为二进制类型和元数据信息，这使你可以将 udt 作为对象进行管理。 UDT 列公开为 DBTYPE_UDT，且其元数据通过核心 OLE DB 接口 IColumnRowset 和新增的 [ISSCommandWithParameters](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md) 接口公开。  
  
> [!NOTE]  
>  IRowsetFind::FindNextRow 方法不适用于 UDT 数据类型  。 如果将 UDT 用作搜索列类型，则返回 DB_E_BADCOMPAREOP。  
  
### <a name="data-bindings-and-coercions"></a>数据绑定和强制  
 下表说明将所列数据类型与某一 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] UDT 一同使用时出现的绑定和强制。 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] DBTYPE_UDT 的 Native Client OLE DB 提供程序公开 UDT 列。 可以通过适当的架构行集获取元数据，这样即可将您自行定义的类型作为对象来管理。  
  
|数据类型|到服务器<br /><br /> **UDT**|到服务器<br /><br /> **non-UDT**|从服务器<br /><br /> **UDT**|从服务器<br /><br /> **non-UDT**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_UDT|支持<sup>6</sup>|错误<sup>1</sup>|支持<sup>6</sup>|错误<sup>5</sup>|  
|DBTYPE_BYTES|支持<sup>6</sup>|N/A<sup>2</sup>|支持<sup>6</sup>|N/A<sup>2</sup>|  
|DBTYPE_WSTR|支持<sup>3,6</sup>|N/A<sup>2</sup>|支持<sup>4,6</sup>|N/A<sup>2</sup>|  
|DBTYPE_BSTR|支持<sup>3,6</sup>|N/A<sup>2</sup>|支持<sup>4</sup>|N/A<sup>2</sup>|  
|DBTYPE_STR|支持<sup>3,6</sup>|N/A<sup>2</sup>|支持<sup>4,6</sup>|N/A<sup>2</sup>|  
|DBTYPE_IUNKNOWN|不支持|N/A<sup>2</sup>|不支持|N/A<sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|支持<sup>6</sup>|N/A<sup>2</sup>|支持<sup>4</sup>|N/A<sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|支持<sup>3,6</sup>|N/A<sup>2</sup>|空值|N/A<sup>2</sup>|  
  
 <sup>1</sup>如果使用 ICommandWithParameters::SetParameterInfo 指定 DBTYPE_UDT 之外的服务器类型，而取值函数类型为 DBTYPE_UDT，则执行该语句时将出错（DB_E_ERRORSOCCURRED；参数状态为 DBSTATUS_E_BADACCESSOR）  。 否则，数据发送到服务器，但服务器会返回错误，指明不存在将 UDT 转换为参数的数据类型的隐式转换。  
  
 <sup>2</sup>超出了本主题的范围。  
  
 <sup>3</sup>从十六进制字符串转换为二进制数据。  
  
 <sup>4</sup>从二进制数据转换为十六进制字符串。  
  
 <sup>5</sup>可能在创建取值函数时或在提取时执行验证，错误为 DB_E_ERRORSOCCURRED，且绑定状态设置为 DBBINDSTATUS_UNSUPPORTEDCONVERSION。  
  
 <sup>6</sup>可使用 BY_REF。  
  
 可绑定 DBTYPE_NULL 和 DBTYPE_EMPTY 用于输入参数，但不能用于输出参数或结果。 为输入参数进行绑定时，状态必须设置为 DBSTATUS_S_ISNULL 或 DBSTATUS_S_DEFAULT。  
  
 还可以将 DBTYPE_UDT 转换为 DBTYPE_EMPTY 和 DBTYPE_NULL，但无法将 DBTYPE_NULL 和 DBTYPE_EMPTY 转换为 DBTYPE_UDT。 这与 DBTYPE_BYTES 的情况是一致的。  
  
> [!NOTE]  
>  借助继承自 ICommandWithParameters 的新接口 ISSCommandWithParameters，可将 UDT 当作参数处理   。 应用程序必须使用此接口为 UDT 参数至少设置 DBPROPSET_SQLSERVERPARAMETER 属性集的 SSPROP_PARAM_UDT_NAME。 否则，ICommand::Execute 将返回 DB_E_ERRORSOCCURRED  。 此接口和属性集将在本主题的下文中介绍。  
  
 如果在某列中插入用户定义类型，而该列的大小不足以包含该类型的所有数据，则 ICommand::Execute 将返回 S_OK，且状态为 DB_E_ERRORSOCCURRED  。  
  
 OLE DB 核心服务 (IDataConvert) 提供的数据转换不适用于 DBTYPE_UDT  。 不支持其他绑定。  
  
### <a name="ole-db-rowset-additions-and-changes"></a>OLE DB 行集的添加和更改内容  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将新值或更改添加到许多核心 OLE DB 架构行集。  
  
#### <a name="the-procedure_parameters-schema-rowset"></a>PROCEDURE_PARAMETERS 架构行集  
 在 PROCEDURE_PARAMETERS 架构行集中添加了以下内容。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|由三部分组成的名称标识符。|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|由三部分组成的名称标识符。|  
|SS_UDT_NAME|DBTYPE_WSTR|由三部分组成的名称标识符。|  
|SS_UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|程序集限定名，其中包括 CLR 引用所需的类型名称和所有程序集标识。|  
  
#### <a name="the-sql_assemblies-schema-rowset"></a>SQL_ASSEMBLIES 架构行集  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序公开了一种特定于访问接口的新架构行集，用于描述已注册的 udt。 可以将 ASSEMBLY 服务器指定为 DBTYPE_WSTR，但它在行集中不存在。 如果未指定，行集将默认使用当前服务器。 下表中定义了 SQL_ASSEMBLIES 架构行集。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|ASSEMBLY_CATALOG|DBTYPE_WSTR|包含该类型的程序集的目录名称。|  
|ASSEMBLY_SCHEMA|DBTYPE_WSTR|包含该类型的程序集的架构名称或所有者名称。 尽管程序集的作用域为数据库而不是架构，但它们仍具有在此所反映的所有者。|  
|ASSEMBLY_NAME|DBTYPE_WSTR|包含该类型的程序集的名称。|  
|ASSEMBLY_ID|DBTYPE_UI4|包含该类型的程序集的对象 ID。|  
|PERMISSION_SET|DBTYPE_WSTR|指示程序集的访问范围的值。 这些值包括“SAFE”、“EXTERNAL_ACCESS”和“UNSAFE”。|  
|ASSEMBLY_BINARY|DBTYPE_BYTES|程序集的二进制表示形式。|  
  
#### <a name="the-sql_assemblies_-dependencies-schema-rowset"></a>SQL_ASSEMBLIES_ DEPENDENCIES 架构行集  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序公开了特定于提供程序的架构行集，用于描述指定服务器的程序集依赖项。 ASSEMBLY_SERVER 可由调用方指定为 DBTYPE_WSTR，但在该行集中不存在。 如果未指定，行集将默认使用当前服务器。 SQL_ASSEMBLY_DEPENDENCIES 架构行集在下表中定义。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|ASSEMBLY_CATALOG|DBTYPE_WSTR|包含该类型的程序集的目录名称。|  
|ASSEMBLY_SCHEMA|DBTYPE_WSTR|包含该类型的程序集的架构名称或所有者名称。 尽管程序集的作用域为数据库而不是架构，但它们仍具有在此所反映的所有者。|  
|ASSEMBLY_ID|DBTYPE_UI4|程序集的对象 ID。|  
|REFERENCED_ASSEMBLY_ID|DBTYPE_UI4|所引用的程序集的对象 ID。|  
  
#### <a name="the-sql_user_types-schema-rowset"></a>SQL_USER_TYPES 架构行集  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序公开新的架构行集，SQL_USER_TYPES，用于描述何时添加指定服务器的注册 Udt。 UDT_SERVER 必须由调用方指定为 DBTYPE_WSTR，但它在该行集中不存在。 在下表中定义 SQL_USER_TYPES 架构行集。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|UDT_CATALOGNAME|DBTYPE_WSTR|对于 UDT 列，此属性是一个字符串，它指定定义 UDT 的目录的名称。|  
|UDT_SCHEMANAME|DBTYPE_WSTR|对于 UDT 列，此属性是一个字符串，它指定定义 UDT 的架构的名称。|  
|UDT_NAME|DBTYPE_WSTR|包含 UDT 类的程序集的名称。|  
|UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|完整类型名称 (AQN) 包括带有命名空间前缀（如果适用）的类型名称。|  
  
#### <a name="the-columns-schema-rowset"></a>COLUMNS 架构行集  
 COLUMNS 架构行集添加了以下列。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|对于 UDT 列，此属性是一个字符串，它指定定义 UDT 的目录的名称。|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|对于 UDT 列，此属性是一个字符串，它指定定义 UDT 的架构的名称。|  
|SS_UDT_NAME|DBTYPE_WSTR|UDT 的名称|  
|SS_UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|完整类型名称 (AQN) 包括带有命名空间前缀（如果适用）的类型名称。|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>OLE DB 属性集的添加和更改内容  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 向许多 core OLE DB 属性集添加新值或更改。  
  
#### <a name="the-dbpropset_sqlserverparameter-property-set"></a>DBPROPSET_SQLSERVERPARAMETER 属性集  
 为了通过 OLE DB 支持 Udt， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 实现了新的 DBPROPSET_SQLSERVERPARAMETER 属性集，该属性集包含以下值。  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|SSPROP_PARAM_UDT_CATALOGNAME|DBTYPE_WSTR|由三部分组成的名称标识符。<br /><br /> 对于 UDT 参数，此属性是一个字符串，它指定定义用户定义类型的目录的名称。|  
|SSPROP_PARAM_UDT_SCHEMANAME|DBTYPE_WSTR|由三部分组成的名称标识符。<br /><br /> 对于 UDT 参数，此属性是一个字符串，它指定定义用户定义类型的架构的名称。|  
|SSPROP_PARAM_UDT_NAME|DBTYPE_WSTR|由三部分组成的名称标识符。<br /><br /> 对于 UDT 列，此属性是一个字符串，它指定仅由一个部分组成的用户定义类型名称。|  
  
 SSPROP_PARAM_UDT_NAME 是必需的。 SSPROP_PARAM_UDT_CATALOGNAME 和 SSPROP_PARAM_UDT_SCHEMANAME 是可选的。 如果任何属性指定有误，将返回 DB_E_ERRORSINCOMMAND。 如果未指定 SSPROP_PARAM_UDT_CATALOGNAME 和 SSPROP_PARAM_UDT_SCHEMANAME，则必须在定义表的同一数据库和架构中定义 UDT。 如果 UDT 定义没有位于表所在的架构中（但位于相同的数据库中），则必须指定 SSPROP_PARAM_UDT_SCHEMANAME。 如果 UDT 定义位于不同的数据库中，则必须指定 SSPROP_PARAM_UDT_CATALOGNAME 和 SSPROP_PARAM_UDT_SCHEMANAME。  
  
#### <a name="the-dbpropset_sqlservercolumn-property-set"></a>DBPROPSET_SQLSERVERCOLUMN 属性集  
 为了支持在 **ITableDefinition** 接口中创建表， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将以下三个新列添加到 DBPROPSET_SQLSERVERCOLUMN 属性集。  
  
|名称|说明|类型|说明|  
|----------|-----------------|----------|-----------------|  
|SSPROP_COL_UDT_CATALOGNAME|UDT_CATALOGNAME|VT_BSTR|对于 DBTYPE_UDT 类型的列，此属性是一个字符串，它指定定义 UDT 的目录的名称。|  
|SSPROP_COL_UDT_SCHEMANAME|UDT_SCHEMANAME|VT_BSTR|对于 DBTYPE_UDT 类型的列，此属性是一个字符串，它指定定义 UDT 的架构的名称。|  
|SSPROP_COL_UDT_NAME|UDT_NAME|VT_BSTR|对于 DBTYPE_UDT 类型的列，此属性是一个字符串，它指定仅由一个部分组成的 UDT 名称。 对于其他列类型，此属性返回空字符串。|  
  
> [!NOTE]  
>  UDT 不出现在 PROVIDER_TYPES 架构行集中。 所有列都具有读写访问权限。  
  
 ADO 将通过使用“说明”列中的相应条目引用这些属性。  
  
 SSPROP_COL_UDTNAME 是必需的。 SSPROP_COL_UDT_CATALOGNAME 和 SSPROP_COL_UDT_SCHEMANAME 是可选的。 如果任何属性指定有误，将返回 DB_E_ERRORSINCOMMAND  。  
  
 如果既未指定 SSPROP_COL_UDT_CATALOGNAME 也未指定 SSPROP_COL_UDT_SCHEMANAME，则必须在定义表的同一数据库和架构中定义 UDT。  
  
 如果 UDT 定义没有与表位于同一架构中（但位于同一数据库中），则必须指定 SSPROP_COL_UDT_SCHEMANAME。  
  
 如果 UDT 定义位于不同的数据库中，则必须指定 SSPROP_COL_UDT_CATALOGNAME 和 SSPROP_COL_UDT_SCHEMANAME。  
  
### <a name="ole-db-interface-additions-and-changes"></a>OLE DB 接口的添加和更改内容  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将新值或更改添加到许多核心 OLE DB 接口。  
  
#### <a name="the-isscommandwithparameters-interface"></a>ISSCommandWithParameters 接口  
 为了通过 OLE DB 支持 Udt， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 实现了许多更改，包括添加 **ISSCommandWithParameters** 接口。 这一新接口继承自核心 OLE DB 接口 ICommandWithParameters  。 除了从 **ICommandWithParameters** 继承的三个方法; **GetParameterInfo**、 **MapParameterNames** 和 **SetParameterInfo**; **ISSCommandWithParameters** 提供了用于处理服务器特定数据类型的 **GetParameterProperties** 和 **SetParameterProperties** 方法。  
  
> [!NOTE]  
>  ISSCommandWithParameters 接口也利用新的 SSPARAMPROPS 结构  。  
  
#### <a name="the-icolumnsrowset-interface"></a>IColumnsRowset 接口  
 除了 **ISSCommandWithParameters** 接口以外， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 还会向从调用 **IColumnsRowset：： GetColumnRowset** 方法返回的行集添加新值，包括以下内容。  
  
|列名|类型|说明|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_UDT_CATALOGNAME|DBTYPE_WSTR|UDT 目录名称标识符。|  
|DBCOLUMN_SS_UDT_SCHEMANAME|DBTYPE_WSTR|UDT 架构名称标识符。|  
|DBCOLUMN_SS_UDT_NAME|DBTYPE_WSTR|UDT 名称标识符。|  
|DBCOLUMN_SS_ASSEMBLY_TYPENAME|DBTYPE_WSTR|程序集限定名，其中包括 CLR 引用所需的类型名称和所有程序集标识。|  
  
 通过查看上面指定的添加的 UDT 元数据，可以在 DBCOLUMN_TYPE 设置为 DBTYPE_UDT 时将服务器 UDT 列与其他二进制类型区分开。 如果该数据不完整，服务器类型为 UDT。 对于非 UDT 服务器类型，这些列的返回结果始终为 NULL。  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 在 Native Client ODBC 驱动程序中进行了多项更改 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，以支持 udt。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] UDT 映射到 SQL_SS_UDT 驱动程序特定的 SQL 数据类型标识符。 UDT 列呈现为 SQL_SS_UDT。 如果通过使用 UDT 的 **ToString** 或 **ToXMLString** 方法或通过 **CAST/CONVERT** 函数，在 SQL 语句中将 udt 列显式映射到另一种类型，则结果集中列的类型将反映该列转换为的实际类型。  
  
### <a name="sqlcolattribute-sqldescribeparam-sqlgetdescfield"></a>SQLColAttribute、SQLDescribeParam、SQLGetDescField  
 添加了四个新的驱动程序特定的描述符字段，以提供要通过 [SQLColAttribute](../../../relational-databases/native-client-odbc-api/sqlcolattribute.md)、 [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md)和 [SQLGetDescField](../../../relational-databases/native-client-odbc-api/sqlgetdescfield.md) 函数检索的结果集或存储过程/参数化查询的 udt 参数的附加信息。  
  
 这四个新添加的描述符字段分别为 SQL_CA_SS_UDT_CATALOG_NAME、SQL_CA_SS_UDT_SCHEMA_NAME、SQL_CA_SS_UDT_TYPE_NAME 和 SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME。  
  
### <a name="sqlcolumns-sqlprocedurecolumns"></a>SQLColumns、SQLProcedureColumns  
 此外，将向 [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md) 和 [SQLProcedureColumns](../../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md) 函数返回的结果集中添加三个新的驱动程序特定列，以提供有关 udt 结果集列或 udt 参数的其他信息。 这三个新列分别为 SS_UDT_CATALOG_NAME、SS_UDT_SCHEMA_NAME 和 SS_UDT_ASSEMBLY_TYPE_NAME。  
  
### <a name="supported-conversions"></a>支持的转换  
 从 SQL 转换为 C 数据类型时，SQL_C_WCHAR、SQL_C_BINARY 和 SQL_C_CHAR 可全部转换为 SQL_SS_UDT。 不过请注意，从 SQL_C_WCHAR 和 SQL_C_CHAR SQL 数据类型转换时，二进制数据将转换为十六进制字符串。  
  
 从 C 转换为 SQL 数据类型时，SQL_C_WCHAR、SQL_C_BINARY 和 SQL_C_CHAR 可全部转换为 SQL_SS_UDT。 但请注意，从 SQL_C_WCHAR 和 SQL_C_CHAR SQL 数据类型转换时，二进制数据会转换为十六进制字符串。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
