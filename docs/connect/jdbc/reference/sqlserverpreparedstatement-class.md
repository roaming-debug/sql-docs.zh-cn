---
description: SQLServerPreparedStatement 类
title: SQLServerPreparedStatement 类 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a8481c06-fbba-432b-8c69-4f4619c20ad4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98d30a2e19ecdc6d5b65a77eb2edc83993c18683
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172667"
---
# <a name="sqlserverpreparedstatement-class"></a>SQLServerPreparedStatement 类
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  表示 JDBC 预定义语句功能的基本实现。  
  
 **包：** com.microsoft.sqlserver.jdbc  
  
 **扩展：** SQLServerStatement  
  
 **实现：** [ISQLServerPreparedStatement](../../../connect/jdbc/reference/isqlserverpreparedstatement-interface.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
public class SQLServerPreparedStatement  
```  
  
## <a name="remarks"></a>备注  
 SQLServerPreparedStatement 可提供使你能够提供作为任意本机 Java 类型和许多 Java 对象类型的参数的方法。 SQLServerPreparedStatement 通过使用  sp_prepare 存储过程来定义某个语句，然后通常使用用户提供的不同参数对每个后续运行的语句重用返回的语句句柄[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
 SQLServerPreparedStatement 支持通过批处理（即在单个数据库往返通信中运行一组预定义语句）来改善运行时性能。  
  
 此类支持解包到 SQLServerPreparedStatement 类、ISQLServerPreparedStatement 接口、java.sql.PreparedStatement 接口，以及用于进行解包的 SQLServerStatement 支持的类和接口。 有关详细信息，请参阅[包装器和接口](../../../connect/jdbc/wrappers-and-interfaces.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [JDBC 驱动程序 API 参考](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
