---
title: " (Native Client OLE DB 提供程序创建 SQL Server 表) |Microsoft Docs"
description: 了解 SQL Server Native Client OLE DB 提供程序如何公开允许使用者创建 SQL Server 表的函数。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- SQL Server Native Client OLE DB provider, tables
- DBCOLUMNDESC usage
- adding tables
- CreateTable function
ms.assetid: a7b8d142-d76a-44d9-a583-86ac5109fbe8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f2dfb1ab5997d92087ac8860cae000c85bbf21be
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743747"
---
# <a name="creating-sql-server-native-client-tables"></a>创建 SQL Server Native Client 表
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序公开 **ITableDefinition：： CreateTable** 函数，允许使用者创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 使用者使用 **CreateTable** 来创建使用者命名的永久表，并使用由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序生成的唯一名称的永久或临时表。  
  
 当使用者调用 **ITableDefinition：： CreateTable** 时，如果 DBPROP_TBL_TEMPTABLE 属性的值 VARIANT_TRUE，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将为使用者生成一个临时表名称。 使用者将 CreateTable 方法的 pTableID 参数设置为 NULL   。 使用 Native Client OLE DB 提供程序生成的名称的临时表 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会显示在 **表** 行集中，而是可以通过 **IOpenRowset** 接口进行访问。  
  
 当使用者在 *pTableID* 参数的 *UName* 联合的 *pwszName* 成员中指定表名时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用该名称创建一个表。 将应用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表命名约束，并且表名可以指示永久表或者指示本地或全局临时表。 有关详细信息，请参阅 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)。 ppTableID 参数可为 NULL  。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序可以生成永久或临时表的名称。 当使用者将 *pTableID* 参数设置为 NULL 并将 *ppTableID* 设置为指向有效的 DBID 时 \* ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将在 *PpTableID* 的值指向的 dbid 的 *pwszName* 成员中返回表的生成名称。 若要创建临时的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序命名的表，使用者在 *rgPropertySets* 参数中引用的表属性集中包含 OLE DB 的表属性 DBPROP_TBL_TEMPTABLE。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序命名的临时表是本地的。  
  
 如果 pTableID 参数的 eKind 成员未指示 DBKIND_NAME，CreateTable 将返回 DB_E_BADTABLEID    。  
  
## <a name="dbcolumndesc-usage"></a>DBCOLUMNDESC 用法  
 使用者可通过使用 pwszTypeName 成员或 wType 成员指示列数据类型   。 如果使用者在 *pwszTypeName* 中指定数据类型，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将忽略 *wType* 的值。  
  
 如果使用 pwszTypeName 成员，则使用者将通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型名称指定数据类型  。 有效的数据类型名称是在 PROVIDER_TYPES 架构行集的 TYPE_NAME 列中返回的名称。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序识别 *wType* 成员中 OLE DB 枚举的 DBTYPE 值的子集。 有关详细信息，请参阅 [ITableDefinition 中的数据类型映射](../../relational-databases/native-client-ole-db-data-types/data-type-mapping-in-itabledefinition.md)。  
  
> [!NOTE]  
>  如果使用者设置 pTypeInfo 或 pclsid 成员以指定列数据类型，则 CreateTable 返回 DB_E_BADTYPE    。  
  
 使用者在 DBCOLUMNDESC dbcid 成员的 uName 联合的 pwszName 成员中指定列名    。 该列名指定为 Unicode 字符串。 dbcid 的 eKind 成员必须为 DBKIND_NAME   。 如果 eKind 无效且 pwszName 为 NULL，或者如果 pwszName 的值不是有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符，则 CreateTable 返回 DB_E_BADCOLUMNID     。  
  
 所有列属性均可用于为表定义的所有列上。 如果所设置的属性值相互冲突，则 CreateTable 可能返回 DB_S_ERRORSOCCURRED 或 DB_E_ERRORSOCCURRED  。 当无效的列属性设置导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表创建失败时，CreateTable 将返回错误  。  
  
 DBCOLUMNDESC 中的列属性解释如下。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_COL_AUTOINCREMENT|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE 说明：在创建的列上设置标识属性。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，标识属性对表中的单一列有效。 如果将多个列的属性设置为 VARIANT_TRUE，则当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序尝试在服务器上创建表时，将生成错误。<br /><br /> 当小数位数为 0 时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识属性仅对 integer、numeric 和 decimal 类型有效    。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序尝试在服务器上创建表时，将属性设置为对任何其他数据类型的列 VARIANT_TRUE 将生成错误。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 DBPROP_COL_AUTOINCREMENT 和 DBPROP_COL_NULLABLE 均为 VARIANT_TRUE 并且未 DBPROP_COL_NULLABLE DBPROPOPTIONS_REQUIRED 的 *dwOption* 时，Native Client OLE DB 提供程序将返回 DB_S_ERRORSOCCURRED。 当 DBPROP_COL_AUTOINCREMENT 和 DBPROP_COL_NULLABLE 均为 VARIANT_TRUE，且 DBPROP_COL_NULLABLE 的 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_NULLABLE dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。|  
