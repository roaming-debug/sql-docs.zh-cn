---
description: setObject 方法 (int, java.lang.Object)
title: setObject 方法 (int, java.lang.Object) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setObject (int, java.lang.Object)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 61f19faa-3006-4a1c-974c-55951e3b3000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d21949a1d33107cd5f10a9352aa6282a8ad2da79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178770"
---
# <a name="setobject-method-int-javalangobject"></a>setObject 方法 (int, java.lang.Object)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  使用给定对象设置指定参数的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setObject(int index,  
                            java.lang.Object obj)  
```  
  
#### <a name="parameters"></a>参数  
 *index*  
  
 指示参数编号的 int。  
  
 *obj*  
  
 一个对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setObject 方法是由 java.sql.PreparedStatement 接口中的 setObject 方法指定的。  
  
 在调用此 setObject 方法前，应用程序可能会使用以下方法之一设置指定的参数：  
  
-   SQLServerPreparedStatement 类或 SQLServerCallableStatement 类的 set\<Type> 方法  
  
-   SQLServerPreparedStatement 类或 SQLServerCallableStatement 类的 setNull 方法。  
  
-   SQLServerCallableStatement 类的 [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) 方法  
  
 在这种情况下，将自动设置参数的类型。 如果应用程序使用 obj 值 NULL 调用此 setObject 方法，驱动程序则会假定参数的类型为以前调用的方法所设置的一个参数类型。  
  
 如果 obj 值为 NULL 且无法确定该参数的类型信息，此 setObject 方法则会将指定的参数转换为 CHAR 后再将其发送到数据库。  
  
 从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 开始，此方法的行为由 sendTimeAsDatetime 连接属性（[设置连接属性](../../../connect/jdbc/setting-the-connection-properties.md)）和 [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md) 修改。  
  
 有关详细信息，请参阅[配置将 java.sql.Time 值发送到服务器的方式](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md)。  
  
## <a name="see-also"></a>另请参阅  
 [setObject 方法 &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement 类](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
