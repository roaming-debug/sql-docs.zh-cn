---
description: setSQLXML 方法 (SQLServerPreparedStatement)
title: setSQLXML 方法 (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 70bbdde0-75f7-4169-88c5-dbbe2c4bcd03
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8fc6011dc256692694e3a3987c8f2e9eb10c7d5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173075"
---
# <a name="setsqlxml-method-sqlserverpreparedstatement"></a>setSQLXML 方法 (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为指定的 SQLXML 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setSQLXML(int parameterIndex,  
                            java.sql.SQLXML xmlObject)  
```  
  
#### <a name="parameters"></a>参数  
 parameterIndex  
  
 指示参数索引的 int。  
  
 xmlObject  
  
 包含参数值的 SQLXML 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setSQLXML 方法是由 java.sql.PreparedStatement 接口中的 setSQLXML 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
