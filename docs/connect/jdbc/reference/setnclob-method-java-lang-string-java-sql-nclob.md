---
description: setNClob 方法 (java.lang.String, java.sql.NClob)
title: setNClob 方法 (java.lang.String, java.sql.NClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4e30d242-0c1b-45db-b75f-41b041692f31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 65b425d8c194d2cdb00ed7319e1b5531e871bdc5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173301"
---
# <a name="setnclob-method-javalangstring-javasqlnclob"></a>setNClob 方法 (java.lang.String, java.sql.NClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为指定 NClob 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setNClob(java.lang.String parameterName,  
              java.sql.NClob value)  
```  
  
#### <a name="parameters"></a>参数  
 parameterName  
  
 包含参数名称的字符串。  
  
 *value*  
  
 NClob 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此方法应该用于 NCHAR、NVARCHAR、NTEXT 和 XML 参数数据类型。  
  
 此 setNClob 方法是由 java.sql.CallableStatement 接口中的 setNClob 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [setNClob 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
