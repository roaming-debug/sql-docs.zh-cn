---
title: 创建分布式事务 |Microsoft Docs
description: 应用程序可以使用 MSDTC 跨多个 SQL Server 实例扩展或分发事务。 .NET 类还可以分发事务。
ms.custom: ''
ms.date: 05/13/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- MS DTC, performing distributed transactions
- SQL Server Native Client ODBC driver, transactions
- distributed transactions [ODBC]
- transactions [ODBC]
- ODBC, transactions
ms.assetid: 2c17fba0-7a3c-453c-91b7-f801e7b39ccb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e87efff1462cef968a6fdce7038c65325bf7681c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755697"
---
# <a name="create-a-distributed-transaction"></a>创建分布式事务

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

<!--
The following includes .md file is Empty, as of long before 2019/May/13.
/includes/snac-deprecated.md
-->


可以通过不同的方式为不同的 Microsoft SQL 系统创建分布式事务。

## <a name="odbc-driver-calls-the-msdtc-for-sql-server-on-premises"></a>ODBC 驱动程序调用 MSDTC 用于本地 SQL Server

Microsoft 分布式事务处理协调器 (MSDTC) 允许应用程序跨两个或更多实例扩展或 _分发_ 事务 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 即使两个实例托管在不同的计算机上，分布式事务也能正常工作。

为本地 Microsoft SQL Server 安装了 MSDTC，但不适用于 Microsoft 的 Azure SQL 数据库云服务。

当你的 c + + 程序管理分布式事务时，SQL Server Native Client 驱动程序 (ODBC) 的开放式数据库连接调用 MSDTC。 Native Client ODBC 驱动程序具有符合开放式组分布式事务处理 (DTP) XA 标准的事务管理器。 MSDTC 需要此相容性。 通常，所有事务管理命令都是通过此 Native Client ODBC 驱动程序发送的。 顺序如下：

1. C + + Native Client ODBC 应用程序通过调用 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)启动了一个事务，并关闭了自动提交模式。

2. 应用程序在计算机 A 上的 SQL Server X 上更新某些数据。

3. 应用程序会更新计算机 B 上 SQL Server Y 上的一些数据。
    - 如果 SQL Server Y 上的更新失败，则将回滚两个 SQL Server 实例上的所有未提交的更新。

4. 最后，应用程序通过使用 SQL_COMMIT 或 SQL_ROLLBACK 选项调用 [SQLEndTran _(1)_](../../../relational-databases/native-client-odbc-api/sqlendtran.md)来结束事务。

_(1)_ 无需 ODBC 即可调用 MSDTC。 在这种情况下，MSDTC 成为事务管理器，应用程序不再使用 **SQLEndTran**。

### <a name="only-one-distributed-transaction"></a>仅一个分布式事务

假设你的 c + + Native Client ODBC 应用程序已登记到分布式事务中。 接下来，应用程序在另一个分布式事务中登记。 在这种情况下， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序将离开原始分布式事务，并登记到新的分布式事务中。

有关详细信息，请参阅 [DTC 程序员参考](/previous-versions/windows/desktop/ms686108(v=vs.85))。

## <a name="c-alternative-for-sql-database-in-the-cloud"></a>云中 SQL 数据库的 c # 替代项

Azure SQL Database 或 Azure Synapse Analytics 不支持 MSDTC。

但是，可以通过使 c # 程序使用 [.net 类 system.exception](/dotnet/api/system.transactions.transactionscope)，为 SQL 数据库创建分布式事务。

### <a name="other-programming-languages"></a>其他编程语言

以下其他编程语言可能不会为 SQL 数据库服务提供对分布式事务的任何支持：

- 使用 ODBC 驱动程序的本机 c + +
- 使用 Transact-sql 的链接服务器
- JDBC 驱动程序

## <a name="see-also"></a>请参阅

[执行事务 (ODBC)](performing-transactions-in-odbc.md)