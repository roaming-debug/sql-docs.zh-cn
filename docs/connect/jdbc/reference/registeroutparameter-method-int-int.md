---
description: registerOutParameter 方法 (int, int)
title: registerOutParameter 方法 (int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.registerOutParameter (int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 169229c7-b75d-498b-a5ac-df300424c909
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d02384d5df0bf1abd17f05b826c762f93cd819ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174226"
---
# <a name="registeroutparameter-method-int-int"></a>registerOutParameter 方法 (int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定序号位置的 OUT 参数注册为给定的 JDBC 类型。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void registerOutParameter(int index,  
                                 int sqlType)  
```  
  
#### <a name="parameters"></a>参数  
 *index*  
  
 一个 int 值，此值指示参数的序号位置。  
  
 *sqlType*  
  
 在 java.sql.Types 中定义的 JDBC 类型代码。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 registerOutParameter 方法是由 java.sql.CallableStatement 接口中的 registerOutParameter 方法指定的。  
  
 自 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC 驱动程序 3.0 起，如果 sqlType 为 java.sql.Types.TIME 类型，此方法的行为由 sendTimeAsDatetime 连接属性（[设置连接属性](../../../connect/jdbc/setting-the-connection-properties.md)）和 [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md) 修改。  
  
 有关详细信息，请参阅[配置将 java.sql.Time 值发送到服务器的方式](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [registerOutParameter 方法 &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
