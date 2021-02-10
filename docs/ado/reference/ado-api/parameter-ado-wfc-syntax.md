---
description: 参数（ADO - WFC 语法）
title: 参数 (ADO-WFC 语法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Parameter collection [ADO], ADO/WFC syntax
ms.assetid: d00d1e1e-14b1-41a2-a00f-2a3cb7396f15
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a8175c2510cfcd977a0144b15f510e1061dc4f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041167"
---
# <a name="parameter-ado---wfc-syntax"></a>参数（ADO - WFC 语法）
## <a name="package-commswfcdata"></a>包 .com. 数据  
  
### <a name="constructor"></a>构造函数  
  
```  
public Parameter()  
public Parameter(String name)  
public Parameter(String name, int type)  
public Parameter(String name, int type, int dir)  
public Parameter(String name, int type, int dir, int size)  
public Parameter(String name, int type, int dir, int size, Object value)  
```  
  
### <a name="methods"></a>方法  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
```  
  
### <a name="properties"></a>属性  
  
```  
public int getAttributes()  
public void setAttributes(int attr)  
public int getDirection()  
public void setDirection(int dir)  
public String getName()  
public void setName(String name)  
public int getNumericScale()  
public void setNumericScale(int scale)  
public int getPrecision()  
public void setPrecision(int prec)  
public int getSize()  
public void setSize(int size)  
public int getType()  
public void setType(int type)  
public com.ms.com.Variant getValue()  
public void setValue(Object v)  
public AdoProperties getProperties()  
```  
  
## <a name="parameter-accessor-methods"></a>参数访问器方法  
 [参数](./parameter-object.md)对象的[Value](./value-property-ado.md)属性可获取或设置该对象的内容。 内容表示为一种变量，一种可以为其分配值和多个数据类型的对象的类型。  
  
 ADO/WFC 使用 **getValue** 方法实现 **Value** 属性，该方法返回变量对象;和 **setValue** 方法，该方法采用变量作为参数。 变体在某些语言（如 Microsoft Visual Basic）中效率高。  
  
 除了 **Value** 属性，ADO/WFC 还提供使用 Java 数据类型获取和设置 **参数** 对象内容的 *访问器* 方法。 这些方法中的大多数都具有格式为 **获取**_数据_ 类型或 **设置**_数据类型_ 的名称。  
  
 有一个值得注意的例外：没有 **getNull** 属性;相反，有一个 **isNull** 属性，它返回一个布尔值，指示该字段是否为 null。  
  
```  
public boolean getBoolean()  
public void setBoolean(boolean v)  
public byte getByte()  
public void setByte(byte v)  
public double getDouble()  
public void setDouble(double v)  
public float getFloat()  
public void setFloat(float v)  
public int getInt()  
public void setInt(int v)  
public long getLong()  
public void setLong(long v)  
public short getShort()  
public void setShort(short v)  
public String getString()  
public void setString(String v)  
public boolean isNull()  
public void setNull()  
```  
  
## <a name="see-also"></a>另请参阅  
 [参数对象](./parameter-object.md)