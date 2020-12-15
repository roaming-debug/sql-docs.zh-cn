---
description: SQLNativeSql
title: SQLNativeSql |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLNativeSql function
ms.assetid: 2d999fec-9e22-4514-ad5f-22a64b82f95b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cebaef8091839467fa818f13516efee384b089c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485079"
---
# <a name="sqlnativesql"></a>SQLNativeSql
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序在不访问服务器的情况下满足 **SQLNativeSql** 请求。 该函数有效地测试 SQL 语句的语法。 语法检查无法确定 SQL 语句中表达式的标识符或结果是否有效，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **SQLNativeSql** 返回的本机 SQL 可能无法运行。  
  
## <a name="see-also"></a>另请参阅  
 [SQLNativeSql 函数](../../odbc/reference/syntax/sqlnativesql-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
