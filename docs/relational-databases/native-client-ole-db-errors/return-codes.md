---
title: '返回代码 (Native Client OLE DB 提供程序) '
description: 了解 SQL Server Native Client OLE DB 支持的返回代码，包括经常遇到的 DB_S_ERRORSOCCURRED HRESULT 值。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB error handling, return codes
- SQL Server Native Client OLE DB provider, errors
- failed function [OLE DB]
- successful function [OLE DB]
- S_FALSE
- IS_ERROR macro
- DB_S_ERRORSOCCURRED return
- return codes [OLE DB]
- S_OK
- FAILED macro
- errors [OLE DB], return codes
ms.assetid: 7f7457e9-fce4-400c-82e5-ee02e9e811c6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d665e65d8dae3a6b114486e77161077ad8c74e6b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738617"
---
# <a name="return-codes-native-client-ole-db-provider"></a>返回代码 (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在最基本的级别上，成员函数要么成功，要么失败。 在稍微精确一些的级别上，函数可能会成功，但是它的成功可能并不是应用程序开发人员想要的成功。  
  
 有关 OLE DB 返回代码的详细信息，请参阅 [Return Codes (OLE DB)](/previous-versions/windows/desktop/ms725451(v=vs.85))（返回代码 (OLE DB)）。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序成员函数返回 S_OK 时，该函数将成功。  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 提供程序成员函数未返回 S_OK，则 OLE/COM HRESULT 解压缩失败，IS_ERROR 宏可以确定函数的总体成功或失败。  
  
 如果 FAILED 或 IS_ERROR 返回 TRUE，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者可确保成员函数执行失败。 如果失败或 IS_ERROR 返回 FALSE，并且 HRESULT 不等于 S_OK，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序使用者可确保函数在某种程度上成功。 使用者可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序错误接口返回有关此 "包含信息成功" 的详细信息。 此外，如果函数明显失败 (则失败的宏将返回 TRUE) ，从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序错误接口可以获得扩展的错误信息。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者通常会遇到 "信息成功" HRESULT 返回 DB_S_ERRORSOCCURRED。 通常，返回 DB_S_ERRORSOCCURRED 的成员函数会定义一个或多个将状态值传递给使用者的参数。 除了在状态值参数中返回的错误信息之外，使用者无法获得其他任何错误信息，因此使用者应将应用程序逻辑实现为在有可用的状态值时检索这些状态值。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序成员函数不会返回成功代码 S_FALSE。 所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序成员函数始终返回 S_OK 以指示成功。  
  
## <a name="see-also"></a>另请参阅  
 [错误](../../relational-databases/native-client-ole-db-errors/errors.md)  
  
