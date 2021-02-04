---
description: getReference 方法 (SQLServerDataSource)
title: getReference 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getReference
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b3fb1a97-86ee-4977-adca-c35ae199dbb3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d44e8f10c1e4160504d20de7d5a9b3e36b31b950
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175204"
---
# <a name="getreference-method-sqlserverdatasource"></a>getReference 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回对此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象的引用。  
  
## <a name="syntax"></a>语法  
  
```  
  
public javax.naming.Reference getReference()  
```  
  
## <a name="return-value"></a>返回值  
 Reference 对象。  
  
## <a name="remarks"></a>注解  
 此 getReference 方法是由 javax.naming.Referenceable 接口中的 getReference 方法指定的。  
  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 之前，如果对 SQLServerDataSource 对象调用 SQLServerDataSource.setTrustStorePassword，密码将会出现在 SQLServerDataSource.getReference 所返回的对象中，从而能够使用此对象建立其他的连接。 在 JDBC Driver 3.0 中，您在使用 SQLServerDataSource.getReference 返回的对象建立连接前，将需要设置该对象的密码。  
  
 而且，如果您在绑定数据源属性前设置 SQLServerDataSource.setTrustStorePassword，在获取连接前必须调用 SQLServerDataSource.setTrustStorePassword。 例如，应用于对象的  
  
```  
ctx = new InitialContext(System.getProperties());  
  
SQLServerDataSource ds1 = (SQLServerDataSource) ctx.lookup(jndiName);  
  
ds1.setTrustStorePassword("XXXXX");  
Connection con = ds1.getConnection("user", "XXXXXX");  
  
ctx.rebind(jndiName, ds1);  
SQLServerDataSource ds2 = (SQLServerDataSource) ctx.lookup(jndiName);  
ds2.setTrustStorePassword("XXXXX");   // reset the truststore password  
con = ds2.getConnection("user", "XXXXXX");   // provide userid and password again  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
