---
description: 'ISSCommandWithParameters：： GetParameterProperties SQL Server Native Client (OLE DB) '
title: ISSCommandWithParameters::GetParameterProperties (OLE DB)
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
ms.assetid: 7f4cc5ea-d028-4fe5-9192-bd153ab3c26c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c641a32268a793b17a9b000d7aa6cdcd663e5e28
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750077"
---
# <a name="isscommandwithparametersgetparameterproperties-in-sql-server-native-client-ole-db"></a>ISSCommandWithParameters：： GetParameterProperties SQL Server Native Client (OLE DB) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回 SSPARAMPROPS 属性集结构的数组，每个 UDT 或 XML 参数对应一个 SSPARAMPROPS 属性集。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>参数  
 pcParams[out][in]   
 一个指向内存的指针，该内存包含 prgParamProperties 中返回的 SSPARAMPROPS 结构数量  。  
  
 prgParamProperties[out]   
 指向内存中将返回 SSPARAMPROPS 结构数组的位置的指针。 提供程序为结构分配内存并返回此内存的地址;当使用者不再需要结构时，将使用 **IMalloc：： Free** 释放此内存。 在调用 **IMalloc：： Free** for *prgParamProperties* 之前，使用者还必须调用每个 DBPROP 结构的 *vValue* 属性的 **VariantClear** ，以防止在变体包含引用类型 (例如 BSTR）的情况下发生内存泄漏。 ) 如果在输出中 *pcParams* 为零或发生除 DB_E_ERRORSOCCURRED 之外的错误，则访问接口不会分配任何内存，并确保 *prgParamProperties* 在输出时为 null 指针。  
  
## <a name="return-code-values"></a>返回代码值  
 **GetParameterProperties** 方法返回与 Core OLE DB **ICommandProperties：： GetProperties** 方法相同的错误代码，但不能引发 DB_S_ERRORSOCCURRED 和 DB_E_ERRORSOCCURED。  
  
## <a name="remarks"></a>备注  
 对于 **GetParameterInfo**， **ISSCommandWithParameters：： GetParameterProperties** 的行为一致。 如果未调用 [ISSCommandWithParameters：： SetParameterProperties](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) 或 **SetParameterInfo** cParams 等于零，则 **GetParameterInfo** 将派生参数信息并返回 this。 如果已为至少一个参数调用了 **ISSCommandWithParameters：： SetParameterProperties** 或 **SetParameterInfo** ，则 **ISSCommandWithParameters：： GetParameterProperties** 仅返回已调用 **ISSCommandWithParameters：： SetParameterProperties** 的那些参数的属性。 如果在 **ISSCommandWithParameters：： GetParameterProperties** 或 **GetParameterInfo** 后调用 **ISSCommandWithParameters：： SetParameterProperties** ，则对 **ISSCommandWithParameters：： GetParameterProperties** 的后续调用将返回为其调用 **ISSCommandWithParameters：： SetParameterProperties** 的那些参数的重写值。  
  
 SSPARAMPROPS 结构的定义如下所示：  

```cpp
struct SSPARAMPROPS {
    DBORDINAL iOrdinal;
    ULONG cPropertySets;
    DBPROPSET *rgPropertySets;
};
```

|成员|说明|  
|------------|-----------------|  
|iOrdinal |所传递参数的序号。|  
|cPropertySets |rgPropertySets 中 DBPROPSET 结构的数量  。|  
|rgPropertySets |指向内存中将返回 DBPROPSET 结构数组的位置的指针。|  
|||

## <a name="see-also"></a>另请参阅  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
