---
title: 创建 SQL Server 表（OLE DB 驱动器）| Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 中的 ITableDefinition::CreateTable 函数如何允许使用者创建 SQL Server 表。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- OLE DB Driver for SQL Server, tables
- DBCOLUMNDESC usage
- adding tables
- CreateTable function
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cab38e1c68bc12cab536090cc074cab14d97a48e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754267"
---
# <a name="creating-sql-server-tables"></a>创建 SQL Server 表
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序公开 ITableDefinition::CreateTable 函数，让使用者能够创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表  。 使用者通过 CreateTable 创建使用者命名的永久表，并创建具有适用于 SQL Server 的 OLE DB 驱动程序生成的唯一名称的永久表或临时表  。  
  
 当使用者调用 ITableDefinition::CreateTable 时，如果 DBPROP_TBL_TEMPTABLE 属性的值为 VARIANT_TRUE，则适用于 SQL Server 的 OLE DB 驱动程序为使用者生成一个临时表名  。 使用者将 CreateTable 方法的 pTableID 参数设置为 NULL   。 适用于 SQL Server 的 OLE DB 驱动程序生成的名称唯一的临时表不出现在 TABLES 行集中，但可通过 IOpenRowset 接口进行访问   。  
  
 当使用者在 pTableID 参数的 uName 联合的 pwszName 成员中指定表名时，适用于 SQL Server 的 OLE DB 驱动程序将使用该名称创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表    。 将应用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表命名约束，并且表名可以指示永久表或者指示本地或全局临时表。 有关详细信息，请参阅 [CREATE TABLE](../../../t-sql/statements/create-table-transact-sql.md)。 ppTableID 参数可为 NULL  。  
  
 OLE DB Driver for SQL Server 可以生成永久表或临时表的名称。 当使用者将 pTableID 参数设置为 NULL，并将 ppTableID 设置为指向有效的 DBID\* 时，适用于 SQL Server 的 OLE DB 驱动程序在 pwszName 成员中返回所生成的表名，其中该成员属于 ppTableID 的值指向的 DBID 的 uName 联合      。 要创建由适用于 SQL Server 的 OLE DB 驱动程序命名的临时表，使用者可在 rgPropertySets 参数引用的表属性集中包含 OLE DB 表属性 DBPROP_TBL_TEMPTABLE  。 OLE DB Driver for SQL Server 命名的临时表是本地表。  
  
 如果 pTableID 参数的 eKind 成员未指示 DBKIND_NAME，CreateTable 将返回 DB_E_BADTABLEID    。  
  
## <a name="dbcolumndesc-usage"></a>DBCOLUMNDESC 用法  
 使用者可通过使用 pwszTypeName 成员或 wType 成员指示列数据类型   。 如果使用者在 pwszTypeName  中指定数据类型，OLE DB Driver for SQL Server 会忽略 wType  的值。  
  
 如果使用 pwszTypeName 成员，则使用者将通过使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型名称指定数据类型  。 有效的数据类型名称是在 PROVIDER_TYPES 架构行集的 TYPE_NAME 列中返回的名称。  
  
 OLE DB Driver for SQL Server 在 wType  成员中识别 OLE DB 枚举的 DBTYPE 值的子集。 有关详细信息，请参阅 [ITableDefinition 中的数据类型映射](../../oledb/ole-db-data-types/data-type-mapping-in-itabledefinition.md)。  
  
> [!NOTE]  
>  如果使用者设置 pTypeInfo 或 pclsid 成员以指定列数据类型，则 CreateTable 返回 DB_E_BADTYPE    。  
  
 使用者在 DBCOLUMNDESC dbcid 成员的 uName 联合的 pwszName 成员中指定列名    。 该列名指定为 Unicode 字符串。 dbcid 的 eKind 成员必须为 DBKIND_NAME   。 如果 eKind 无效且 pwszName 为 NULL，或者如果 pwszName 的值不是有效的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识符，则 CreateTable 返回 DB_E_BADCOLUMNID     。  
  
 所有列属性均可用于为表定义的所有列上。 如果所设置的属性值相互冲突，则 CreateTable 可能返回 DB_S_ERRORSOCCURRED 或 DB_E_ERRORSOCCURRED  。 当无效的列属性设置导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表创建失败时，CreateTable 将返回错误  。  
  
 DBCOLUMNDESC 中的列属性解释如下。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_COL_AUTOINCREMENT|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE 说明：在创建的列上设置标识属性。 对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，标识属性对表中的单一列有效。 如果将多列的此属性设置为 VARIANT_TRUE，则当适用于 SQL Server 的 OLE DB 驱动程序尝试在服务器上创建该表时，将生成错误。<br /><br /> 当小数位数为 0 时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识属性仅对 integer、numeric 和 decimal 类型有效    。 如果在任何其他数据类型的列上将此属性设置为 VARIANT_TRUE，则当适用于 SQL Server 的 OLE DB 驱动程序尝试在服务器上创建该表时，将生成错误。<br /><br /> 当 DBPROP_COL_AUTOINCREMENT 和 DBPROP_COL_NULLABLE 均为 VARIANT_TRUE，且 DBPROP_COL_NULLABLE 的 dwOption 不为 DBPROPOPTIONS_REQUIRED 时，适用于 SQL Server 的 OLE DB 驱动程序返回 DB_S_ERRORSOCCURRED  。 当 DBPROP_COL_AUTOINCREMENT 和 DBPROP_COL_NULLABLE 均为 VARIANT_TRUE，且 DBPROP_COL_NULLABLE 的 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_NULLABLE dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。|  
