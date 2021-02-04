---
description: getAsciiStream 方法 (SQLServerNClob)
title: getAsciiStream 方法 (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ff1d47e4-572a-4169-a631-ac261f7642b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 874c8da7b55b3a50bfbc76e9fa2a88b332c9eb6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165778"
---
# <a name="getasciistream-method-sqlservernclob"></a>getAsciiStream 方法 (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索由此 NClob 对象指定的 NCLOB 值作为 ASCII 流。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.InputStream getAsciiStream()  
```  
  
## <a name="return-value"></a>返回值  
 包含 NCLOB 数据的 InputStream 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getAsciiStream 方法是由 java.sql.SQLServerNClob 接口中的 getAsciiStream 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerNClob 方法](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [SQLServerNClob 成员](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob 类](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
