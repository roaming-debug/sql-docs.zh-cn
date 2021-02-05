---
description: SQLServerCallableStatement 类
title: SQLServerCallableStatement 类 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30710a63-c05d-47d9-9cf9-c087a1c76373
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60faed31bc161bda250a4981d98406f81815a1ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178415"
---
# <a name="sqlservercallablestatement-class"></a>SQLServerCallableStatement 类
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  允许你指定要与输入和输出参数一起使用的要调用的存储过程名称。 此类还可以使用 `? = call( ?, ..)` 语义检索返回状态值。  
  
 **包：** com.microsoft.sqlserver.jdbc  
  
 **实现：** [ISQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
 **扩展：** [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
public final class SQLServerCallableStatement  
```  
  
## <a name="remarks"></a>备注  
 SQLServerCallableStatement 使你能够指定要与输入和输出参数一起调用的存储过程名称。 SQLServerCallableStatement 还可以使用 `? = call( ?, ..)` 语法检索返回状态值。  
  
 此类支持解包到 SQLServerCallableStatement 类、ISQLServerCallableStatement 接口、java.sql.CallableStatement 接口，以及用于进行解包的 SQLServerPreparedStatement 支持的类和接口。 有关详细信息，请参阅[包装器和接口](../../../connect/jdbc/wrappers-and-interfaces.md)。  
  
 针对某一类型调用 SQLServerCallableStatement set 方法之一时，如果该类型与 [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) 中指定的类型冲突，则将使用最后一个 SQLServerCallableStatement set 方法指定的类型。 但是这可能导致出现不兼容的数据类型转换错误。 如果未调用 SQLServerCallableStatement set 方法，则将使用第一个 [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) 调用所指定的类型。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 符合 JDBC 4.0 建议，即在检索 OUT 参数之前必须检索一个结果集和更新计数。 如果在完全处理该结果集和更新计数前检索 OUT 参数，则将丢失尚未处理的结果集和更新计数。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerCallableStatement 成员](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [JDBC 驱动程序 API 参考](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
