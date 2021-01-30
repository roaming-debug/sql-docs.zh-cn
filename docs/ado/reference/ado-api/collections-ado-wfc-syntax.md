---
description: 集合（ADO - WFC 语法）
title: 集合 (ADO-WFC 语法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- syntax indexes [ADO], ADO/WFC
- collections [ADO], ADO/WFC syntax
- ADO/WFC syntax index [ADO]
ms.assetid: 073f9a0e-c755-42dd-9f71-4647d68e331a
author: rothja
ms.author: jroth
ms.openlocfilehash: d8fca0d29736fb9a57d9a8f72703d3870db15087
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171510"
---
# <a name="collections-ado---wfc-syntax"></a>集合（ADO - WFC 语法）
**包 .com. 数据**  
  
## <a name="parameters"></a>参数  
  
### <a name="methods"></a>方法  
  
```  
public void append(com.ms.wfc.data.Parameter param)  
public void delete(int n)  
public void delete(String s)  
public void refresh()  
public Parameter getItem(int n)  
public Parameter getItem(String s)  
```  
  
### <a name="properties"></a>属性  
  
```  
public int getCount()  
```  
  
## <a name="fields"></a>字段  
  
### <a name="methods"></a>方法  
  
```  
public void append(String name, int type)  
public void append(String name, int type, int definedSize)  
public void append(String name, int type, int definedSize, int attrib)  
public void delete(int n)  
public void delete(String s)  
public void refresh()  
public com.ms.wfc.data.Field getItem(int n)  
public com.ms.wfc.data.Field getItem(String s)  
```  
  
### <a name="properties"></a>属性  
  
```  
public int getCount()  
```  
  
## <a name="errors"></a>错误  
  
### <a name="methods"></a>方法  
  
```  
public void clear()  
public void refresh()  
public com.ms.wfc.data.Error getItem(int n)  
public com.ms.wfc.data.Error getItem(String s)  
```  
  
### <a name="properties"></a>属性  
  
```  
public int getCount()  
```  
  
## <a name="see-also"></a>另请参阅  
 [ADO)  (收集 ](./errors-collection-ado.md)   
 [字段集合 (ADO) ](./fields-collection-ado.md)   
 [参数集合 (ADO)](./parameters-collection-ado.md)