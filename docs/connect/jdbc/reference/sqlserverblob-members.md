---
description: SQLServerBlob 成员
title: SQLServerBlob 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 90e48555-ea83-4a90-80a3-51bc685015ec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33ad2623cf011690ca26ffd5acbc61c2c98cb78f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178397"
---
# <a name="sqlserverblob-members"></a>SQLServerBlob 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
  
|名称|说明|  
|----------|-----------------|  
|[SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-constructor-sqlserverconnection-byte.md)|初始化 SQLServerBlob 类的新实例。|  
  
## <a name="fields"></a>字段  
 无。  
  
## <a name="inherited-fields"></a>继承的字段  
 无。  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[free](../../../connect/jdbc/reference/free-method-sqlserverblob.md)|此方法可释放 BLOB 对象以及它所持有的资源。|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlserverblob.md)|返回一个用于从 BLOB 中读取数据的输入流。|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlserverblob.md)|获取 BLOB 数据作为字节数组。|  
|[length](../../../connect/jdbc/reference/length-method-sqlserverblob.md)|返回 BLOB 对象中的字节数。|  
|[position](../../../connect/jdbc/reference/position-method-sqlserverblob.md)|根据给定的模式和起始索引返回 BLOB 中指定模式的位置。|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-method-sqlserverblob.md)|检索可用于写入 BLOB 值的流。|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlserverblob.md)|将给定的字节数组写入 BLOB（从给定的位置开始），然后返回写入的字节数。|  
|[truncate](../../../connect/jdbc/reference/truncate-method-sqlserverblob.md)|将 BLOB 截断给定的长度。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.lang.Object|clone、equals、finalize、getClass、hashCode、notify、notifyAll、toString、wait|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerBlob 类](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
