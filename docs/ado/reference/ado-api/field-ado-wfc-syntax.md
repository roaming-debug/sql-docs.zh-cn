---
description: 字段（ADO - WFC 语法）
title: 字段 (ADO-WFC 语法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Field collection [ADO], ADO/WFC syntax
ms.assetid: 7e01cb24-2338-4f92-ad46-8d97248e1a4d
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d998deb2d09e77e54ba9c733e371cc60a439761
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034097"
---
# <a name="field-ado---wfc-syntax"></a>字段（ADO - WFC 语法）
## <a name="package-commswfcdata"></a>包 .com. 数据  
  
### <a name="methods"></a>方法  
  
```  
public void appendChunk(byte[] bytes)  
public void appendChunk(char[] chars)  
public void appendChunk(String chars)  
public byte[] getByteChunk(int len)  
public char[] getCharChunk(int len)  
public String getStringChunk(int len)  
```  
  
### <a name="properties"></a>属性  
  
```  
public int getActualSize()  
public int getAttributes()  
public void setAttributes(int pl)  
public com.ms.com.IUnknown getDataFormat()  
public void setDataFormat(com.ms.com.IUnknown format)  
```  
  
  (有关详细信息，请参阅 IDataFormat 接口的文档。 )   
  
```  
public int getDefinedSize()  
public void setDefinedSize(int pl)  
public String getName()  
public int getNumericScale()  
public void setNumericScale(byte pbNumericScale)  
public Variant getOriginalValue()  
public int getPrecision()  
public void setPrecision(byte pbPrecision)  
public int getType()  
public void setType(int pDataType)  
public Variant getUnderlyingValue()  
public Variant getValue()  
public void setValue(Variant value)  
public AdoProperties getProperties()  
```  
  
### <a name="field-accessor-methods"></a>字段访问器方法  
 [Field](../../../ado/reference/ado-api/field-object.md)对象的[Value](../../../ado/reference/ado-api/value-property-ado.md)属性可获取或设置该对象的内容。 内容表示为一种变量，一种可以为其分配值和多个数据类型的对象的类型。  
  
 ADO/WFC 使用 **getValue** 方法实现 **Value** 属性，该方法返回变量对象;和 **setValue** 方法，该方法采用变量作为参数。 变体在某些语言（如 Microsoft Visual Basic）中效率高。  
  
 除了 **Value** 属性，ADO/WFC 还提供使用 Java 数据类型获取和设置 **字段** 对象内容的 *访问器* 方法。 这些方法中的大多数都具有格式为 **获取**_数据_ 类型或 **设置**_数据类型_ 的名称。  
  
 有两个值得注意的异常：其中一个 **getObject** 方法返回强制转换为指定类的对象。 没有 **getNull** 属性;相反，有一个 **isNull** 属性，它返回一个布尔值，指示该字段是否为 null。  
  
```  
public native boolean getBoolean();  
public void setBoolean(boolean v)  
public native byte getByte();  
public void setByte(byte v)  
public native byte[] getBytes();  
public void setBytes(byte[] v)  
public native double getDouble();  
public void setDouble(double v)  
public native float getFloat();  
public void setFloat(float v)  
public native int getInt();  
public void setInt(int v)  
public native long getLong();  
public void setLong(long v)  
public native short getShort();  
public void setShort(short v)  
public native String getString();  
public void setString(String v)  
public native boolean isNull();  
public void setNull()  
public Object getObject()  
public Object getObject(Class c)  
public void setObject(Object value)  
```  
  
## <a name="see-also"></a>另请参阅  
 [字段对象](../../../ado/reference/ado-api/field-object.md)
