---
description: getClientInfo 方法 (java.lang.String)
title: getClientInfo 方法 (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e8e632c4-d6cc-4c5e-b6ad-873579343b19
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5a1b16d597533a6c8b2970f9ddd1a66a8b88c225
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168076"
---
# <a name="getclientinfo-method-javalangstring"></a>getClientInfo 方法 (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索指定客户端信息属性的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.lang.String getClientInfo (java.lang.String name)  
```  
  
#### <a name="parameters"></a>参数  
 *name*  
  
 包含要检索的客户端信息属性名称的 String。  
  
## <a name="return-value"></a>返回值  
 包含客户端信息属性值的 String。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getClientInfo 方法是由 java.sql.Connection 接口中的 getClientInfo 方法指定的。  
  
 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 不支持任何客户端信息属性。 因此，此方法将返回 null。  
  
 与此类似，应用程序可以使用 [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) 类中的 [getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md) 方法来检索驱动程序支持的客户端信息属性列表。 [getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md) 方法返回空结果集。  
  
## <a name="see-also"></a>另请参阅  
 [getClientInfo 方法 &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/getclientinfo-method-sqlserverconnection.md)   
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
