---
description: 在 SQL Server 本机客户端中使用 XML 数据类型
title: 使用 XML 数据类型 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- IRowsetChange interface
- IRowsetUpdate interface
- data access [SQL Server Native Client], xml data type
- SQL Server Native Client OLE DB schema rowsets
- PROVIDER_TYPES rowset
- IColumnsInfo interface
- IRowsetFind interface
- IColumnsRowset interface
- PROCEDURE_PARAMETERS rowset
- SQLNCLI, XML
- xml data type [SQL Server], SQL Server Native Client
- SQL Server Native Client, XML
- IRowset interface
- ISequentialStream interface
- ISSCommandWithParameters interface
- SS_XMLSCHEMA rowset
- SQL Server Native Client OLE DB interfaces
- XML [SQL Server], SQL Server Native Client
- COLUMNS rowset
ms.assetid: a7af5b72-c5c2-418d-a636-ae4ac6270ee5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 76b24491df72773fb45b0513720151ed9a9c6585
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461958"
---
# <a name="using-xml-data-types-in-sql-server-native-clients"></a>在 SQL Server 本机客户端中使用 XML 数据类型
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入了 xml 数据类型，它可用于在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中存储 XML 文档和片段。 xml 数据类型是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的内置数据类型，在某些方面类似于其他内置类型（如 int 和 varchar）。 与使用其他内置类型一样，可以在创建表时将 xml 数据类型用作列类型，也可以将其用作变量类型、参数类型或函数返回类型，还可以将其用在 CAST 和 CONVERT 函数中  。  
  
## <a name="programming-considerations"></a>编程时的注意事项  
 XML 可以是自描述的，即可以根据需要包含一个 XML 标头来指定文档的编码，例如：  
  
 `<?xml version="1.0" encoding="windows-1252"?><doc/>`  
  
 XML 标准描述 XML 处理器如何通过检查文档的前若干个字节来检测用于文档的编码。 有时，应用程序指定的编码可能与文档指定的编码相冲突。 对于作为绑定参数传递的文档，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将 XML 视为二进制数据，因此不执行转换，并且 XML 分析器可以使用在文档内指定的编码而不会出现问题。 然而，对于绑定为 WSTR 的 XML 数据，应用程序必须确保将文档编码为 Unicode。 这可能需要将文件加载到 DOM 中，同时将编码更改为 Unicode 并对文档进行序列化。 否则，可能发生数据转换，而导致 XML 无效或损坏。  
  
 当在文字中指定 XML 时，也可能发生冲突。 例如，下面的内容无效：  
  
 `INSERT INTO xmltable(xmlcol) VALUES('<?xml version="1.0" encoding="UTF-16"?><doc/>')`  
  
 `INSERT INTO xmltable(xmlcol) VALUES(N'<?xml version="1.0" encoding="UTF-8"?><doc/>')`  
  
## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 DBTYPE_XML 是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口中特定于 XML 的新数据类型。 此外，可以通过现有的 OLE DB 类型（DBTYPE_BYTES、DBTYPE_WSTR、DBTYPE_BSTR、DBTYPE_XML、DBTYPE_STR、DBTYPE_VARIANT 和 DBTYPE_IUNKNOWN）访问 XML 数据。 可以从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口行集的列中按以下格式检索存储在类型为 XML 的列中的数据：  
  
-   文本字符串  
  
-   **ISequentialStream**  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序不包括 SAX 读取器，但可以轻松地将 **ISequentialStream** 传递给 MSXML 中的 sax 和 DOM 对象。  
  
 **ISequentialStream** 应用于检索大型 XML 文档。 用于其他大值类型的相同技术也适用于 XML。 有关详细信息，请参阅[使用大型数据类型](../../../relational-databases/native-client/features/using-large-value-types.md)。  
  
 还可以通过常见接口（如 IRow::GetColumns、IRowChange::SetColumns 和 ICommand::Execute）检索、插入或更新行集中类型为 XML 的列中存储的数据    。 与检索案例类似，应用程序可以将文本字符串或 **ISequentialStream** 传递给 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序。  
  
