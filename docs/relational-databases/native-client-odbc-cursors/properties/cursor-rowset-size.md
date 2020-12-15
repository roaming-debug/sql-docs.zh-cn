---
description: 游标行集大小
title: 游标行集大小 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], rowset size
- ODBC cursors, rowset size
- rowsets [ODBC]
ms.assetid: 2febe2ae-fdc1-490e-a79f-c516bc8e7c3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66d8314123ffd37bb8163dc18900e62b252a6ce0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473748"
---
# <a name="cursor-rowset-size"></a>游标行集大小
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 游标并不仅限于一次提取一行。 它们可以在对 **SQLFetch** 或 [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)的每次调用中检索多个行。 当与客户端/服务器数据库（例如 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]）一起使用时，可以更有效地一次提取多行。 提取时返回的行数称为行集大小，并使用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)的 SQL_ATTR_ROW_ARRAY_SIZE 来指定。  
  
```  
SQLUINTEGER uwRowsize;  
SQLSetStmtAttr(m_hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)uwRowsetSize, SQL_IS_UINTEGER);  
```  
  
 行集大小大于 1 的游标称为块状游标。  
  
 有两种方法可用于绑定块状游标的结果集列：  
  
-   按列绑定  
  
     每个列绑定到一个变量数组。 每个数组所具有的元素数目等于行集大小。  
  
-   按行绑定  
  
     使用将所有列的数据和指示符保存在一个行中的结构来构建数组。 数组所具有的结构数目等于行集大小。  
  
 使用按列绑定或按行绑定时，对 **SQLFetch** 或 **SQLFetchScroll** 的每个调用都将使用检索到的行集中的数据填充绑定数组。  
  
 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) 也可用于从块游标中检索列数据。 由于 **SQLGetData** 一次只处理一行，因此在调用 **SQLGetData** 之前，必须调用 **SQLSetPos** 将行集中的特定行设置为当前行。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序通过使用行集快速检索整个结果集提供优化。 若要使用此优化，请在调用 **SQLExecDirect** 或 **SQLExecute** 时，将游标属性设置为默认 (只进、只读、行集大小 = 1) 。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序设置默认的结果集。 在不需要滚动的情况下将结果传输到客户端时，该优化功能比服务器游标更有效。 执行语句后，请增加行集大小并使用按列绑定或按行绑定。 这允许 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用默认结果集将结果行高效地发送到客户端，而 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] NATIVE client ODBC 驱动程序会持续从客户端上的网络缓冲区中提取行。  
  
## <a name="see-also"></a>另请参阅  
 [游标属性](../../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
  
