---
description: relative 方法 (SQLServerResultSet)
title: relative 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.relative
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2bcdbb69-95fd-4ae8-8488-1a75a91fe2e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d61c075e123fc4ae77d852fbe2c3e221bf6ca231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176663"
---
# <a name="relative-method-sqlserverresultset"></a>relative 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将游标沿正向或反向移动相对于当前行的指定行数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean relative(int nRows)  
```  
  
#### <a name="parameters"></a>参数  
 nRows  
  
 指示要移动的行数的 int。  
  
## <a name="return-value"></a>返回值  
 如果游标在行上，则为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 relative 方法是由 java.sql.ResultSet 接口中的 relative 方法指定的。  
  
 如果尝试移到结果集中的第一行之前或最后一行之后，将使游标移到第一行之前或最后一行之后。 调用 `relative(0)` 有效，但是不会更改游标位置。  
  
 调用 `relative(1)` 方法与调用 [next](../../../connect/jdbc/reference/next-method-sqlserverresultset.md) 方法效果相同。 调用 `relative(-1)` 方法与调用 [previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md) 方法效果相同。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