> [!NOTE]  
>  要通过 ISequentialStream 接口以字符串格式发送 XML 数据，必须通过指定 DBTYPE_IUNKNOWN 获取 ISequentialStream 并在绑定中将其 pObject 参数设置为 Null    。  
  
 当由于使用者缓冲区过小而导致已检索的 XML 数据被截断时，长度可能作为 0xffffffff 返回，这意味着长度为未知的。 这与将它实现为针对客户端进行流处理的数据类型而未在发送实际数据之前发送长度信息是一致的。 在某些情况下，当提供程序缓冲了整个值（如 **IRowset：：：：** ）并执行数据转换时，可能会返回实际长度。  
  
 服务器会将发送到 SQL Server 的 XML 数据视为二进制数据。 这可防止发生任何转换并允许 XML 分析器自动检测 XML 编码。 这样，就可以接受更广泛的 XML 文档（例如，以 UTF-8 编码的文档）作为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的输入。  
  
 如果将输入 XML 绑定为 DBTYPE_WSTR，则应用程序必须确保已对其进行 Unicode 编码，以避免由于不必要的数据转换而可能导致损坏。  
  
### <a name="data-bindings-and-coercions"></a>数据绑定和强制  
 下表描述将所列数据类型与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] xml  数据类型一同使用时出现的绑定和强制转换。  
  
|数据类型|到服务器<br /><br /> **XML**|到服务器<br /><br /> **Non-XML**|从服务器<br /><br /> **XML**|从服务器<br /><br /> **Non-XML**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_XML|传递<sup>6，7</sup>|错误<sup>1</sup>|确定<sup>11, 6</sup>|错误<sup>8</sup>|  
|DBTYPE_BYTES|传递<sup>6，7</sup>|N/A<sup>2</sup>|成功 <sup>11，6</sup>|N/A <sup>2</sup>|  
|DBTYPE_WSTR|传递<sup>6，10</sup>|N/A <sup>2</sup>|成功<sup>4，6，12</sup>|N/A <sup>2</sup>|  
|DBTYPE_BSTR|传递<sup>6，10</sup>|N/A <sup>2</sup>|成功 <sup>3</sup>|N/A <sup>2</sup>|  
|DBTYPE_STR|成功<sup>6，9，10</sup>|N/A <sup>2</sup>|成功<sup>5，6，12</sup>|N/A <sup>2</sup>|  
|DBTYPE_IUNKNOWN|通过 ISequentialStream 的字节流<sup>7</sup>|N/A <sup>2</sup>|通过 ISequentialStream 的字节流<sup>11</sup>|N/A <sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|传递<sup>6，7</sup>|N/A <sup>2</sup>|空值|N/A <sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|传递<sup>6，10</sup>|N/A <sup>2</sup>|成功<sup>3</sup>|N/A <sup>2</sup>|  
  
 <sup>1</sup>如果使用 ICommandWithParameters::SetParameterInfo  指定了除 DBTYPE_XML 以外的服务器类型，而取值函数类型为 DBTYPE_XML，那么就会在执行语句时出错（DB_E_ERRORSOCCURRED，参数状态为 DBSTATUS_E_BADACCESSOR）；否则，数据发送到服务器，但服务器会返回错误，指明不存在将 XML 转换为参数的数据类型的隐式转换。  
  
 <sup>2</sup>超出了本主题的范围。  
  
 <sup>3</sup>格式为 UTF-16、无字节顺序标记 (BOM)、无编码规格，且无空终止。  
  
 <sup>4</sup>格式为 UTF-16、无 BOM、无编码规格，且空终止。  
  
 <sup>5</sup>格式为在客户端代码页中编码的带有空终止符的多字节字符。 从服务器提供的 Unicode 进行转换可能导致数据损坏，因此，强烈建议不要使用此绑定。  
  
 <sup>6</sup>可使用 BY_REF。  
  
 <sup>7</sup>UTF-16 必须以 BOM 开头。 否则，服务器可能无法正确地识别编码。  
  
 <sup>8</sup>在创建取值函数时或在提取时，可能进行验证。 此错误为 DB_E_ERRORSOCCURRED，绑定状态设置为 DBBINDSTATUS_UNSUPPORTEDCONVERSION。  
  
 <sup>9</sup>使用客户端代码页将数据转换为 Unicode，然后发送到服务器。 如果文档编码与客户端代码页不匹配，则这可能导致数据损坏，因此强烈建议不要使用此绑定。  
  
 <sup>10</sup>始终向发送到服务器的数据添加一个 BOM。 如果数据已经以一个 BOM 开始，则这可能导致在缓冲区的开头具有两个 BOM。 服务器使用第一个 BOM 将编码识别为 UTF-16，然后放弃它。 第二个 BOM 将被解释为零宽度不间断空格字符。  
  
 <sup>11</sup>格式为 UTF-16、无编码规格，且向从服务器收到的数据添加一个 BOM。 如果服务器返回了空字符串，则仍将 BOM 返回给应用程序。 如果缓冲区长度的字节数为奇数，则正确截断数据。 如果分块区返回了整个值，则可将其进行连接，重新构建正确的值。  
  
 <sup>12</sup>如果缓冲区长度小于两个字符（即，空值没有足够的空间），则会报告溢出错误。  
  
