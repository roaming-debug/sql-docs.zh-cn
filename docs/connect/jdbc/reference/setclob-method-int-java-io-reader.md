---
description: setClob 方法 (int, java.io.Reader)
title: setClob 方法 (int, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2b3727da-0480-4cea-b8b1-abda90699b84
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7cedf6f11a2ae415705b744e1d7d534b2223033
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173607"
---
# <a name="setclob-method-int-javaioreader"></a>setClob 方法 (int, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为指定的 Reader 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setClob(int parameterIndex,  
                          java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>参数  
 parameterIndex  
  
 指示参数索引的 int。  
  
 reader  
  
 Reader 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setClob 方法是由 java.sql.PreparedStatement 接口中的 setClob 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [setClob 方法 &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setclob-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