|DBPROP_COL_DEFAULT|R/W：读取/写入<br /><br /> 默认值：无<br /><br /> 说明:创建列的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] DEFAULT 约束。<br /><br /> vValue DBPROP 成员可以属于许多类型中的任何一种  。 vValue.vt 成员指定的类型应与列的数据类型兼容  。 例如，对于定义为 DBTYPE_WSTR 的某列将 BSTR N/A 定义为默认值就是一个兼容的匹配。 如果在定义为 DBTYPE_R8 的列上定义同一默认值，则当适用于 SQL Server 的 OLE DB 驱动程序尝试在服务器上创建该表时，将生成错误。|  
|DBPROP_COL_DESCRIPTION|R/W：读取/写入<br /><br /> 默认值：无<br /><br /> 说明:DBPROP_COL_DESCRIPTION 列属性不是由 OLE DB Driver for SQL Server 实现。<br /><br /> 当使用者尝试写入属性值时，DBPROP 结构的 dwStatus 成员将返回 DBPROPSTATUS_NOTSUPPORTED  。<br /><br /> 设置此属性不会对 OLE DB Driver for SQL Server 构成致命错误。 如果所有其他参数值有效，则创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。|  
|DBPROP_COL_FIXEDLENGTH|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明:如果使用者使用 DBCOLUMNDESC 的 wType  成员来定义列的数据类型，OLE DB Driver for SQL Server 使用 DBPROP_COL_FIXEDLENGTH 来确定数据类型映射。 有关详细信息，请参阅 [ITableDefinition 中的数据类型映射](../../oledb/ole-db-data-types/data-type-mapping-in-itabledefinition.md)。|  
|DBPROP_COL_NULLABLE|R/W：读取/写入<br /><br /> 默认值：无<br /><br /> 说明:如果此属性已设置，OLE DB Driver for SQL Server 会在创建表时指明列是否应接受 NULL 值。 如果未设置此属性，则该列能否接受 NULL 作为值将由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ANSI_NULLS 默认数据库选项确定。<br /><br /> OLE DB Driver for SQL Server 是符合 ISO 规定的提供程序。 所连接的会话展现 ISO 行为。 如果使用者未设置 DBPROP_COL_NULLABLE，则列接受 NULL 值。|  
|DBPROP_COL_PRIMARYKEY|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE 说明：若为 VARIANT_TRUE，OLE DB Driver for SQL Server 创建具有 PRIMARY KEY 约束的列。<br /><br /> 当定义为列属性时，仅单一列能够确定约束。 如果在多个列上将此属性设置为 VARIANT_TRUE，则当适用于 SQL Server 的 OLE DB 驱动程序尝试创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表时，将返回错误。<br /><br /> 注意：使用者可以使用 IIndexDefinition::CreateIndex  在两列或更多列上创建 PRIMARY KEY 约束。<br /><br /> 当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 DBPROP_COL_UNIQUE 的 dwOption 不为 DBPROPOPTIONS_REQUIRED 时，适用于 SQL Server 的 OLE DB 驱动程序返回 DB_S_ERRORSOCCURRED  。<br /><br /> 当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 DBPROP_COL_UNIQUE 的 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_PRIMARYKEY dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。<br /><br /> 当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_NULLABLE 均为 VARIANT_TRUE 时，适用于 SQL Server 的 OLE DB 驱动程序返回错误。<br /><br /> 当使用者尝试在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型无效的列上创建 PRIMARY KEY 约束时，适用于 SQL Server 的 OLE DB 驱动程序将从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 返回错误。 对于使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型 bit、text、ntext 和 image 创建的列，无法定义 PRIMARY KEY 约束     。|  
|DBPROP_COL_UNIQUE|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE 说明：将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] UNIQUE 约束应用于列。<br /><br /> 当定义为列属性时，此约束仅应用于单一列。 使用者可以使用 IIndexDefinition::CreateIndex 将 UNIQUE 约束应用于两列或更多列的组合值  。<br /><br /> DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 dwOption 不为 DBPROPOPTIONS_REQUIRED 时，适用于 SQL Server 的 OLE DB 驱动程序返回 DB_S_ERRORSOCCURRED  。<br /><br /> 当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，将返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_PRIMARYKEY dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。<br /><br /> DBPROP_COL_NULLABLE 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 dwOption 不为 DBPROPOPTIONS_REQUIRED 时，适用于 SQL Server 的 OLE DB 驱动程序返回 DB_S_ERRORSOCCURRED  。<br /><br /> 当 DBPROP_COL_NULLABLE 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，将返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_NULLABLE dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。<br /><br /> 当使用者尝试在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型无效的列创建 UNIQUE 约束时，适用于 SQL Server 的 OLE DB 驱动程序将从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 返回错误。 无法在使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bit  数据类型创建的列上定义 UNIQUE 约束。|  
  
 当使用者调用 ITableDefinition::CreateTable 时，适用于 SQL Server 的 OLE DB 驱动程序如下解释表属性  。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_TBL_TEMPTABLE|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE 说明：默认情况下，OLE DB Driver for SQL Server 创建使用者命名的表。 若为 VARIANT_TRUE，OLE DB Driver for SQL Server 为使用者生成临时表名。 使用者将 CreateTable 的 pTableID 参数设置为 NULL   。 ppTableID 参数必须包含有效指针  。|  
  
 如果使用者请求在成功创建的表中打开行集，则适用于 SQL Server 的 OLE DB 驱动程序会打开游标支持的行集。 可以在所传递的属性集中指示任何行集属性。  
  
 此示例创建一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。  
  
