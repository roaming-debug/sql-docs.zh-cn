---
description: getCharacterStream 方法 (long, long) (SQLServerNClob)
title: getCharacterStream 方法 (long, long) (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5a8028bc-c877-4668-b662-0746d462040e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dea79fc0a80221462e3c01a9c3b9ca625b77dd86
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168090"
---
# <a name="getcharacterstream-method-long-long-sqlservernclob"></a>getCharacterStream 方法 (long, long) (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索具有指定位置和长度且作为 Reader 对象或字符流的 NCLOB 数据。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.Reader getCharacterStream(long pos,  
                                  long length)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 指示与要检索的部分值的第一个字符之间偏移量的 long 值。  
  
 *length*  
  
 指示要检索的部分值的字符长度的 long。  
  
## <a name="return-value"></a>返回值  
 包含 NCLOB 数据的 Reader 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getCharacterStream 方法是由 java.sql.NClob 接口中的 getCharacterStream 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [getCharacterStream 方法 &#40;SQLServerNClob&#41;](../../../connect/jdbc/reference/getcharacterstream-method-sqlservernclob.md)   
 [SQLServerNClob 方法](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [SQLServerNClob 成员](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob 类](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
