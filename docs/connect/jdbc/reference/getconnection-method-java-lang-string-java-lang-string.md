---
description: getConnection 方法 (java.lang.String, java.lang.String)
title: getConnection 方法 (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getConnection (java.lang.String, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 78db89d6-a8a0-4116-8885-548e627220ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bdb2c566b6cbd89ac01c9dc5b990268055a3fe09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163300"
---
# <a name="getconnection-method-javalangstring-javalangstring"></a>getConnection 方法 (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  使用给定的用户名和密码尝试与此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象表示的数据源建立连接。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.Connection getConnection(java.lang.String username,  
                                         java.lang.String password)  
```  
  
#### <a name="parameters"></a>参数  
 *username*  
  
 一个包含用户名的字符串。  
  
 *password*  
  
 一个包含密码的字符串。  
  
## <a name="return-value"></a>返回值  
 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象。  
  
## <a name="exceptions"></a>例外  
 java.sql.SQLException  
  
## <a name="remarks"></a>注解  
 此 getConnection 方法是由 javax.sql.DataSource 接口中的 getConnection 方法指定的。  
  
 如果使用非 Null 的用户名或密码调用 getConnection 方法，将替换初始化 SQLServerConnection 对象时为 SQLServerDataSource 类设置的用户名和密码属性。 例如，如果调用方对数据源调用了 [setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md) 和 [setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)，然后调用 getConnection 并提供非 Null 的用户名或非 Null 的密码，则由 setUser 和 setPassword 设置的用户名和密码将被替换为传入 getConnection 的用户名和密码。  
  
> [!NOTE]  
>  在这种情况下，通过调用 [setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md) 方法在 URL 中设置的用户名和密码将保持不变。  
  
## <a name="see-also"></a>另请参阅  
 [getConnection 方法 &#40;SQLServerDataSource&#41;](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)   
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