> [!NOTE]  
>  对于 NULL XML 值，不返回值。  
  
 XML 标准要求 UTF-16 编码的 XML，以便以字节顺序标记 (BOM) UTF-16 字符代码 0xFEFF 开头。 使用 WSTR 和 BSTR 绑定时， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 不需要或添加 BOM，因为绑定隐含了编码。 当使用 BYTES、XML 或 IUNKNOWN 绑定时，目的是为了在处理其他 XML 处理器和存储系统时提供简便性。 在此情况下，应向 UTF-16 编码的 XML 提供 BOM，并且应用程序无需关心实际编码，因为绝大多数的 XML 处理器（包括 SQL Server）会通过检查该值的前几个字节推导出编码。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]使用字节、XML 或 IUNKNOWN 绑定从 Native Client 收到的 XML 数据始终以 utf-16 进行编码，并且具有 BOM 并且没有嵌入的编码声明。  
  
 OLE DB 核心服务 (IDataConvert) 提供的数据转换不适用于 DBTYPE_XML  。  
  
 当向服务器发送数据时将执行验证。 应由应用程序处理客户端验证和编码更改，强烈建议您不要直接处理 XML 数据，而应使用 DOM 或 SAX 读取器来处理此数据。  
  
 可绑定 DBTYPE_NULL 和 DBTYPE_EMPTY 用于输入参数，但不能用于输出参数或结果。 当将它们绑定用于输入参数时，状态必须设置为 DBSTATUS_S_ISNULL 或 DBSTATUS_S_DEFAULT。  
  
 可以将 DBTYPE_XML 转换为 DBTYPE_EMPTY 和 DBTYPE_NULL，并可以将 DBTYPE_EMPTY 转换为 DBTYPE_XML，但无法将 DBTYPE_NULL 转换为 DBTYPE_XML。 这与 DBTYPE_WSTR 相一致。  
  
 DBTYPE_IUNKNOWN 是支持的绑定（如上表中所示），但在 DBTYPE_XML 与 DBTYPE_IUNKNOWN 之间不存在转换。 DBTYPE_IUNKNOWN 不能与 DBTYPE_BYREF 一起使用。  
  
### <a name="ole-db-rowset-additions-and-changes"></a>OLE DB 行集的添加和更改内容  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将新值或更改添加到许多核心 OLE DB 架构行集。  
  
#### <a name="the-columns-and-procedure_parameters-schema-rowsets"></a>COLUMNS 和 PROCEDURE_PARAMETERS 架构行集  
 COLUMNS 和 PROCEDURE_PARAMETERS 架构行集添加了以下列。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|在其中定义 XML 架构集合的目录的名称。 对于非 XML 列或非类型化 XML 列，为 NULL。|  
|SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|在其中定义 XML 架构集合的架构的名称。 对于非 XML 列或非类型化 XML 列，为 NULL。|  
|SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|XML 架构集合的名称。 对于非 XML 列或非类型化 XML 列，为 NULL。|  
  
#### <a name="the-provider_types-schema-rowset"></a>PROVIDER_TYPES 架构行集  
 在 PROVIDER_TYPES 架构行集中，xml 数据类型的 COLUMN_SIZE 值为 0，且 DATA_TYPE 为 DBTYPE_XML  。  
  
#### <a name="the-ss_xmlschema-schema-rowset"></a>SS_XMLSCHEMA 架构行集  
 所引入的新的架构行集 SS_XMLSCHEMA 可供客户端检索 XML 架构信息。 SS_XMLSCHEMA 行集包含以下列。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|XML 集合所属的目录。|  
|SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|XML 集合所属的架构。|  
|SCHEMACOLLECTIONNAME|DBTYPE_WSTR|类型化的 XML 列的 XML 架构集合名称；否则为 NULL。|  
|TARGETNAMESPACEURI|DBTYPE_WSTR|XML 架构的目标命名空间。|  
|SCHEMACONTENT|DBTYPE_WSTR|XML 架构内容。|  
  
 每个 XML 架构都按目录名称、架构名称、架构集合名称和目标命名空间统一资源标识符 (URI) 限定范围。 此外，还定义了新的 GUID，其名称为 DBSCHEMA_XML_COLLECTIONS。 SS_XMLSCHEMA 架构行集的限制数和受限列定义如下。  
  
|GUID|限制数|受限列|  
|----------|----------------------------|------------------------|  
|DBSCHEMA_XML_COLLECTIONS|4|SCHEMACOLLECTION_CATALOGNAME<br /><br /> SCHEMACOLLECTION_SCHEMANAME<br /><br /> SCHEMACOLLECTIONNAME<br /><br /> TARGETNAMESPACEURI|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>OLE DB 属性集的添加和更改内容  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 向许多 core OLE DB 属性集添加新值或更改。  
  
#### <a name="the-dbpropset_sqlserverparameter-property-set"></a>DBPROPSET_SQLSERVERPARAMETER 属性集  
 为了通过 OLE DB 支持 **xml** 数据类型， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 实现了新的 DBPROPSET_SQLSERVERPARAMETER 属性集，该属性集包含以下值。  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|在其中定义 XML 架构集合的目录（数据库）的名称。 由三部分组成的 SQL 名称标识符的一部分。|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|架构集合内 XML 架构的名称。 由三部分组成的 SQL 名称标识符的一部分。|  
|SSPROP_PARAM_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|目录内 XML 架构集合的名称。由三部分组成的 SQL 名称标识符的一部分。|  
  
#### <a name="the-dbpropset_sqlservercolumn-property-set"></a>DBPROPSET_SQLSERVERCOLUMN 属性集  
 为了支持在 **ITableDefinition** 接口中创建表， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将三个新列添加到 DBPROPSET_SQLSERVERCOLUMN 属性集。  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME|VT_BSTR|对于类型化 XML 列，此属性是一个字符串，它指定在其中存储 XML 架构的目录的名称。 对于其他列类型，此属性返回空字符串。|  
|SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME|VT_BSTR|对于类型化 XML 列，此属性是一个字符串，它指定定义此列的 XML 架构的名称。|  
|SSPROP_COL_XML_SCHEMACOLLECTIONNAME|VT_BSTR|对于类型化 XML 列，此属性是一个字符串，它指定定义此值的 XML 架构集合的名称。|  
  
 与 SSPROP_PARAM 值类似，所有这些属性都是可选的且默认值为空。 仅当指定 SSPROP_COL_XML_SCHEMACOLLECTIONNAME 时，才能指定 SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME 和 SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME。 在将 XML 传递到服务器时，如果包含这些值，则将针对当前数据库检查它们是否存在（有效性），并且针对架构检查实例数据。 在所有情况下，要使这些值有效，它们必须全部为空或全部添入这些值。  
  
### <a name="ole-db-interface-additions-and-changes"></a>OLE DB 接口的添加和更改内容  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将新值或更改添加到许多核心 OLE DB 接口。  
  
#### <a name="the-isscommandwithparameters-interface"></a>ISSCommandWithParameters 接口  
 为了通过 OLE DB 支持 **xml** 数据类型， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 实现了许多更改，包括添加 [ISSCommandWithParameters](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md) 接口。 这一新接口继承自核心 OLE DB 接口 ICommandWithParameters  。 除了从 **ICommandWithParameters** 继承的三个方法; **GetParameterInfo**、 **MapParameterNames** 和 **SetParameterInfo**; **ISSCommandWithParameters** 提供了用于处理服务器特定数据类型的 [GetParameterProperties](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md) 和 [SetParameterProperties](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) 方法。  
  
> [!NOTE]  
>  ISSCommandWithParameters 接口也利用新的 SSPARAMPROPS 结构  。  
  
#### <a name="the-icolumnsrowset-interface"></a>IColumnsRowset 接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 特定列添加到 **IColumnRowset：： GetColumnsRowset** 方法返回的行集。 这些列包含 XML 架构集合的由三个部分组成的名称。 对于非 XML 列或非类型化 XML 列，所有这三列均取默认值 NULL。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|XML 架构集合所属的目录，<br /><br /> 否则为 Null。|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|XML 架构集合所属的架构。 否则为 Null。|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|类型化 XML 列的 XML 架构集合的名称，否则为 NULL。|  
  
