---
title: ODBC 中的事务 |Microsoft Docs
description: ODBC 在连接级别管理事务，在自动提交或手动提交模式下提交或回滚所有已完成的工作。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, transactions
- transactions [ODBC]
- ODBC, transactions
ms.assetid: c5a87fa5-827a-4e6f-a0d9-924bac881eb0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 071146a14be3dbeff057845f0458de422617430f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483108"
---
# <a name="performing-transactions-in-odbc"></a>在 ODBC 中执行事务
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 中的事务按连接级别进行管理。 在应用程序完成某一事务时，它提交或回滚通过该连接上的所有语句句柄完成的所有工作。 若要提交或回滚事务，应用程序应调用 [SQLEndTran](../../../relational-databases/native-client-odbc-api/sqlendtran.md) ，而不是提交 COMMIT 或 ROLLBACK 语句。  
  
 应用程序调用 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) ，以在两种管理事务的 ODBC 模式之间切换：  
  
-   自动提交模式  
  
     每个语句都在成功完成后自动提交。 当您在自动提交模式中运行时，不要求其他事务管理功能。  
  
-   手动提交模式  
  
     所有已执行的语句都包含在同一事务中，直到通过调用 **SQLEndTran** 专门停止它。  
  
 自动提交模式是针对 ODBC 的默认的事务模式。 建立连接后，该连接处于自动提交模式，直到调用 **SQLSetConnectAttr** ，通过设置自动提交模式来切换为手动提交模式。 在应用程序关闭自动提交后，发送到数据库的下一个语句将开始某一事务。 然后，在应用程序调用带有 SQL_COMMIT 或 SQL_ROLLBACK 选项的 **SQLEndTran** 之前，该事务将一直有效。 **SQLEndTran** 在启动下一个事务后发送到数据库的命令。  
  
 如果应用程序从手动提交模式切换到自动提交模式，则驱动程序将提交在该连接上当前打开的所有事务。  
  
 ODBC 应用程序不应使用 Transact-SQL 事务语句（例如 BEGIN TRANSACTION、COMMIT TRANSACTION 或 ROLLBACK TRANSACTION），因为这可能导致驱动程序中的不确定行为。 ODBC 应用程序应在自动提交模式下运行，而不是使用任何事务管理函数或语句，或在手动提交模式下运行，并使用 ODBC **SQLEndTran** 函数提交或回滚事务。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行事务 ]()  
  
