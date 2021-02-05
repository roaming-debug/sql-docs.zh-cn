---
description: setResponseBuffering 方法 (SQLServerStatement)
title: setResponseBuffering 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.setResponseBuffering(String responseBufferingValue)
apilocation:
- SQLServerStatement.setResponseBuffering(String responseBufferingValue)
apitype: Assembly
ms.assetid: 9f489835-6cda-4c8c-b139-079639a169cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fecbee60be6ffe48ffd72d66bf12e78de40a93c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178667"
---
# <a name="setresponsebuffering-method-sqlserverstatement"></a>setResponseBuffering 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的响应缓冲模式设置为不区分大小写的“String full”或“adaptive”。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setResponseBuffering(java.lang.String value)  
```  
  
#### <a name="parameters"></a>参数  
 *value*  
  
 包含响应缓冲模式的 String。 有效模式可以为下列不区分大小写的字符串之一：“full”或“adaptive”。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 “adaptive”指定在必要时缓冲尽可能少的数据。  
  
 “full”指定在运行时从服务器读取全部结果。  
  
 adaptive 是 JDBC 驱动程序版本 2.0 和 3.0 中的默认值。 full 是版本低于 2.0 的 JDBC 驱动程序中的默认值。  
  
 [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) 方法允许覆盖当前 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的 responseBuffering 连接 String 属性。 若要详细了解如何使用响应缓冲模式，请参阅[使用自适应缓冲](../../../connect/jdbc/using-adaptive-buffering.md)。  
  
 如果应用程序为 [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) 方法指定无效的参数值，则会引发 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)   
 [使用自适应缓冲](../../../connect/jdbc/using-adaptive-buffering.md)  
  
  