#### <a name="the-irowset-interface"></a>IRowset 接口  
 通过 IRowset::GetData 方法检索 XML 列中的 XML 实例  。 根据由客户端指定的绑定，可以将 XML 实例作为 DBTYPE_BSTR、DBTYPE_WSTR、DBTYPE_VARIANT、DBTYPE_XML、DBTYPE_STR、DBTYPE_BYTES 进行检索，或者通过 DBTYPE_IUNKNOWN 将其作为接口检索。 如果使用者指定 DBTYPE_BSTR、DBTYPE_WSTR 或 DBTYPE_VARIANT，则访问接口将 XML 实例转换为用户请求的类型，并将其放入在相应绑定中指定的位置。  
  
 如果使用者指定 DBTYPE_IUNKNOWN 并将 pObject 参数设置为 NULL，或将 pObject 参数设置为 IID_ISequentialStream，则提供程序将 ISequentialStream 接口返回到使用者，让其可流式处理该列之外的 XML 数据    。 然后，ISequentialStream 将 XML 数据作为 Unicode 字符流返回  。  
  
 当返回与 DBTYPE_IUNKNOWN 绑定的 XML 值时，访问接口将报告 `sizeof (IUnknown *)` 的大小值。 请注意，这与在将某列绑定为 DBTYPE_IUnknown 或 DBTYPE_IDISPATCH 时所采取的方法相一致；当无法确定实际列大小时，将返回 DBTYPE_IUNKNOWN/ISequentialStream。  
  
#### <a name="the-irowsetchange-interface"></a>IRowsetChange 接口  
 使用者可以通过两种方法更新列中的 XML 实例。 第一种方法是使用提供程序创建的存储对象 ISequentialStream  。 使用者可调用 ISequentialStream::Write 方法直接更新提供程序返回的 XML 实例  。  
  
 第二种方法是通过 IRowsetChange::SetData 或 IRowsetChange::InsertRow 方法   。 使用这种方法，可以在类型为 DBTYPE_BSTR、DBTYPE_WSTR、DBTYPE_VARIANT、DBTYPE_XML 或 DBTYPE_IUNKNOWN 的绑定中，指定使用者缓冲区内的 XML 实例。  
  
 对于 DBTYPE_BSTR、DBTYPE_WSTR 或 DBTYPE_VARIANT，访问接口将位于使用者缓冲区中的 XML 实例存储到适当的列中。  
  
 在 DBTYPE_IUNKNOWN/ISequentialStream 的情况下，如果使用者未指定任何存储对象，则使用者必须事先创建 **ISequentialStream** 对象，将 XML 文档与对象绑定，然后通过 **IRowsetChange：： SetData** 方法将该对象传递给提供程序。 使用者还可以创建存储对象，将 pObject 参数设置为 IID_ISequentialStream，创建 ISequentialStream 对象，然后将 ISequentialStream 对象传递给 IRowsetChange::SetData 方法    。 在这两种情况下，提供程序都可以通过 ISequentialStream 对象检索 XML 对象，然后将其插入适当的列中  。  
  
#### <a name="the-irowsetupdate-interface"></a>IRowsetUpdate 接口  
 IRowsetUpdate 接口提供了用于延迟更新的功能  。 在使用者调用 **IRowsetUpdate： Update** 方法之前，对行集可用的数据不会提供给其他事务。  
  
#### <a name="the-irowsetfind-interface"></a>IRowsetFind 接口  
 IRowsetFind::FindNextRow 方法不使用 xml 数据类型   。 调用 IRowsetFind::FindNextRow 且 hAccessor 参数指定 DBTYPE_XML 列时，返回 DB_E_BADBINDINFO   。 此时不考虑正在搜索的列的类型。 对于任何其他绑定类型，如果要搜索的列属于 xml 数据类型，则 FindNextRow 将失败并返回 DB_E_BADCOMPAREOP   。  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序中，对各种函数进行了一些更改以支持 **xml** 数据类型。  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 [SQLColAttribute](../../../relational-databases/native-client-odbc-api/sqlcolattribute.md)函数具有三个新的字段标识符，包括 SQL_CA_SS_XML_SCHEMACOLLECTION_CATALOG_NAME、SQL_CA_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME 和 SQL_CA_SS _XML_SCHEMACOLLECTION_NAME。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序报告 SQL_DESC_DISPLAY_SIZE 和 SQL_DESC_LENGTH 列 SQL_SS_LENGTH_UNLIMITED。  
  
