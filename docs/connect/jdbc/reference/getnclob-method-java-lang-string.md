---
description: getNClob 方法 (java.lang.String)
title: getNClob 方法 (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: be01ce56-8f13-437b-8de6-246cda5f7830
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41dc7c3a59b7221e62b241d917a6839234122b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175375"
---
# <a name="getnclob-method-javalangstring"></a>getNClob 方法 (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索 JDBC NCLOB 参数作为 Java 编程语言中的 NClob 对象的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.NClob getNClob(java.lang.String parameterName)  
```  
  
#### <a name="parameters"></a>参数  
 parameterName  
  
 包含参数名称的字符串。  
  
## <a name="return-value"></a>返回值  
 NClob 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getNClob 方法是由 java.sql.CallableStatement 接口中的 getNClob 方法指定的。  
  
 此方法仅支持检索 NCHAR、NVARCHAR、NTEXT 和 XML 参数。 在其他数据类型参数上调用这些方法会引发异常。  
  
## <a name="see-also"></a>另请参阅  
 [getNClob 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
