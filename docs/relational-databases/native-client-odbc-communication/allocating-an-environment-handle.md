---
description: 分配环境句柄
title: 分配环境句柄 |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, environment handles
- ODBC applications, connections
- handles [SQL Server Native Client]
- environment handles [SQLNCLI]
ms.assetid: 15c1b428-ea6d-4672-894c-f0e289e2da3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88193ccd3e9f9393829fb9816b2149dadbeb0484
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756377"
---
# <a name="allocating-an-environment-handle"></a>分配环境句柄
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在应用程序可以调用任何 ODBC 函数之前，它必须初始化 ODBC 环境并分配环境句柄。 这是全局上下文句柄，并且是 ODBC 中其他句柄的占位符。 为此，可调用 **SQLAllocHandle** ，并将 *HandleType* 参数设置为 SQL_HANDLE_ENV 并将 *将 inputhandle* 设置为 SQL_NULL_HANDLE。  
  
 分配环境句柄之后，应用程序必须设置环境属性以指示它将使用哪一个版本的 ODBC 函数调用。 使用 ODBC 3。*x* 函数调用 [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) ，并将 *特性* 参数设置为 SQL_ATTR_ODBC_VERSION，将 *将 valueptr* 设置为 SQL_OV_ODBC3。  
  
## <a name="see-also"></a>另请参阅  
 [与 SQL Server &#40;ODBC&#41;通信 ](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
