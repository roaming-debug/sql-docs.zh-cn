---
description: getClientInfoProperties 方法 (SQLServerDatabaseMetaData)
title: getClientInfoProperties 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 1568aef4-f4c4-40a0-a1ab-9c106905bd92
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fe2bb68b8567393ccaa8481e0903bd8b97c5664
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163333"
---
# <a name="getclientinfoproperties-method-sqlserverdatabasemetadata"></a>getClientInfoProperties 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索驱动程序支持的客户端信息属性的列表。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.ResultSet getClientInfoProperties()  
```  
  
## <a name="return-value"></a>返回值  
 ResultSet 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getClientInfoProperties 方法是由 java.sql.DatabaseMetaData 接口中的 getClientInfoProperties 方法指定的。  
  
> [!NOTE]  
>  此方法返回空结果集。 驱动程序支持只设置 applicationName，只在连接时设置 applicationName。 SQL Server 不支持在建立连接后更新客户端应用程序信息。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
