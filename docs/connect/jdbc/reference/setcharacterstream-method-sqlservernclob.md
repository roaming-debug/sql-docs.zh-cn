---
description: setCharacterStream 方法 (SQLServerNClob)
title: setCharacterStream 方法 (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 09042ee9-dfb1-4d0b-82bd-d1224b0aea80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a21bb749f191e9269c3a1086317d6a9ef1c6236c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173652"
---
# <a name="setcharacterstream-method-sqlservernclob"></a>setCharacterStream 方法 (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索用于在指定起始位置将 Unicode 字符流写入此 java.sql.NClob 对象表示的 NCLOB 值的流。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.io.Writer setCharacterStream(long pos)  
```  
  
#### <a name="parameters"></a>参数  
 pos  
  
 写入 NCLOB 值的起始位置；第一个位置为 1。  
  
## <a name="return-value"></a>返回值  
 表示可向其中写入 Unicode 编码字符的流的 Writer 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setCharacterStream 方法是由 java.sql.NClob 接口中的 setCharacterStream 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerNClob 方法](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [SQLServerNClob 成员](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob 类](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
