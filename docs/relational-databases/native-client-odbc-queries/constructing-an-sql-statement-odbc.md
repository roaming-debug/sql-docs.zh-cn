---
title: 构造 SQL 语句 (ODBC) |Microsoft Docs
description: 了解 SQL Server Client ODBC 驱动程序如何处理 SQL 语句，如何将某些语句分析到 Transact-sql 语句中，以及如何将其他语句传递到数据库。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, statements
- statements [ODBC], constructing
- ODBC applications, statements
ms.assetid: 0acc71e2-8004-4dd8-8592-05c022bdd692
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5963474c575dc33d7c33da14ae13dc43608a81b8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438420"
---
# <a name="constructing-an-sql-statement-odbc"></a>构造 SQL 语句 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 应用程序基本上通过执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句来执行所有的数据库访问。 这些语句的形式取决于应用程序的要求。 SQL 语句可以采用下列方式构造：  
  
-   硬编码  
  
     应用程序作为固定任务执行的静态语句。  
  
-   运行时构造  
  
     运行时构造的 SQL 语句使用户可以通过使用常用子句（如 SELECT、WHERE 和 ORDER BY）来调整语句。 这包括用户输入的即席查询。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]客户端 ODBC 驱动程序仅对不受支持的 ODBC 和 ISO 语法 [!INCLUDE[ssDE](../../includes/ssde-md.md)] （驱动程序转换为）分析 SQL 语句 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 其他所有 SQL 语法将按原样传递给[!INCLUDE[ssDE](../../includes/ssde-md.md)]，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在此确定该语法是否为有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这种方法具有两个好处：  
  
-   降低开销  
  
     由于驱动程序只需扫描较少的 ODBC 和 ISO 子句，因而最大程度地减少了其处理开销。  
  
-   灵活性  
  
     程序员可以调整其应用程序的可移植性。 若要增强针对多个数据库的可移植性，可优先使用 ODBC 和 ISO 语法。 若要使用特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的增强功能，可使用相应的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序支持完整的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法，因此基于 ODBC 的应用程序可以利用中的所有功能 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 SELECT 语句中的列列表应当只包含执行当前任务所需的列。 这样做不仅可减少通过网络发送的数据量，还可降低数据库更改对应用程序的影响。 如果应用程序未引用表中的列，则应用程序不受对该列所做任何更改的影响。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行查询 ](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