### <a name="sqlcolumns"></a>SQLColumns  
 [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md)函数包含三个新列，其中包括 SS_XML_SCHEMACOLLECTION_CATALOG_NAME、SS_XML_SCHEMACOLLECTION_SCHEMA_NAME 和 SS_XML_SCHEMACOLLECTION_NAME。 现有的 TYPE_NAME 列用于指示 XML 类型的名称，而 XML 类型列或参数的 DATA_TYPE 为 SQL_SS_XML。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序报告 COLUMN_SIZE 和 CHAR_OCTET_LENGTH 值 SQL_SS_LENGTH_UNLIMITED。  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]当无法在[SQLDescribeCol](../../../relational-databases/native-client-odbc-api/sqldescribecol.md)函数中确定列大小时，Native Client ODBC 驱动程序将报告 SQL_SS_LENGTH_UNLIMITED。  
  
### <a name="sqlgettypeinfo"></a>SQLGetTypeInfo  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序报告 SQL_SS_LENGTH_UNLIMITED 作为 [SQLGetTypeInfo](../../../relational-databases/native-client-odbc-api/sqlgettypeinfo.md)函数中 **xml** 数据类型的最大 COLUMN_SIZE。  
  
### <a name="sqlprocedurecolumns"></a>SQLProcedureColumns  
 [SQLProcedureColumns](../../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md)函数具有与 **SQLColumns** 函数相同的列添加。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序报告 SQL_SS_LENGTH_UNLIMITED 作为 **xml** 数据类型的最大 COLUMN_SIZE。  
  
### <a name="supported-conversions"></a>支持的转换  
 当从 SQL 转换为 C 数据类型时，SQL_C_WCHAR、SQL_C_BINARY 和 SQL_C_CHAR 可全部转换为 SQL_SS_XML，但必须符合以下规定：  
  
-   SQL_C_WCHAR：格式为 UTF-16，无字节顺序标记 (BOM)，无空终止。  
  
-   SQL_C_BINARY：格式为 UTF-16，无空终止。 向从服务器接收的数据添加一个 BOM。 如果服务器返回了空字符串，则仍然将 BOM 返回给应用程序。 如果缓冲区长度的字节数为奇数，则会正确地截断数据。 如果分块区返回了整个值，则可以将它们连接起来以重新构建正确的值。  
  
-   SQL_C_CHAR：格式为在客户端代码页中编码的带有空终止的多字节字符。 从服务器提供的 UTF-16 进行转换可能导致数据损坏，因此，强烈建议不要使用此绑定。  
  
 当从 C 转换为 SQL 数据类型时，SQL_C_WCHAR、SQL_C_BINARY 和 SQL_C_CHAR 可全部转换为 SQL_SS_XML，但必须符合以下规定：  
  
-   SQL_C_WCHAR：始终向发送到服务器的数据添加一个 BOM。 如果数据已经以一个 BOM 开始，则这可能导致在缓冲区的开头具有两个 BOM。 服务器使用第一个 BOM 将编码识别为 UTF-16，然后放弃它。 第二个 BOM 将被解释为零宽度不间断空格字符。  
  
-   SQL_C_BINARY：不执行转换，数据“按原样”传递到服务器。 UTF-16 数据必须以 BOM 为开头；否则，服务器可能无法正确地识别编码。  
  
-   SQL_C_CHAR：数据在客户端上转换为 UTF-16 并发送到服务器，就像 SQL_C_WCHAR 一样（包括添加 BOM）。 如果未在客户端代码页中对 XML 进行编码，则这可能导致数据损坏。  
  
 XML 标准要求 UTF-16 编码的 XML，以便以字节顺序标记 (BOM) UTF-16 字符代码 0xFEFF 开头。 使用 SQL_C_BINARY 绑定时， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 不需要或添加 BOM，因为绑定隐含了编码。 其目的是为了在处理其他 XML 处理器和存储系统时提供简便性。 在此情况下，应向 UTF-16 编码的 XML 提供 BOM，并且应用程序无需关心实际编码，因为绝大多数的 XML 处理器（包括 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]）会通过检查该值的前几个字节推导出编码。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]使用 SQL_C_BINARY 绑定从 Native Client 收到的 XML 数据始终以 utf-16 进行编码，并且具有 BOM，不带嵌入的编码声明。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
