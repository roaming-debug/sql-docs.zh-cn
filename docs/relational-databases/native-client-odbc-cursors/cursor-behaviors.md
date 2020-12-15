---
description: 游标行为
title: 游标行为 |Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [SQL Server]
- SQL Server Native Client ODBC driver, cursors
- version-based optimistic concurrency
- cursors [ODBC], cursor behaviors
- ODBC applications, cursors
- SQL_ATTR_CURSOR_SENSITIVITY option
- SQL_ATTR_CURSOR_SCROLLABLE option
- sensitivity behavior of cursor
- ODBC cursors, cursor behaviors
ms.assetid: 742ddcd2-232b-4aa1-9212-027df120ad35
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 91bc7286beb8b03f6e76698e84a8fcf3d8d3f11c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438658"
---
# <a name="cursor-behaviors"></a>游标行为
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 支持通过指定游标的可滚动性和敏感性来指定其行为的 ISO 选项。 这些行为是通过在对 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)的调用上设置 SQL_ATTR_CURSOR_SCROLLABLE 和 SQL_ATTR_CURSOR_SENSITIVITY 选项来指定的。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序通过请求具有以下特征的服务器游标来实现这些选项。  
  
|游标行为设置|请求的服务器游标的特征|  
|------------------------------|---------------------------------------------|  
|SQL_SCROLLABLE 和 SQL_SENSITIVE|由键集驱动的游标和基于版本的乐观并发|  
|SQL_SCROLLABLE 和 SQL_INSENSITIVE|静态游标和只读并发|  
|SQL_SCROLLABLE 和 SQL_UNSPECIFIED|静态游标和只读并发|  
|SQL_NONSCROLLABLE 和 SQL_SENSITIVE|只进游标和基于版本的乐观并发|  
|SQL_NONSCROLLABLE 和 SQL_INSENSITIVE|默认结果集（只进，只读）|  
|SQL_NONSCROLLABLE 和 SQL_UNSPECIFIED|默认结果集（只进，只读）|  
  
 基于版本的乐观并发要求基础表中有一个 **时间戳** 列。 如果对没有 **时间戳** 列的表请求基于版本的乐观并发控制，则服务器将使用基于值的乐观并发。  
  
## <a name="scrollability"></a>可滚动性  
 当 SQL_ATTR_CURSOR_SCROLLABLE 设置为 SQL_SCROLLABLE 时，游标支持 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)的 *FetchOrientation* 参数的所有不同值。 当 SQL_ATTR_CURSOR_SCROLLABLE 设置为 SQL_NONSCROLLABLE 时，游标仅支持 SQL_FETCH_NEXT 的 *FetchOrientation* 值。  
  
## <a name="sensitivity"></a>敏感性  
 将 SQL_ATTR_CURSOR_SENSITIVITY 设置为 SQL_SENSITIVE 时，游标可以反映由当前用户所做的或由其他用户提交的数据修改。 将 SQL_ATTR_CURSOR_SENSITIVITY 设置为 SQL_INSENSITIVE 时，游标不能反映数据修改。  
  
## <a name="see-also"></a>另请参阅  
 [使用游标 (ODBC) ](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md) [Cursor 属性](properties/cursor-properties.md) 
  
  
