---
description: setTrustStore 方法 (SQLServerDataSource)
title: setTrustStore 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setTrustStore Method (SQLServerDataSource)
apilocation:
- setTrustStore Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: bab5485d-4547-426c-adbe-44e2b5702d1d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d537e17309db560b914ea4b9a71c726e9bfe2003
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172951"
---
# <a name="settruststore-method-sqlserverdatasource"></a>setTrustStore 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置指向证书 trustStore 文件的路径（包括文件名）。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setTrustStore(java.lang.String trustStore)  
```  
  
#### <a name="parameters"></a>参数  
  trustStore  
  
 包含证书 trustStore 文件的路径（包括文件名）的字符串  。  
  
## <a name="remarks"></a>备注  
 如果未指定 trustStore 属性或将此属性设置为 null，则 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 将依赖于信任关系管理器工厂的查找规则来确定要使用哪一个证书存储区。 默认的 SunX509 TrustManagerFactory 尝试按此顺序在下列位置查找信任的资料：  
  
-   1. 由“javax.net.ssl.trustStore”Java 虚拟机 (JVM) 系统属性指定的文件。  
  
-   2. “\<java-home>/lib/security/jssecacerts”文件。  
  
-   3. “\<java-home>/lib/security/cacerts”文件。  
  
 有关详细信息，请参阅 Sun Microsystems 网站上的 SunX509 TrustManager 接口文档。  
  
 如果 trustStore 属性设置为字符串或空字符串 ""，则驱动程序将使用该值来查找 trustStore 文件以验证服务器 TLS/SSL 证书。  
  
 可随 trustStore 属性同时指定 trustStorePassword 属性，并使用其值来打开 trustStore 文件。 有关详细信息，请参阅 [setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
