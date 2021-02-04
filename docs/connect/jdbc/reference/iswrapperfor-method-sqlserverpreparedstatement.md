---
description: isWrapperFor 方法 (SQLServerPreparedStatement)
title: isWrapperFor 方法 (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b0e591b1-73e2-4f90-967f-5555eadfc3f1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 462353b405c2cb1e9d93f0fecbb478c9fc73b555
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177141"
---
# <a name="iswrapperfor-method-sqlserverpreparedstatement"></a>isWrapperFor 方法 (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  指示此语句对象是否为指定接口的包装。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean isWrapperFor(Class iface)  
```  
  
#### <a name="parameters"></a>参数  
 iface  
  
 用于定义接口的 class。  
  
## <a name="return-value"></a>返回值  
 如果此对象实现了接口或包装了实现接口的对象，则为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 [isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md) 方法和 [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md) 方法由在 JDBC 4.0 规范中引入的 java.sql.Wrapper 接口定义。  
  
 如果此方法返回 true，则将使用同一参数成功调用 [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)。  
  
 有关详细信息，请参阅[包装器和接口](../../../connect/jdbc/wrappers-and-interfaces.md)。  
  
## <a name="see-also"></a>另请参阅  
 [unwrap 方法 &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement 类](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
