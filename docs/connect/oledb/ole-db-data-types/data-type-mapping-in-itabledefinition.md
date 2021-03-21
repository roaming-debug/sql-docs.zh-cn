---
title: ITableDefinition 中的数据类型映射（OLE DB 驱动器）| Microsoft Docs
description: 了解在使用 ITableDefinition::CreateTable 方法创建表时，OLE DB Driver for SQL Server 使用者如何指定 SQL Server 数据类型。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- OLE DB Driver for SQL Server, data types
- ITableDefinition interface
- DBCOLUMNDESC structure
- data types [OLE DB]
- CreateTable function
- OLE DB, data types
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e111f1760415a3511686dce17d8dec5784c9a991
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755557"
---
# <a name="data-type-mapping-in-itabledefinition"></a>ITableDefinition 中的数据类型映射
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  当使用 ITableDefinition::CreateTable 函数创建表时，适用于 SQL Server 的 OLE DB 驱动程序使用者可以在向其传递的 DBCOLUMNDESC 数组的 pwszTypeName 成员中指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。 如果使用者按照名称指定列的数据类型，则忽略由 DBCOLUMNDESC 结构的 wType 成员表示的 OLE DB 数据类型映射  。  
  
 当使用 DBCOLUMNDESC 结构的 wType 成员指定具有 OLE DB 数据类型的新列数据类型时，适用于 SQL Server 的 OLE DB 驱动程序将按以下方式映射 OLE DB 数据类型  。  
  
|OLE DB 数据类型|SQL Server<br /><br /> 数据类型|其他信息|  
|----------------------|------------------------------|----------------------------|  
|DBTYPE_BOOL|**bit**||  
|DBTYPE_BYTES|binary、varbinary、image 或 varbinary(max)    |OLE DB Driver for SQL Server 将检查 DBCOLUMNDESC 结构的 ulColumnSize  成员。 根据该值和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的版本，OLE DB Driver for SQL Server 将该类型映射为 image  。<br /><br /> 如果 ulColumnSize 的值小于 binary 数据类型列的最大长度，则适用于 SQL Server 的 OLE DB 驱动程序将检查 DBCOLUMNDESC 的 rgPropertySets 成员    。 如果 DBPROP_COL_FIXEDLENGTH 为 VARIANT_TRUE，则 OLE DB Driver for SQL Server 将该类型映射为 binary  。 如果属性值为 VARIANT_FALSE，则 OLE DB Driver for SQL Server 将该类型映射为 varbinary  。 在这两种情况下，DBCOLUMNDESC 的 ulColumnSize 成员将确定创建的 SQL Server 列的宽度  。|  
|DBTYPE_CY|**money**||  
|DBTYPE_DBTIMESTAMP|**datetime2**||  
|DBTYPE_GUID|**uniqueidentifier**||  
|DBTYPE_I2|**smallint**||  
|DBTYPE_I4|**int**||  
|DBTYPE_I8|**bigint**||
|DBTYPE_NUMERIC|**numeric**|适用于 SQL Server 的 OLE DB 驱动程序检查 DBCOLUMDESC 的 bPrecision 和 bScale 成员以确定 numeric 列的精度和小数位数    。|  
|DBTYPE_R4|**real**||  
|DBTYPE_R8|**float**||  
|DBTYPE_STR|char、varchar、text 或 varchar(max)    |OLE DB Driver for SQL Server 将检查 DBCOLUMNDESC 结构的 ulColumnSize  成员。 根据该值和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的版本，OLE DB Driver for SQL Server 将该类型映射为 text  。<br /><br /> 如果 ulColumnSize 的值小于 multibyte 数据类型列的最大长度，则适用于 SQL Server 的 OLE DB 驱动程序将检查 DBCOLUMNDESC 的 rgPropertySets 成员   。 如果 DBPROP_COL_FIXEDLENGTH 为 VARIANT_TRUE，则 OLE DB Driver for SQL Server 将该类型映射为 char  。 如果属性值为 VARIANT_FALSE，则 OLE DB Driver for SQL Server 将该类型映射为 varchar  。 在这两种情况下，DBCOLUMNDESC ulColumnSize 成员将确定创建的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 列的宽度。|  
|DBTYPE_UDT|**UDT**|需要 UDT 列时，ITableDefinition::CreateTable 将在 DBCOLUMNDESC 结构中用到以下信息   ：<br /><br /> pwSzTypeName  将被忽略。<br /><br /> rgPropertySets 必须包括 DBPROPSET_SQLSERVERCOLUMN 属性集，如[使用用户定义的类型](../../oledb/features/using-user-defined-types.md)中的 DBPROPSET_SQLSERVERCOLUMN 部分所述。|  
|DBTYPE_UI1|**tinyint**||  
|DBTYPE_VARIANT|**sql_variant**||
|DBTYPE_WSTR|nchar、nvarchar、ntext 或 nvarchar(max)    |OLE DB Driver for SQL Server 将检查 DBCOLUMNDESC 结构的 ulColumnSize  成员。 根据该值，OLE DB Driver for SQL Server 将该类型映射为 ntext  。<br /><br /> 如果 ulColumnSize 的值小于 Unicode 数据类型列的最大长度，则适用于 SQL Server 的 OLE DB 驱动程序将检查 DBCOLUMNDESC 的 rgPropertySets 成员   。 如果 DBPROP_COL_FIXEDLENGTH 为 VARIANT_TRUE，则 OLE DB Driver for SQL Server 将该类型映射为 nchar  。 如果属性值为 VARIANT_FALSE，则 OLE DB Driver for SQL Server 将该类型映射为 nvarchar  。 在这两种情况下，DBCOLUMNDESC ulColumnSize 成员将确定创建的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 列的宽度。|  
|DBTYPE_XML|**XML**||  

> [!NOTE]  
>  创建新表时，适用于 SQL Server 的 OLE DB 驱动程序只映射前一个表中指定的 OLE DB 数据类型枚举值。 尝试创建其中某一列为任何其他 OLE DB 数据类型的表时将生成错误。  

## <a name="see-also"></a>另请参阅  
 [数据类型 (OLE DB)](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  
  
