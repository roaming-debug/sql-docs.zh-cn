---
description: getNString 方法 (int)
title: getNString 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2048bb9f-7d9b-4aaa-b135-c716910cc800
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 95acb14fd2c51884b116e27d9d58a63555c88574
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175346"
---
# <a name="getnstring-method-int"></a>getNString 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定 NCHAR、NVARCHAR 或 LONGNVARCHAR 参数的值作为 Java 编程语言中的 String 进行检索。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final java.lang.String getNString(int parameterIndex)  
```  
  
#### <a name="parameters"></a>参数  
 parameterIndex  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 AStringobject。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getNString 方法是由 java.sql.CallableStatement 接口中的 getNString 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getNString 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getnstring-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
