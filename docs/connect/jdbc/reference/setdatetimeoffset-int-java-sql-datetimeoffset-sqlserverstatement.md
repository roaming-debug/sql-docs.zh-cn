---
description: setDateTimeOffset(int, java.sql.DateTimeOffset) (SQLServerStatement)
title: setDateTimeOffset(int, java.sql.DateTimeOffset) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e8b6e380-6b53-489b-be73-73fcb5258269
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58ee3ec9304023f459a0164ecf15316ff629d308
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179024"
---
# <a name="setdatetimeoffsetint-javasqldatetimeoffset-sqlserverstatement"></a>setDateTimeOffset(int, java.sql.DateTimeOffset) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为给定的 DateTimeOffset 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setDateTimeOffset(int parameterIndex, DateTimeOffset dateTime)  
```  
  
#### <a name="parameters"></a>参数  
 parameterIndex  
  
 要设置的列的索引。  
  
 *dateTimeOffset*  
  
 一个 DateTimeOffset 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 DateTimeOffset 格式为“YYYY-MM-DD HH-MM-SS[.nnnnnnn] [+][-] HH:MM”。 请以下表作为参考。  
  
|SQL 类型|插入|  
|--------------|------------|  
|datetime|只能插入：“YYYY-MM-DD hh:mm:ss[.nnn]”|  
|smalldatetime|只能插入：“YYYY-MM-DD hh:mm:ss”|  
|时间|只能插入：“hh:mm:ss[.nnnnnnn]”|  
|Date|只能插入：“YYYY-MM-DD”|  
|DateTime2|只能插入：“YYYY-MM-DD hh:mm:ss[.nnnnnnn]”|  
  
## <a name="see-also"></a>另请参阅  
 [getDateTimeOffset &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getdatetimeoffset-sqlserverresultset.md)   
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
