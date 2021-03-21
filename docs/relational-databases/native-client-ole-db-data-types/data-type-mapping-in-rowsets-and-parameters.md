---
description: '行集和参数中的数据类型映射 (Native Client OLE DB 提供程序) '
title: 行集和参数中的数据类型映射 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- DBTYPE_SQLVARIANT data type
- SQL Server Native Client OLE DB provider, data types
- rowsets [OLE DB], data type mapping
- data types [OLE DB]
- GetColumnInfo function
- parameters [OLE DB]
- SSPROP_ALLOWNATIVEVARIANT property
- GetParameterInfo function
- OLE DB, data types
ms.assetid: 3d831ff8-3b79-4698-b2c1-2b5dd2f8235c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aa356a77c5dffca7565fe563eeea3a4bdc1e5055
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748477"
---
# <a name="sql-server-native-client-data-type-mapping-in-rowsets-and-parameters"></a>SQL Server Native Client 行集和参数中的数据类型映射
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在行集和作为参数值的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 情况下，Native Client OLE DB 提供程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用以下 OLE DB 定义的数据类型来表示数据，这些数据类型在函数 **IColumnsInfo：： GetColumnInfo** 和 **ICommandWithParameters：： GetParameterInfo** 中报告。  
  
|SQL Server 数据类型|OLE DB 数据类型|  
|--------------------------|----------------------|  
|**bigint**|DBTYPE_I8|  
|**binary**|DBTYPE_BYTES|  
|**bit**|DBTYPE_BOOL|  
|**char**|DBTYPE_STR|  
|**datetime**|DBTYPE_DBTIMESTAMP|  
|**datetime2**|DBTYPE_DBTIME2|  
|**decimal**|DBTYPE_NUMERIC|  
|**float**|DBTYPE_R8|  
|**图像**|DBTYPE_BYTES|  
|**int**|DBTYPE_I4|  
|**money**|DBTYPE_CY|  
|**nchar**|DBTYPE_WSTR|  
|**ntext**|DBTYPE_WSTR|  
|**numeric**|DBTYPE_NUMERIC|  
|**nvarchar**|DBTYPE_WSTR|  
|**real**|DBTYPE_R4|  
|**smalldatetime**|DBTYPE_DBTIMESTAMP|  
|**smallint**|DBTYPE_I2|  
|**smallmoney**|DBTYPE_CY|  
|**sql_variant**|DBTYPE_VARIANT、DBTYPE_SQLVARIANT|  
|**sysname**|DBTYPE_WSTR|  
|**text**|DBTYPE_STR|  
|**timestamp**|DBTYPE_BYTES|  
|**tinyint**|DBTYPE_UI1|  
|**UDT**|DBTYPE_UDT|  
|**uniqueidentifier**|DBTYPE_GUID|  
|**varbinary**|DBTYPE_BYTES|  
|**varchar**|DBTYPE_STR|  
|**XML**|DBTYPE_XML|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持使用者请求的数据转换，如图所示。  
  
 sql_variant 对象可以保留除 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max)、xml、timestamp 和 Microsoft .NET Framework 公共语言运行时 (CLR) 用户定义类型以外的任意 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型的数据。 另外，sql_variant 数据实例还不能将 sql_variant 作为其基础的基本数据类型。 例如，列中的某些行可能包含 smallint 值，而其他某些行可能包含 float 值，剩余的行则包含 char/nchar 值。  
  
> [!NOTE]  
>  sql_variant 数据类型类似于 Microsoft Visual Basic® 中的 Variant 数据类型以及 OLEDB 中的 DBTYPE_VARIANT 和 DBTYPE_SQLVARIANT  。  
  
 当提取 sql_variant 数据作为 DBTYPE_VARIANT 时，该数据被放置到缓冲区的 VARIANT 结构中  。 但 VARIANT 结构中的子类型可能无法映射为 sql_variant 数据类型中定义的子类型  。 然后，必须提取 sql_variant 数据作为 DBTYPE_SQLVARIANT，以使所有子类型匹配  。  
  
## <a name="dbtype_sqlvariant-data-type"></a>DBTYPE_SQLVARIANT 数据类型  
 为了支持 **sql_variant** 数据类型， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序公开了一种称为 DBTYPE_SQLVARIANT 的特定于提供程序的数据类型。 当提取 sql_variant 数据作为 DBTYPE_SQLVARIANT 时，该数据存储到特定于访问接口的 SSVARIANT 结构中  。 SSVARIANT 结构包含与 sql_variant 数据类型的子类型匹配的所有子类型  。  
  
 此外，还必须将 SSPROP_ALLOWNATIVEVARIANT 会话属性设置为 TRUE。  
  
## <a name="provider-specific-property-ssprop_allownativevariant"></a>特定于访问接口的 SSPROP_ALLOWNATIVEVARIANT 属性  
 提取数据时，您可以显式指定应为列或参数返回的数据类型。 还可以使用 IColumnsInfo 获取列信息，并将该信息用于绑定  。 当使用 IColumnsInfo 获取列信息以用于绑定目的时，如果 SSPROP_ALLOWNATIVEVARIANT 会话属性为 FALSE（默认值），则为 sql_variant 列返回 DBTYPE_VARIANT   。 如果 SSPROP_ALLOWNATIVEVARIANT 属性为 FALSE，则不支持 DBTYPE_SQLVARIANT。 如果 SSPROP_ALLOWNATIVEVARIANT 属性设置为 TRUE，列类型将作为 DBTYPE_SQLVARIANT 返回，在这种情况下，缓冲区将保留 SSVARIANT 结构。 在提取 sql_variant 数据作为 DBTYPE_SQLVARIANT 时，必须将 SSPROP_ALLOWNATIVEVARIANT 会话属性设置为 TRUE  。  
  
 SSPROP_ALLOWNATIVEVARIANT 属性是特定于访问接口的 DBPROPSET_SQLSERVERSESSION 属性集的一部分，因此，该属性是一个会话属性。  
  
 DBTYPE_VARIANT 适用于所有其他 OLE DB 访问接口。  
  
## <a name="ssprop_allownativevariant"></a>SSPROP_ALLOWNATIVEVARIANT  
 SSPROP_ALLOWNATIVEVARIANT 是一个会话属性，并且是 DBPROPSET_SQLSERVERSESSION 属性集的一部分。  
  
|属性 ID|说明|  
|-|-|  
|SSPROP_ALLOWNATIVEVARIANT|键入：VT_BOOL<br /><br /> R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明:确定提取的数据是作为 DBTYPE_VARIANT 还是作为 DBTYPE_SQLVARIANT。<br /><br /> VARIANT_TRUE：列类型作为 DBTYPE_SQLVARIANT 返回（这种情况下，缓冲区会保留 SSVARIANT 结构）。<br /><br /> VARIANT_FALSE：列类型作为 DBTYPE_VARIANT 返回，且缓冲区会保留 VARIANT 结构。|  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (OLE DB)](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  
