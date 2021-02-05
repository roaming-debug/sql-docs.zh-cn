---
description: setUser 方法 (SQLServerDataSource)
title: setUser 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setUser
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d2ea7906-2d10-438d-aa51-f576eea923c7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 952d6fb05ffdf9b07c1811712f50a04fa0d4e3dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178461"
---
# <a name="setuser-method-sqlserverdatasource"></a>setUser 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置用于连接数据源的用户名。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setUser(java.lang.String user)  
```  
  
#### <a name="parameters"></a>参数  
 *user*  
  
 一个包含用户名的字符串。  
  
## <a name="remarks"></a>注解  
 setUser 方法设置将用于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的用户名。 如果未设置用户名值，[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md) 方法则返回默认值 Null。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
