---
description: 字符数据的自动转换
title: 字符数据的自动转换 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- result sets [ODBC], autotranslating character data
- data types [ODBC], autotranslating character data
- ACPs
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- AutoTranslate feature
- ANSI code pages
- character data autotranslation [ODBC]
- autotranslating character data
- SQL Server Native Client ODBC driver, data types
- ODBC data types, autotranslating character data
ms.assetid: 86a8adda-c5ad-477f-870f-cb370c39ee13
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2de8b3a1fc6b1bd6547ec413fb5d8e4e0fbca6dd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438195"
---
# <a name="autotranslation-of-character-data"></a>字符数据的自动转换
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  字符数据（如使用 SQL_C_CHAR 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **CHAR**、 **varchar** 或 **text** 数据类型存储在中的数据）声明的字符数据只能表示有限数量的字符。 对于每个字符使用一个字节进行存储的字符数据，它只能表示 256 个字符。 使用客户端计算机的 ANSI 代码页 (ACP) 解释存储在 SQL_C_CHAR 变量中的值。 使用服务器上的 **char**、 **varchar** 或 **text** 数据类型存储的值将使用服务器的 ACP 进行计算。  
  
 如果服务器和客户端具有相同的 ACP，则在解释 SQL_C_CHAR、 **CHAR**、 **varchar** 或 **text** 对象中存储的值时，它们没有任何问题。 如果服务器和客户端具有不同的 Acp，则从客户端 SQL_C_CHAR 的数据可能会在服务器上解释为不同的字符（如果在 **CHAR**、 **varchar** 或 **text** 列、变量或参数中使用）。 例如，在使用代码页437的计算机上，包含值0xA5 的字符字节被解释为字符-，并被解释为运行代码页1252的计算机上的日元符号 (¥) 。  
  
 Unicode 数据在存储时每个字符使用两个字节。 Unicode 规范涵盖所有扩展字符，因此每一 Unicode 字符都会在所有计算机上解释为相同的字符。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序的 AutoTranslate 功能尝试最大程度地减少在客户端和具有不同代码页的服务器之间移动字符数据的问题。 可以在 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)的连接字符串中设置 AutoTranslate，也可以在 [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md)的配置字符串中设置，也可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 ODBC 管理器为 Native Client ODBC 驱动程序配置数据源时设置。  
  
 如果将 AutoTranslate 设置为 "no"，则不会对客户端上的 SQL_C_CHAR 变量和数据库中的 **CHAR**、 **varchar** 或 **text** 列、变量或参数之间移动的数据进行转换 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果数据包含扩展字符并且客户端和服务器计算机使用不同的代码页，则位模式在这两台计算机上会得到不同的解释。 如果这两台计算机使用相同的代码页，则数据将得到相同的解释。  
  
 当 AutoTranslate 设置为 "yes" 时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序使用 Unicode 来转换客户端上 SQL_C_CHAR 变量和数据库中的 **CHAR**、 **varchar** 或 **text** 列、变量或参数之间移动的数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：  
  
-   将数据从客户端上的 SQL_C_CHAR 变量发送到数据库中的 **CHAR**、 **varchar** 或 **text** 列、变量或参数时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，ODBC 驱动程序将首先使用客户端的 ACP 从 SQL_C_CHAR 转换为 unicode，然后使用服务器的 ACP 将 unicode 转换回字符。  
  
-   将数据从数据库中的 **char**、 **varchar** 或 **text** 列、变量或参数发送 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到客户端上的 SQL_C_CHAR 变量时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序首先使用服务器的 ACP 将字符转换为 Unicode，然后使用客户端的 ACP 将 Unicode 转换回 SQL_C_CHAR。  
  
 由于所有这些转换都是通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在客户端上执行的 Native CLIENT ODBC 驱动程序来完成的，因此服务器 ACP 必须是客户端计算机上安装的代码页之一。  
  
 通过 Unicode 对字符进行转换可确保存在于两个代码页中的所有字符都能得到正确的转换。 但是，如果存在于其中一个代码页的某个字符在另一个代码页中不存在，则该字符在目标代码页中将无法表示。 例如，代码页 1252 包含注册商标符号 (®)，而代码页 437 则不包含此符号。  
  
 AutoTranslate 设置对以下转换没有任何影响：  
  
-   在字符 SQL_C_CHAR 客户端变量和 Unicode **nchar**、 **nvarchar** 或 **ntext** 列、变量或参数之间移动数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   SQL_C_WCHAR 客户端变量和数据库中的字符 **char**、 **varchar** 或 **text** 列、变量或参数之间移动数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 在数据从字符移动到 Unicode 时，始终必须对其进行转换。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;处理结果 ](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)   
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  
