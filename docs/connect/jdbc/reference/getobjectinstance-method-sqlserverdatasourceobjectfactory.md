---
description: getObjectInstance 方法 (SQLServerDataSourceObjectFactory)
title: getObjectInstance 方法 (SQLServerDataSourceObjectFactory) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSourceObjectFactory.getObjectInstance
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0a1503e2-e991-4d70-a223-087fc63baf73
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 015c197f80839a175697c5db8fbaf5b6eb42edf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175301"
---
# <a name="getobjectinstance-method-sqlserverdatasourceobjectfactory"></a>getObjectInstance 方法 (SQLServerDataSourceObjectFactory)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索指定数据源对象的实例。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.lang.Object getObjectInstance(java.lang.Object ref,  
                                          javax.naming.Name name,  
                                          javax.naming.Context c,  
                                          java.util.Hashtable h)  
```  
  
#### <a name="parameters"></a>参数  
 *ref*  
  
 Object 值  。  
  
 *name*  
  
 对象的名称。  
  
 *c*  
  
 相对指定名称的上下文。  
  
 *h*  
  
 创建对象时使用的环境。  
  
## <a name="return-value"></a>返回值  
 Object 值  。  
  
## <a name="exceptions"></a>例外  
 java.sql.SQLException  
  
## <a name="remarks"></a>注解  
 此 getObjectInstance 方法是由 javax.naming.spi.ObjectFactory 接口中的 getObjectInstance 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSourceObjectFactory 方法](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-methods.md)   
 [SQLServerDataSourceObjectFactory 成员](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-members.md)   
 [SQLServerDataSourceObjectFactory 类](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-class.md)  
  
  
