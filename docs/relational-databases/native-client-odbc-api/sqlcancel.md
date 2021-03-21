---
description: SQLCancel
title: SQLCancel |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLCancel function
ms.assetid: d4c965ae-c1ac-4e9d-b4b9-32b561401106
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 64a3a1849524aca952c0710500907f209a42e3d5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755127"
---
# <a name="sqlcancel"></a>SQLCancel
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md)主题指出，在 ODBC 2.x 中，如果应用程序在对语句进行处理时调用 **SQLCancel** ，则 **SQLCancel** 与带有 **SQL_CLOSE** 选项的 **SQLFreeStmt** 具有相同的效果;此行为仅用于完整性定义，应用程序应调用 **SQLFreeStmt** 或 **SQLCloseCursor** 以关闭游标。 但即使您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 应用程序将 ODBC API 版本设置为 3.5. x 或更高版本， **SQLCancel** 函数也将使用 odbc 2.x 的行为。  
  
## <a name="see-also"></a>另请参阅  
 [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
