---
title: 本机错误号 |Microsoft Docs
description: 对于错误，SQL Server Native Client ODBC 驱动程序将从 SQL Server 或（针对驱动程序检测到的错误）返回本机错误号。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, native error numbers
- SQL Server Native Client ODBC driver, errors
- native error numbers [SQL Server Native Client]
- messages [ODBC], native error numbers
- errors [ODBC], native error numbers
ms.assetid: 77cbc826-f47f-4803-8e7a-223d6df069b1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e336c5a85b61d0bff726efc5f3927ae3a1503880
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438470"
---
# <a name="native-error-numbers"></a>本机错误号
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于)  (返回的数据源中发生的错误 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] NATIVE Client ODBC 驱动程序返回由返回的本机错误号 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 对于驱动程序检测到的错误， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序返回本机错误号0。 有关本机错误号列表的详细信息，请参阅中 **master** 数据库中 **sysmessages** 系统表的错误列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 有关状态错误代码的信息，请参阅 [SQLSTATE &#40;ODBC 错误代码&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md)。 对于网络库返回的错误，本机错误号来自于基础网络软件。  
  
## <a name="see-also"></a>另请参阅  
 [处理错误和消息](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
