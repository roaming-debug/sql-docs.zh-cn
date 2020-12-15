---
description: 'ITableDefinition 中的数据类型映射 (Native Client OLE DB 提供程序) '
title: ITableDefinition 中的数据类型映射 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- SQL Server Native Client OLE DB provider, data types
- ITableDefinition interface
- DBCOLUMNDESC structure
- data types [OLE DB]
- CreateTable function
- OLE DB, data types
ms.assetid: 13292d1f-c17e-4d11-bf98-3460a10cbb18
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 601dced15dd86a4caf639f5bc48aeadf81166925
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477958"
---
# <a name="sql-server-native-client-data-type-mapping-in-itabledefinition"></a>SQL Server Native Client ITableDefinition 中的数据类型映射
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  使用 **ITableDefinition：： CreateTable** 函数创建表时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者可以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在传递的 DBCOLUMNDESC 数组的 *pwszTypeName* 成员中指定数据类型。 如果使用者按照名称指定列的数据类型，则忽略由 DBCOLUMNDESC 结构的 wType 成员表示的 OLE DB 数据类型映射  。  
  
 使用 DBCOLUMNDESC 结构 *wType* 成员指定具有 OLE DB 数据类型的新列数据类型时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将按如下所示映射 OLE DB 数据类型。  
  
|OLE DB 数据类型|SQL Server<br /><br /> 数据类型|其他信息|  
|----------------------|------------------------------|----------------------------|  
|DBTYPE_BOOL|**bit**||  
|DBTYPE_BYTES|binary、varbinary、image 或 varbinary(max)    |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序检查 DBCOLUMNDESC 结构的 *ulColumnSize* 成员。 基于实例的值和版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射到 **映像**。<br /><br /> 如果 *ulColumnSize* 的值小于 **binary** 数据类型列的最大长度，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将检查 DBCOLUMNDESC *rgPropertySets* 成员。 如果 VARIANT_TRUE DBPROP_COL_FIXEDLENGTH，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **binary**。 如果 VARIANT_FALSE 属性的值，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **varbinary**。 在这两种情况下，DBCOLUMNDESC 的 ulColumnSize 成员将确定创建的 SQL Server 列的宽度  。|  
|DBTYPE_CY|**money**||  
|DBTYPE_DBTIMESTAMP|**datetime**||  
|DBTYPE_GUID|**uniqueidentifier**||  
|DBTYPE_I2|**smallint**||  
|DBTYPE_I4|**int**||  
|DBTYPE_NUMERIC|**numeric**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序检查 DBCOLUMDESC *BPrecision* 和 *bScale* 成员，以确定 **数值** 列的精度和小数位数。|  
|DBTYPE_R4|**real**||  
|DBTYPE_R8|**float**||  
|DBTYPE_STR|char、varchar、text 或 varchar(max)    |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序检查 DBCOLUMNDESC 结构的 *ulColumnSize* 成员。 基于实例的值和版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **文本**。<br /><br /> 如果 *ulColumnSize* 的值小于多字节字符数据类型列的最大长度，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将检查 DBCOLUMNDESC *rgPropertySets* 成员。 如果 VARIANT_TRUE DBPROP_COL_FIXEDLENGTH，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **char**。 如果 VARIANT_FALSE 属性的值，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **varchar**。 在这两种情况下，DBCOLUMNDESC ulColumnSize 成员将确定创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列的宽度。|  
|DBTYPE_UDT|**UDT**|需要 UDT 列时，ITableDefinition::CreateTable 将在 DBCOLUMNDESC 结构中用到以下信息   ：<br /><br /> pwSzTypeName  将被忽略。<br /><br /> rgPropertySets 必须包括 DBPROPSET_SQLSERVERCOLUMN 属性集，如[使用用户定义的类型](../../relational-databases/native-client/features/using-user-defined-types.md)中的 DBPROPSET_SQLSERVERCOLUMN 部分所述。|  
|DBTYPE_UI1|**tinyint**||  
|DBTYPE_WSTR|nchar、nvarchar、ntext 或 nvarchar(max)    |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序检查 DBCOLUMNDESC 结构的 *ulColumnSize* 成员。 根据值， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **ntext**。<br /><br /> 如果 *ulColumnSize* 的值小于 Unicode 字符数据类型列的最大长度，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将检查 DBCOLUMNDESC *rgPropertySets* 成员。 如果 VARIANT_TRUE DBPROP_COL_FIXEDLENGTH，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射为 **nchar**。 如果 VARIANT_FALSE 属性的值，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将该类型映射到 **nvarchar**。 在这两种情况下，DBCOLUMNDESC ulColumnSize 成员将确定创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列的宽度。|  
|DBTYPE_XML|**XML**||  
  
> [!NOTE]  
>  创建新表时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口只映射前一个表中指定的 OLE DB 数据类型枚举值。 尝试创建其中某一列为任何其他 OLE DB 数据类型的表时将生成错误。  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (OLE DB)](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  
