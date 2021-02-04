---
description: unwrap 方法 (SQLServerStatement)
title: unwrap 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ce680176-ef04-4e44-bb6c-ec50bd06e7e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 633d24d6d90166e361f21f197da27e1d27c0eee1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160895"
---
# <a name="unwrap-method-sqlserverstatement"></a>unwrap 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回一个实现指定接口的对象，从而允许访问特定于 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的方法。  
  
## <a name="syntax"></a>语法  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>参数  
 iface  
  
 定义接口的类型为 T 的类。  
  
## <a name="return-value"></a>返回值  
 实现指定接口的对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md)方法由在 JDBC 4.0 规范中引入的 java.sql.Wrapper 接口定义。  
  
 应用程序可能需要访问特定于 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的 JDBC API 扩展。 如果类公开供应商扩展，则 unwrap 方法支持对此对象扩展的公共类取消包装。  
  
 调用此方法时，对象会取消对 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 类的包装。  
  
 有关代码示例，请参阅[更新大型数据示例](../../../connect/jdbc/updating-large-data-sample.md)或 [unwrap 方法 (SQLServerCallableStatement)](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)。  
  
 有关详细信息，请参阅[包装器和接口](../../../connect/jdbc/wrappers-and-interfaces.md)。  
  
## <a name="see-also"></a>另请参阅  
 [isWrapperFor 方法 &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)   
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
