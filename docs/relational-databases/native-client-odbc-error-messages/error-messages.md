---
description: 错误消息
title: 错误消息 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], types
- ODBC error handling, message types
- errors [ODBC], types
ms.assetid: 46c0c22e-d105-4d5b-bb9d-5694472e8651
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e86cf87c114bd08e8e1f32189ca9d0755b1764e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745867"
---
# <a name="error-messages"></a>错误消息
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序返回的消息文本将放在 **SQLGetDiagRec** 的 *MessageText* 参数中。 错误源由消息标头指示：  
  
 [Microsoft][ODBC 驱动程序管理器]  
 这些错误由 ODBC 驱动程序管理器引发。  
  
 [Microsoft][ODBC 游标库]  
 这些错误由 ODBC 游标库引发。  
  
 [Microsoft][SQL Server Native Client]  
 这些错误是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序引发的。 如果没有名为 Net-Library 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的其他节点，驱动程序就会发生该错误。  
  
 微软[SQL Server Native Client][*Net-transportname*]  
 这些错误由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 网络库引发，其中， *net net-transportname* 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端网络传输 (的显示名称，例如命名管道、共享内存、TCP/IP 套接字或通过) 。 错误消息的提醒内容含有调用的 Net-Library 函数以及 TDS 函数在基础网络 API 中调用的函数。 返回的 *pfNative* 错误代码是基础网络协议堆栈中的错误代码。  
  
 微软[SQL Server Native Client][ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]]  
 这些错误由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引发。 错误消息中的提醒内容是来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的错误消息文本。 返回的 *pfNative* 代码是来自的错误号 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 若要详细了解 (的错误消息列表及其数字) 可以返回的错误消息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅中 **master** 数据库中 **sysmessages** 系统表的 "描述" 和 "错误" 列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="see-also"></a>另请参阅  
 [处理错误和消息](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
