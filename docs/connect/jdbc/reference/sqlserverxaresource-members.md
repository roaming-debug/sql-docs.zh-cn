---
description: SQLServerXAResource 成员
title: SQLServerXAResource 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a069bf2c-1b70-4817-b084-a508445de799
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 81a63c16b08b7c1d418614a1e521cc8bede0b0f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181663"
---
# <a name="sqlserverxaresource-members"></a>SQLServerXAResource 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
 无。  
  
## <a name="fields"></a>字段  
  
|名称|说明|  
|----------|-----------------|  
|[SSTRANSTIGHTLYCPLD](../../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md)|用于允许使用紧密结合的 XA 事务，这些事务具有不同的 XA 分支事务 ID (XID)，但具有相同的全局事务 ID (GTRID)。|  
  
## <a name="inherited-fields"></a>继承的字段  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|javax.transaction.xa.XAResource|TMENDRSCAN、TMFAIL、TMJOIN、TMNOFLAGS、TMONEPHASE、TMRESUME、TMSTARTRSCAN、TMSUCCESS、TMSUSPEND、XA_OK、XA_RDONLY|  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[commit](../../../connect/jdbc/reference/commit-method-sqlserverxaresource.md)|提交由给定的 Xid 对象所指定的全局事务。|  
|[end](../../../connect/jdbc/reference/end-method-sqlserverxaresource.md)|结束代表某个事务分支执行的工作。|  
|[forget](../../../connect/jdbc/reference/forget-method-sqlserverxaresource.md)|通知资源管理器忘记试探完成的事务分支。|  
|[getTransactionTimeout](../../../connect/jdbc/reference/gettransactiontimeout-method-sqlserverxaresource.md)|获取为此 [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) 对象设置的当前事务超时值。|  
|[isSameRM](../../../connect/jdbc/reference/issamerm-method-sqlserverxaresource.md)|确定目标对象所表示的资源管理器实例是否与给定 XAResource 对象所表示的资源管理器实例相同。|  
|[prepare](../../../connect/jdbc/reference/prepare-method-sqlserverxaresource.md)|请求资源管理器为提交给定 Xid 对象所指定的事务做好准备。|  
|[recover](../../../connect/jdbc/reference/recover-method-sqlserverxaresource.md)|从资源管理器获取准备好的事务分支的列表。|  
|[rollback](../../../connect/jdbc/reference/rollback-method-sqlserverxaresource.md)|请求资源管理器回滚代表事务分支执行的工作。|  
|[setTransactionTimeout](../../../connect/jdbc/reference/settransactiontimeout-method-sqlserverxaresource.md)|为此 [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) 对象设置当前事务超时值。|  
|[start](../../../connect/jdbc/reference/start-method-sqlserverxaresource.md)|开始代表在 Xid 对象中指定的事务分支工作。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.lang.Object|clone、equals、finalize、getClass、hashCode、notify、notifyAll、toString、wait|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerXAResource 类](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
