---
title: ISSDataClassification::GetSensitivityClassification | Microsoft Docs
description: ISSDataClassification::GetSensitivityClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification::GetSensitivityClassification
apitype: COM
helpviewer_keywords:
- GetSensitivityClassification method
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 232bcf6187967e9dab7b09414626785d7808e18f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837304"
---
# <a name="issdataclassificationgetsensitivityclassification"></a>ISSDataClassification::GetSensitivityClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  检索活动行集的敏感度分类数据。 有关详细信息和代码示例，请参阅[使用数据分类](../features/using-data-classification.md)。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT GetSensitivityClassification(
    SENSITIVITYCLASSIFICATION** ppSensitivityClassification);
```  
  
## <a name="arguments"></a>自变量  
  ppSensitivityClassification[out]  
 指向 SENSITIVITYCLASSIFICATION 结构指针的指针。 如果方法失败或者不存在可用的数据分类信息，则提供程序不会分配任何内存，并且会确保 ppSensitivityClassification 参数在输出时为一个空指针。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。    
  
 E_INVALIDARG  
 ppSensitivityClassification 参数为 Null。  
  
 E_OUTOFMEMORY  
 OLE DB Driver for SQL Server 无法分配足够的内存来完成请求。  

  
## <a name="remarks"></a>备注  
OLE DB Driver for SQL Server 分配内存块来保存 SENSITIVITYCLASSIFICATION 结构和此结构引用的数据。 当使用者不再需要访问分类数据时，使用者必须通过调用 [IMalloc::Free](/windows/win32/api/objidl/nf-objidl-imalloc-free) 方法释放该内存。  
  
 SENSITIVITYCLASSIFICATION 结构的定义如下所示：
  
```cpp
typedef struct tagSensitivityClassification
{
    USHORT                     cSensitivityLabels;
    SENSITIVITYLABEL          *rgSensitivityLabels;
    USHORT                     cInformationTypes;
    INFORMATIONTYPE           *rgInformationTypes;
    USHORT                     cColumnSensitivityMetadata;
    COLUMNSENSITIVITYMETADATA *rgColumnSensitivityMetadata;
    SENSITIVITYRANKENUM        eQuerySensitivityRank;
} SENSITIVITYCLASSIFICATION;
```  

|成员|描述|  
|------------|-----------------|  
|cSensitivityLabels|rgSensitivityLabels 中的 SENSITIVITYLABEL 结构数。|  
|rgSensitivityLabels|SENSITIVITYLABEL 结构的数组。|  
|cInformationTypes|rgInformationTypes 中的 INFORMATIONTYPE 结构数。|  
|rgInformationTypes|INFORMATIONTYPE 结构的数组。|  
|cColumnSensitivityMetadata|rgColumnSensitivityMetadata 中的 COLUMNSENSITIVITYMETADATA 结构数。|  
|rgColumnSensitivityMetadata|COLUMNSENSITIVITYMETADATA 结构的数组。|  
|eQuerySensitivityRank|用于获取行集的查询的敏感度的相对排名。|  

SENSITIVITYLABEL 结构的定义如下所示：
```cpp
typedef struct tagSENSITIVITYLABEL
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} SENSITIVITYLABEL;
```

|成员|描述|  
|------------|-----------------|  
|pwszName|敏感度标签的名称。|  
|pwszId|敏感度标签的标识符。|  

INFORMATIONTYPE 结构的定义如下所示：
```cpp
typedef struct tagINFORMATIONTYPE
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} INFORMATIONTYPE;
```

|成员|描述|  
|------------|-----------------|  
|pwszName|信息类型的名称。|  
|pwszId|信息类型的标识符。|  

COLUMNSENSITIVITYMETADATA 结构的定义如下所示：
```cpp
typedef struct tagCOLUMNSENSITIVITYMETADATA
{
    SENSITIVITYPROPERTY* rgSensitivityProperties;
    USHORT cSensitivityProperties;
} COLUMNSENSITIVITYMETADATA;
```

|成员|描述|  
|------------|-----------------|  
|cSensitivityProperties|rgSensitivityProperties 中的 SENSITIVITYPROPERTY 结构数。|  
|rgSensitivityProperties|SENSITIVITYPROPERTY 结构的数组。|  

SENSITIVITYRANKENUM 枚举定义如下：
```cpp
typedef enum tagSENSITIVITYRANKENUM
{
    SENSITIVITYRANK_NOT_DEFINED = -1,
    SENSITIVITYRANK_NONE = 0,
    SENSITIVITYRANK_LOW = 10,
    SENSITIVITYRANK_MEDIUM = 20,
    SENSITIVITYRANK_HIGH = 30,
    SENSITIVITYRANK_CRITICAL = 40
} SENSITIVITYRANKENUM;
```

SENSITIVITYPROPERTY 结构定义如下：
```cpp
typedef struct tagSENSITIVITYPROPERTY
{
    SENSITIVITYLABEL* pSensitivityLabel;
    INFORMATIONTYPE* pInformationType;
    SENSITIVITYRANKENUM eSensitivityRank;
} SENSITIVITYPROPERTY;
```

|成员|描述|  
|------------|-----------------|  
|pSensitivityLabel|指向 SENSITIVITYLABEL 结构的指针。|  
|pInformationType|指向 INFORMATIONTYPE 结构的指针。|  
|eSensitivityRank|作为每列数据的一部分的列的敏感度的相对排名。|  

## <a name="see-also"></a>另请参阅  
 [ISSDataClassification](../../oledb/ole-db-interfaces/issdataclassification-ole-db.md)  
 [行集](../ole-db-rowsets/rowsets.md)  
