---
description: setResponseBuffering 方法 (SQLServerDataSource)
title: setResponseBuffering 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setResponseBuffering(String responseBufferingValue)
apilocation:
- SQLServerDataSource.setResponseBuffering(String responseBufferingValue)
apitype: Assembly
ms.assetid: c9e43ff2-8117-4dca-982d-83c863d0c8e1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a62919c7efbefd41505f0ac3e6c021229f3a74d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173163"
---
# <a name="setresponsebuffering-method-sqlserverdatasource"></a>setResponseBuffering 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置通过使用此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象创建的连接的响应缓冲模式。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setResponseBuffering(java.lang.String value)  
```  
  
#### <a name="parameters"></a>参数  
 *value*  
  
 包含缓冲和流模式的 String。 有效模式可以为下列不区分大小写的字符串之一：“full”或“adaptive”。  
  
## <a name="remarks"></a>注解  
 指定在运行时从服务器读取全部结果的 full 值。  
  
 adaptive 值指定在必要时缓冲尽可能少的数据。 adaptive 值为默认缓冲模式。  
  
 若要详细了解如何使用响应缓冲模式，请参阅[使用自适应缓冲](../../../connect/jdbc/using-adaptive-buffering.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
