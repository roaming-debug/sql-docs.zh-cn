---
description: ISO 选项的作用
title: ISO 选项的效果 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ISO options (ODBC)
- ODBC applications, ISO options
- ODBC applications, statements
- SQL Server Native Client ODBC driver, ISO options
- statements [ODBC], ISO options
ms.assetid: 813f1397-fa0b-45ec-a718-e13fe2fb88ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 53012e9698cdd7132b6ea35a8296d84287a2767f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438370"
---
# <a name="effects-of-iso-options"></a>ISO 选项的作用
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 标准与 ISO 标准十分相似，而且 ODBC 应用程序期望 ODBC 驱动程序能提供标准行为。 为了使其行为符合 ODBC 标准中定义的行为， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序始终使用其连接 SQL Server 版本中提供的任何 ISO 选项。  
  
 当 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT odbc 驱动程序连接到实例时 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，服务器会检测到客户端正在使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] native client odbc 驱动程序并在上设置了几个选项。  
  
 驱动程序将自己发出这些语句；ODBC 应用程序不会做任何工作以请求它们。 设置这些选项可提高使用驱动程序的 ODBC 应用程序的可移植性，因为服务器的行为将与 ISO 标准保持一致。  
  
 基于 DB-Library 的应用程序通常不会打开这些选项。 当运行相同的 SQL 语句时，在 ODBC 或 DB-Library 客户端之间观察不同行为的站点不应假定这一点是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序的问题。 它们应该首先使用与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序使用的相同 SET 选项在 DB-Library 环境中重新运行该语句。  
  
 由于用户和应用程序可能会随时打开和关闭 SET 选项，存储过程和触发器的开发人员还应分别在上面列出的 SET 选项处于打开和关闭状态的情况下对其存储过程和触发器进行认真测试。 这可以确保存储过程和触发器正常工作，而无论特定连接在调用过程和触发器时打开哪些选项。 如果存储过程或触发器需要对这些选项之一进行特殊设置，则应该在触发器或存储过程的开始处发出 SET 语句。 此 SET 语句仅在触发器或存储过程执行期间保持有效；过程或触发器结束后，即会还原原始设置。  
  
 在连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例时，还会打开第四个 SET 选项 (CONCAT_NULL_YIELDS_NULL)。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]如果在数据源中或在[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)或[SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)中指定了 AnsiNPW = NO，则 Native Client ODBC 驱动程序不会将这些选项设置为 on。  
  
 与前面提到的 ISO 选项一样， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序不会在数据源中或在 **SQLDriverConnect** 或 **SQLBrowseConnect** 中指定 QuotedID = NO 时打开 QUOTED_IDENTIFIER 选项。  
  
 若要使驱动程序能够了解 SET 选项的当前状态，ODBC 应用程序不应使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] SET 语句设置这些选项。 它们只应使用数据源或连接选项设置这些选项。 如果应用程序发出 SET 语句，则驱动程序可能会生成错误的 SQL 语句。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行语句 ](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)   
 [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)   
 [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
  
