---
description: ODBC 游标库
title: ODBC 游标库 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], library
- SQL_CUR_USE_DRIVER option
- ODBC applications, cursors
- ODBC cursors, library
- SQL_CUR_USE_IF_NEEDED option
- SQLSetConnectAttr function
- SQL_CUR_USE_ODBC option
ms.assetid: 3c610d3d-6e06-49cf-9a40-05b6a1c83a32
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b38644e7d4ef2eb1d0e5b53166366bd116c1639e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756257"
---
# <a name="odbc-cursor-library"></a>ODBC 游标库
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  某些 ODBC 驱动程序仅支持默认游标设置;这些驱动程序也不支持定位游标操作，如 **SQLSetPos**。 ODBC 游标库是用于对通常不支持块状游标或静态游标的驱动程序实现这些游标的 Microsoft 数据访问组件 (MDAC) 的组件。 游标库还实现了定位的 UPDATE 和 DELETE 语句，并为它创建的游标实现了 **SQLSetPos** 。  
  
 ODBC 游标库作为 ODBC 驱动程序管理器和 ODBC 驱动程序之间的层实现。 如果加载 ODBC 游标库，ODBC 驱动程序管理器则将与游标相关的所有命令路由到该游标库，而不是驱动程序。 游标库通过从基础驱动程序提取整个结果集并将此结果集缓存到客户端来实现游标。 当使用 ODBC 游标库时，应用程序仅限于该游标库的游标功能；对基础驱动程序中的其他游标功能的支持不可用于该应用程序。  
  
 通常不需要将 ODBC 游标库与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序一起使用，因为与 ODBC 游标库相比，该驱动程序自身支持更多的游标功能。 与 Native Client ODBC 驱动程序一起使用 ODBC 游标库的唯一原因 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 是，驱动程序通过服务器游标实现其游标支持，而服务器游标不支持所有 SQL 语句。 如果需要配合使用静态游标和存储过程、批处理或包含 COMPUTE、COMPUTE BY、FOR BROWSE 或 INTO 的 SQL 语句，请考虑使用 ODBC 游标库。 但是，使用游标库时应非常小心，因为它在客户端上缓存整个结果集，这可能占用大量内存并降低性能。  
  
 应用程序在连接到数据源之前，使用 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 来设置 SQL_ATTR_ODBC_CURSORS 连接属性，从而按连接方式调用游标库。 SQL_ATTR_ODBC_CURSORS 设置为下列三个值之一：  
  
 SQL_CUR_USE_ODBC  
 当此选项设置为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT odbc 驱动程序时，ODBC 游标库将替代 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] NATIVE client odbc 驱动程序的本机游标支持。 只有该游标库支持的游标类型才能用于连接；不能使用服务器游标。  
  
 SQL_CUR_USE_DRIVER  
 如果设置了此选项，则 native Client ODBC 驱动程序的本机支持的所有游标都 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以用于连接。 不能使用 ODBC 游标库。 所有游标均作为服务器游标实现。  
  
 SQL_CUR_USE_IF_NEEDED  
 如果设置了此选项，则效果与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序一起使用时的效果 SQL_CUR_USE_DRIVER 相同。 在连接时，ODBC 驱动程序管理器会进行测试，以查看连接的 ODBC 驱动程序是否支持 [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)的 SQL_FETCH_PRIOR 选项。 如果驱动程序不支持此选项，ODBC 驱动程序管理器则加载 ODBC 游标库。 如果驱动程序支持此选项，ODBC 驱动程序管理器则不加载 ODBC 游标库，应用程序将使用该驱动程序的本机支持。 由于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序支持 SQL_FETCH_PRIOR，Odbc 驱动程序管理器不会加载 odbc 游标库。  
  
 游标库允许应用程序对一个连接使用多个活动语句，以及可滚动、可更新的游标。 必须加载游标库以支持该功能。 使用 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 指定应如何使用游标库，并使用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 来指定游标类型、并发和行集大小。  
  
## <a name="see-also"></a>另请参阅  
 [如何实现游标](../../../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
  
