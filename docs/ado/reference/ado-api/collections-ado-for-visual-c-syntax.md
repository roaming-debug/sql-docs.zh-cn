---
description: 集合（ADO for Visual C++ 语法）
title: Visual C++ 语法) 的 ADO (集合 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- ADO for Visual C++ syntax [ADO]
- syntax indexes [ADO], ADO for Visual C++ syntax
- collections [ADO], ADO for Visual C++ syntax
ms.assetid: 6a0109a0-f2d9-4f7c-8e1e-42763f9acaea
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cda61fc0d1d206a75059263c5450c813a9e5ddb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164685"
---
# <a name="collections-ado-for-visual-c-syntax"></a>集合（ADO for Visual C++ 语法）
## <a name="parameters"></a>参数  
  
### <a name="methods"></a>方法  
  
```  
Append(IDispatch *Object);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 有关详细信息，请参阅  
  
-   [Append 方法 (ADO)](./append-method-ado.md)  
  
-   [Delete 方法（ADO 参数集合）](./delete-method-ado-parameters-collection.md)  
  
-   [Refresh 方法 (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>属性  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, _ADOParameter **ppvObject);  
```  
  
 有关详细信息，请参阅  
  
-   [Count 属性 (ADO)](./count-property-ado.md)  
  
-   [Item 属性 (ADO)](./item-property-ado.md)  
  
## <a name="fields"></a>字段  
  
### <a name="methods"></a>方法  
  
```  
Append(BSTR bstrName, DataTypeEnum Type, long DefinedSize, FieldAttributeEnum Attrib);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 有关详细信息，请参阅  
  
-   [Append 方法 (ADO)](./append-method-ado.md)  
  
-   [Delete 方法（ADO 参数集合）](./delete-method-ado-parameters-collection.md)  
  
-   [Refresh 方法 (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>属性  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOField **ppvObject);  
```  
  
 有关详细信息，请参阅  
  
-   [Count 属性 (ADO)](./count-property-ado.md)  
  
-   [Item 属性 (ADO)](./item-property-ado.md)  
  
## <a name="errors"></a>错误  
  
### <a name="methods"></a>方法  
  
```  
Clear(void);  
Refresh(void);  
```  
  
 有关详细信息，请参阅  
  
-   [Clear 方法 (ADO)](./clear-method-ado.md)  
  
-   [Refresh 方法 (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>属性  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOError **ppvObject);  
```  
  
 有关详细信息，请参阅  
  
-   [Count 属性 (ADO)](./count-property-ado.md)  
  
-   [Item 属性 (ADO)](./item-property-ado.md)  
  
## <a name="properties"></a>属性  
  
### <a name="methods"></a>方法  
  
```  
Refresh(void);  
```  
  
 有关详细信息，请参阅  
  
-   [Refresh 方法 (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>属性  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOProperty **ppvObject);  
```  
  
 有关详细信息，请参阅  
  
-   [Count 属性 (ADO)](./count-property-ado.md)  
  
-   [Item 属性 (ADO)](./item-property-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO)  (收集 ](./errors-collection-ado.md)   
 [字段集合 (ADO) ](./fields-collection-ado.md)   
 [ADO) 的参数集合 (](./parameters-collection-ado.md)   
 [属性集合 (ADO)](./properties-collection-ado.md)