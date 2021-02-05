---
description: getSelectMethod 方法 (SQLServerDataSource)
title: getSelectMethod 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getSelectMethod
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6255d2e-0028-474a-afa8-553ef092243e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b7f60926fdf69a27c5e78d2817084cfe1500f91b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175046"
---
# <a name="getselectmethod-method-sqlserverdatasource"></a>getSelectMethod 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回用于通过使用此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象创建的所有结果集的默认游标类型。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.lang.String getSelectMethod()  
```  
  
## <a name="return-value"></a>返回值  
 一个包含默认游标类型的 String 值。  
  
## <a name="remarks"></a>注解  
 selectMethod 属性指定用于结果集的默认游标类型。 当处理大型结果集但不想在客户端的内存中存储整个结果集时，此属性很有用。 通过将属性设置为“cursor”，可以创建可一次提取更小数据块区的服务器端游标。 如果未设置 selectMethod 属性，getSelectMethod 将返回默认值“direct”。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
