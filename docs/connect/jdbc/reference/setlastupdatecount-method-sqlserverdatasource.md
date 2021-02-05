---
description: setLastUpdateCount 方法 (SQLServerDataSource)
title: setLastUpdateCount 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLastUpdateCount
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5487631a-1107-4169-84ca-b77fd09bea66
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dc4cc7f66e75ce6c3f69a65ab9ab71e3f4769212
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173403"
---
# <a name="setlastupdatecount-method-sqlserverdatasource"></a>setLastUpdateCount 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置一个指示是否启用 lastUpdateCount 属性的 Boolean 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setLastUpdateCount(boolean lastUpdateCount)  
```  
  
#### <a name="parameters"></a>参数  
 *lastUpdateCount*  
  
 如果启用了 lastUpdateCount，则为 true。 否则为 **false**。  
  
## <a name="remarks"></a>备注  
 如果将 lastUpdateCount 属性设置为 true，则 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 将只返回由 SQL 语句传递给服务器的最后更新计数。 如果将 lastUpdateCount 属性设置为 false，则驱动程序将返回所有更新计数，包括任何可能已不再使用的触发器所返回的更新计数。 如果未设置 lastUpdateCount 属性，则 [getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md) 方法将返回默认值 true。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