```  
// This CREATE TABLE statement shows the details of the table created by   
// the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//    CONSTRAINT PK_OrderDetails  
//         PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
// The PRIMARY KEY constraint is created in an additional example.  
HRESULT CreateTable  
    (  
    ITableDefinition* pITableDefinition  
    )  
    {  
    DBID            dbidTable;  
    const ULONG     nCols = 5;  
    ULONG           nCol;  
    ULONG           nProp;  
    DBCOLUMNDESC    dbcoldesc[nCols];  
  
    HRESULT         hr;  
  
    // Set up column descriptions. First, set default property values for  
    //  the columns.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbcoldesc[nCol].pwszTypeName = NULL;  
        dbcoldesc[nCol].pTypeInfo = NULL;  
        dbcoldesc[nCol].rgPropertySets = new DBPROPSET;  
        dbcoldesc[nCol].pclsid = NULL;  
        dbcoldesc[nCol].cPropertySets = 1;  
        dbcoldesc[nCol].ulColumnSize = 0;  
        dbcoldesc[nCol].dbcid.eKind = DBKIND_NAME;  
        dbcoldesc[nCol].wType = DBTYPE_I4;  
        dbcoldesc[nCol].bPrecision = 0;  
        dbcoldesc[nCol].bScale = 0;  
  
        dbcoldesc[nCol].rgPropertySets[0].rgProperties =   
            new DBPROP[NCOLPROPS_MAX];  
        dbcoldesc[nCol].rgPropertySets[0].cProperties = NCOLPROPS_MAX;  
        dbcoldesc[nCol].rgPropertySets[0].guidPropertySet =  
            DBPROPSET_COLUMN;  
  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                dwOptions = DBPROPOPTIONS_REQUIRED;  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].colid  
                 = DB_NULLID;  
  
            VariantInit(  
                &(dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                    vValue));  
  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt = VT_BOOL;  
            }  
        }  
  
    // Set the column-specific information.  
    dbcoldesc[0].dbcid.uName.pwszName = L"OrderID";  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[0].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[1].dbcid.uName.pwszName = L"ProductID";  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[1].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[2].dbcid.uName.pwszName = L"UnitPrice";  
    dbcoldesc[2].wType = DBTYPE_CY;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[2].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[3].dbcid.uName.pwszName = L"Quantity";  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[3].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[4].dbcid.uName.pwszName = L"Discount";  
    dbcoldesc[4].wType = DBTYPE_NUMERIC;  
    dbcoldesc[4].bPrecision = 2;  
    dbcoldesc[4].bScale = 2;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].dwPropertyID =   
        DBPROP_COL_DEFAULT;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.vt = VT_BSTR;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.bstrVal =  
        SysAllocString(L"0");  
    dbcoldesc[4].rgPropertySets[0].cProperties = 2;  
  
    // Set up the dbid for OrderDetails.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    if (FAILED(hr = pITableDefinition->CreateTable(NULL, &dbidTable,  
        nCols, dbcoldesc, NULL, 0, NULL, NULL, NULL)))  
        {  
        DumpError(pITableDefinition, IID_ITableDefinition);  
        goto SAFE_EXIT;  
        }  
  
SAFE_EXIT:  
    // Clean up dynamic allocation in the property sets.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            if (dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt == VT_BSTR)  
                {  
                SysFreeString(dbcoldesc[nCol].rgPropertySets[0].  
                    rgProperties[nProp].vValue.bstrVal);  
                }  
            }  
  
        delete [] dbcoldesc[nCol].rgPropertySets[0].rgProperties;  
        delete [] dbcoldesc[nCol].rgPropertySets;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>另请参阅  
 [表和索引](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)  
  
  
