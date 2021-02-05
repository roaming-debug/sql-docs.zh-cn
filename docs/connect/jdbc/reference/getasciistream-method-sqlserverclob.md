---
description: getAsciiStream 方法 (SQLServerClob)
title: getAsciiStream 方法 (SQLServerClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.getAsciiStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 134abe5e-5add-4d27-b333-b4b0f4d94c31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c11c82325914bf029a7ac5bf0b5c1b2a5b63058c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168326"
---
# <a name="getasciistream-method-sqlserverclob"></a>getAsciiStream 方法 (SQLServerClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将 CLOB 作为 ASCII 流具体化。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.InputStream getAsciiStream()  
```  
  
## <a name="return-value"></a>返回值  
 包含 CLOB 数据的输入流。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getAsciiStream 方法是由 java.sql.Clob 接口中的 getAsciiStream 方法指定的。  
  
 始终返回字节流并假定 CLOB 中的数据采用 ASCII 格式，因为系统无法知道它是 Unicode 还是任何其他多字节代码页。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerClob 方法](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [SQLServerClob 成员](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [SQLServerClob 类](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
