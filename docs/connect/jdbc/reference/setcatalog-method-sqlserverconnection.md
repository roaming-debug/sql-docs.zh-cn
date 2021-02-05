---
description: setCatalog 方法 (SQLServerConnection)
title: setCatalog 方法 (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setCatalog
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 553c0603-c07d-436a-86eb-3ba6b51bd696
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d7c72a4f3198f1d38906288a5c5cec8f3873cb53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173733"
---
# <a name="setcatalog-method-sqlserverconnection"></a>setCatalog 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将给定目录名称设置为选择在其中使用此 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象的数据库的子空间。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setCatalog(java.lang.String catalog)  
```  
  
#### <a name="parameters"></a>参数  
 *catalog*  
  
 一个包含目录名称的字符串  。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setCatalog 方法是由 java.sql.Connection 接口中的 setCatalog 方法指定的。  
  
 catalog 参数自动由 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 转义。 使用此方法可为 Connection 对象设置目录属性。 无法通过任何其他方法隐式设置该属性。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
