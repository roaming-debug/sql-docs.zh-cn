---
description: getXopenStates 方法 (SQLServerDataSource)
title: getXopenStates 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getXopenStates
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: de6fdf6b-8345-4490-b35e-7115b61e782e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 115ceb9c00c9d5b9e8871724786b4adcec1b89dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177561"
---
# <a name="getxopenstates-method-sqlserverdatasource"></a>getXopenStates 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回指示是否启用了将 SQL 状态转换为 XOPEN 兼容状态的 boolean 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean getXopenStates()  
```  
  
## <a name="return-value"></a>返回值  
 如果启用了将 SQL 状态转换为 XOPEN 兼容状态，则为 true。 否则为 **false**。  
  
## <a name="remarks"></a>备注  
 如果将 xopenStates 属性设置为 true，则 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 会将 SQL 状态转换为 XOPEN 兼容状态。 默认为 false，这将导致 JDBC 驱动程序生成 SQL 99 状态代码。 如果未设置 xopenStates，则 getXopenStates 方法会返回默认值 false。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
