---
description: SQLRowCount
title: SQLRowCount |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLRowCount function
ms.assetid: 967ed3d4-3d31-4485-ac92-027076ebc829
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16990f0abf057c7df6aedb8f3c15b7b489bf94e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483419"
---
# <a name="sqlrowcount"></a>SQLRowCount
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当参数值的数组绑定到语句执行时，如果参数值的任意行在语句执行中生成错误条件，则 **SQLRowCount** 将返回 SQL_ERROR。 不通过函数的 *RowCountPtr* 参数返回值。  
  
 应用程序可以利用 SQL_ATTR_PARAMS_PROCESSED_PTR 语句属性捕获在错误发生之前已处理的参数个数。  
  
 此外，应用程序还可以使用由状态值构成的数组（通过使用 SQL_ATTR_PARAM_STATUS_PTR 语句属性进行绑定），来捕获生成错误的参数行的数组偏移量。 应用程序可以遍历状态数组以确定已处理的实际行数。  
  
 当 [!INCLUDE[tsql](../../includes/tsql-md.md)] 执行包含 output 子句的 INSERT、UPDATE、DELETE 或 MERGE 语句时，SQLRowCount 将不会返回受影响的行数，直到输出子句生成的结果集中的所有行都已被使用。 若要要使用这些行，请调用 SQLFetch 或 SQLFetchScroll。 SQLResultCols 将返回-1，直到使用完所有结果行为止。 在 SQLFetch 或 SQLFetchScroll 返回 SQL_NO_DATA 之后，应用程序必须调用 SQLRowCount 来确定受影响的行数，然后再调用 SQLMoreResults 以移到下一个结果。  
  
## <a name="see-also"></a>另请参阅  
 [SQLRowCount 函数](../../odbc/reference/syntax/sqlrowcount-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
