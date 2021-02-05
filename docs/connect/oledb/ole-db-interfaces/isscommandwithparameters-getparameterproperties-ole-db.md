---
title: ISSCommandWithParameters::GetParameterProperties (OLE DB) | Microsoft Docs
description: ISSCommandWithParameters::GetParameterProperties 会返回一个数组，其中包含 OLE DB Driver for SQL Server 中的属性集结构，并且会分别为每个 UDT 或 XML 参数返回一个。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b820f2bb9fe248b8ca4645024214b19112c56ad8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183691"
---
# <a name="isscommandwithparametersgetparameterproperties-ole-db"></a>ISSCommandWithParameters::GetParameterProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  返回 SSPARAMPROPS 属性集结构的数组，每个 UDT 或 XML 参数对应一个 SSPARAMPROPS 属性集。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>参数  
 pcParams[out][in]   
 一个指向内存的指针，该内存包含 prgParamProperties 中返回的 SSPARAMPROPS 结构数量  。  
  
 prgParamProperties[out]   
 指向内存中将返回 SSPARAMPROPS 结构数组的位置的指针。 提供程序为结构分配内存并返回此内存的地址；当使用者不再需要这些结构时，可以使用 `IMalloc::Free` 释放该内存。 在为 prgParamProperties 调用 `IMalloc::Free` 之前，使用者还必须为每个 DBPROP 结构的 vValue 属性调用 `VariantClear`，以防止当变量包含引用类型（如 BSTR）时发生内存泄漏。 如果 pcParams 在输出时为零或者发生了 DB_E_ERRORSOCCURRED 之外的错误，则提供程序不分配任何内存，且确保 prgParamProperties 在输出时是一个空指针   。  
  
## <a name="return-code-values"></a>返回代码值  
 `GetParameterProperties` 方法返回与核心 OLE DB `ICommandProperties::GetProperties` 方法相同的错误代码，只是不引发 DB_S_ERRORSOCCURRED 和 DB_E_ERRORSOCCURED。  
  
## <a name="remarks"></a>备注  
 `ISSCommandWithParameters::GetParameterProperties` 方法与 `GetParameterInfo` 的行为一致。 如果未调用 [`ISSCommandWithParameters::SetParameterProperties`](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) 或 `SetParameterInfo`，或在 cParams 等于零的情况下调用了它们，`GetParameterInfo` 将派生参数信息并返回该信息。 如果至少对一个参数调用了 `ISSCommandWithParameters::SetParameterProperties` 或 `SetParameterInfo`，则 `ISSCommandWithParameters::GetParameterProperties` 方法仅返回调用的 `ISSCommandWithParameters::SetParameterProperties` 所针对的参数的属性。 如果在调用 `ISSCommandWithParameters::GetParameterProperties` 或 `GetParameterInfo` 之后调用了 `ISSCommandWithParameters::SetParameterProperties`，则对 `ISSCommandWithParameters::GetParameterProperties` 的后续调用将返回调用的 `ISSCommandWithParameters::SetParameterProperties` 方法所针对的参数的重写值。  
  
 SSPARAMPROPS 结构的定义如下所示：  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
|成员|说明|  
|------------|-----------------|  
|iOrdinal |所传递参数的序号。|  
|cPropertySets |rgPropertySets 中 DBPROPSET 结构的数量  。|  
|rgPropertySets |指向内存中将返回 DBPROPSET 结构数组的位置的指针。|  
  
## <a name="see-also"></a>另请参阅  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