|DBPROP_COL_DEFAULT|R/W：读取/写入<br /><br /> 默认值：无<br /><br /> 说明:创建列的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DEFAULT 约束。<br /><br /> vValue DBPROP 成员可以属于许多类型中的任何一种  。 vValue.vt 成员指定的类型应与列的数据类型兼容  。 例如，对于定义为 DBTYPE_WSTR 的某列将 BSTR N/A 定义为默认值就是一个兼容的匹配。 如果对定义为 DBTYPE_R8 的列定义相同的默认值，则当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序尝试在服务器上创建表时，将生成错误。|  
|DBPROP_COL_DESCRIPTION|R/W：读取/写入<br /><br /> 默认值：无<br /><br /> 说明： DBPROP_COL_DESCRIPTION 列属性不是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序实现的。<br /><br /> 当使用者尝试写入属性值时，DBPROP 结构的 dwStatus 成员将返回 DBPROPSTATUS_NOTSUPPORTED  。<br /><br /> 设置属性不会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序提供致命错误。 如果所有其他参数值有效，则创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。|  
|DBPROP_COL_FIXEDLENGTH|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当使用者使用 DBCOLUMNDESC 的 *wType* 成员定义列的数据类型时，Native Client OLE DB 提供程序使用 DBPROP_COL_FIXEDLENGTH 确定数据类型映射。 有关详细信息，请参阅 [ITableDefinition 中的数据类型映射](../../relational-databases/native-client-ole-db-data-types/data-type-mapping-in-itabledefinition.md)。|  
|DBPROP_COL_NULLABLE|R/W：读取/写入<br /><br /> 默认值：无<br /><br /> 说明：在创建表时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序指示列是否应在设置了属性时接受 null 值。 如果未设置此属性，则该列能否接受 NULL 作为值将由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ANSI_NULLS 默认数据库选项确定。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序是符合 ISO 要求的提供程序。 所连接的会话展现 ISO 行为。 如果使用者未设置 DBPROP_COL_NULLABLE，则列接受 NULL 值。|  
|DBPROP_COL_PRIMARYKEY|R/W：读取/写入<br /><br /> 默认值： VARIANT_FALSE 说明： VARIANT_TRUE 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将创建具有 PRIMARY KEY 约束的列。<br /><br /> 当定义为列属性时，仅单一列能够确定约束。 如果将多个列的属性 VARIANT_TRUE 设置为，则当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序尝试创建该表时，将返回错误 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br /> 注意：使用者可以使用 IIndexDefinition::CreateIndex  在两列或更多列上创建 PRIMARY KEY 约束。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE 并且未 DBPROP_COL_UNIQUE DBPROPOPTIONS_REQUIRED 的 *dwOption* 时，Native Client OLE DB 提供程序将返回 DB_S_ERRORSOCCURRED。<br /><br /> 当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 DBPROP_COL_UNIQUE 的 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_PRIMARYKEY dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_NULLABLE 都 VARIANT_TRUE 时，Native Client OLE DB 提供程序将返回错误。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当使用者尝试对无效数据类型的列创建 PRIMARY KEY 约束时，Native Client OLE DB 提供程序将返回错误 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 对于使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型 bit、text、ntext 和 image 创建的列，无法定义 PRIMARY KEY 约束     。|  
|DBPROP_COL_UNIQUE|R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE 说明：将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] UNIQUE 约束应用于列。<br /><br /> 当定义为列属性时，此约束仅应用于单一列。 使用者可以使用 IIndexDefinition::CreateIndex 将 UNIQUE 约束应用于两列或更多列的组合值  。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 都是 VARIANT_TRUE 并且 *dwOption* 未 DBPROPOPTIONS_REQUIRED 时，Native Client OLE DB 提供程序将返回 DB_S_ERRORSOCCURRED。<br /><br /> 当 DBPROP_COL_PRIMARYKEY 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，将返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_PRIMARYKEY dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]当 DBPROP_COL_NULLABLE 和 DBPROP_COL_UNIQUE 都是 VARIANT_TRUE 并且 *dwOption* 未 DBPROPOPTIONS_REQUIRED 时，Native Client OLE DB 提供程序将返回 DB_S_ERRORSOCCURRED。<br /><br /> 当 DBPROP_COL_NULLABLE 和 DBPROP_COL_UNIQUE 均为 VARIANT_TRUE，且 dwOption 等于 DBPROPOPTIONS_REQUIRED 时，将返回 DB_E_ERRORSOCCURRED  。 此列使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识属性进行定义，并且 DBPROP_COL_NULLABLE dwStatus 成员设置为 DBPROPSTATUS_CONFLICTING  。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当使用者尝试对无效数据类型的列创建 UNIQUE 约束时，Native Client OLE DB 提供程序将返回错误 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 无法在使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bit  数据类型创建的列上定义 UNIQUE 约束。|  
  
 当使用者调用 **ITableDefinition：： CreateTable** 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将按如下所示解释表属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_TBL_TEMPTABLE|R/W：读取/写入<br /><br /> 默认值： VARIANT_FALSE 说明：默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将创建使用者命名的表。 VARIANT_TRUE 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将为使用者生成一个临时表名称。 使用者将 CreateTable 的 pTableID 参数设置为 NULL   。 ppTableID 参数必须包含有效指针  。|  
  
 如果使用者请求在成功创建的表上打开行集，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序会打开游标支持的行集。 可以在所传递的属性集中指示任何行集属性。  
  
 此示例创建一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
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
 [表和索引](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
  
