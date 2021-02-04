---
description: createBlob 方法 (SQLServerConnection)
title: createBlob 方法 (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 630a93b0-6e3c-4255-a007-1097ce0ee243
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1eef993496280ed3dc8e64d7c19a0605a3e5c552
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163458"
---
# <a name="createblob-method-sqlserverconnection"></a>createBlob 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  创建不含任何数据的 Blob 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.Blob createBlob()  
```  
  
## <a name="return-value"></a>返回值  
 Blob 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 createBlob 方法是由 java.sql.Connection 接口中的 createBlob 方法指定的。  
  
 此方法取代了对 [SQLServerBlob 构造函数（SQLServerConnection、byte）](../../../connect/jdbc/reference/sqlserverblob-constructor-sqlserverconnection-byte.md)的需求。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
