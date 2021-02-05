---
description: getNCharacterStream 方法 (int)
title: getNCharacterStream 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 6ae704f5-823c-4dfe-8c08-07b547c61a3c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d32325349d63d1dbc4633e97ea7c4aebfd3dc56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175435"
---
# <a name="getncharacterstream-method-int"></a>getNCharacterStream 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根据给定的参数索引，检索指定参数作为 Reader 对象的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final java.io.Reader getNCharacterStream(int parameterIndex)  
```  
  
#### <a name="parameters"></a>参数  
 parameterIndex  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 Reader 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 访问 NCHAR、NVARCHAR 和 LONGNVARCHAR 参数时，应使用此方法。  
  
 此 getNCharacterStream 方法是由 java.sql.CallableStatement 接口中的 getNCharacterStream 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getNCharacterStream 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
