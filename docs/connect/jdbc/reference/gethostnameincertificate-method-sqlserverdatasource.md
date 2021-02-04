---
description: getHostNameInCertificate 方法 (SQLServerDataSource)
title: getHostNameInCertificate 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- getHostNameInCertificate Method (SQLServerDataSource)
apilocation:
- getHostNameInCertificate Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 45ea04e2-9ea5-4171-9136-d09f8a95e128
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2514fae5914ddf409254531961e4e67765438aac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162965"
---
# <a name="gethostnameincertificate-method-sqlserverdatasource"></a>getHostNameInCertificate 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回用于验证 SQL Server 传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）证书的主机名。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.lang.String getHostNameInCertificate()  
```  
  
## <a name="return-value"></a>返回值  
 包含主机名的 String  ，如果未设置值，则为 Null。  
  
## <a name="remarks"></a>备注  
 使用 TLS/SSL 加密通信层时，主机名用于验证 SQL Server TLS/SSL 证书值。  
  
 如果未设置主机名，[getHostNameInCertificate](../../../connect/jdbc/reference/gethostnameincertificate-method-sqlserverdatasource.md) 方法将返回 Null。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
