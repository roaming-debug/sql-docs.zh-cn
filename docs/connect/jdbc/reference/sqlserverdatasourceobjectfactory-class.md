---
description: SQLServerDataSourceObjectFactory 类
title: SQLServerDataSourceObjectFactory 类 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b616632b-5987-470d-b36c-b22fa9213145
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f4545776ff08e1f2fdd1760e652770a34733f058
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178274"
---
# <a name="sqlserverdatasourceobjectfactory-class"></a>SQLServerDataSourceObjectFactory 类
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  表示将来自 Java 命名和目录接口 (JNDI) 的数据源具体化的对象工厂。  
  
 **包：** com.microsoft.sqlserver.jdbc  
  
 **扩展：** java.lang.Object  
  
 **实现：** javax.naming.spi.ObjectFactory  
  
## <a name="syntax"></a>语法  
  
```  
  
public class SQLServerDataSourceObjectFactory  
```  
  
## <a name="remarks"></a>备注  
 所有的数据源类都继承此方法。 作为对可引用接口的支持的一部分，[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 公开了实现 ObjectFactory 的此类。 Java 应用程序服务器将在数据源类上调用 getReference，这将创建一个在内部将类名用作类工厂的 Reference 对象。  
  
 当 Java 应用程序服务器必须取消对 Reference 对象的引用时，它会创建一个 SQLServerDataSourceObjectFactory 对象的实例，然后调用 [getObjectInstance](../../../connect/jdbc/reference/getobjectinstance-method-sqlserverdatasourceobjectfactory.md) 方法，通过传入 Reference 对象来检索数据源实例。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSourceObjectFactory 成员](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-members.md)   
 [JDBC 驱动程序 API 参考](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
