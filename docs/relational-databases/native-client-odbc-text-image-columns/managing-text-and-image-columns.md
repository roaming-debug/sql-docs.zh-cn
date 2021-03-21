---
description: 管理 Text 和 Image 列
title: 管理文本和图像列 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- text columns [ODBC]
- SQL Server Native Client ODBC driver, image columns
- SQL Server Native Client ODBC driver, text columns
- data types [ODBC], text
- columns [ODBC]
- ODBC data types, image columns
- data types [ODBC], mapping
- ODBC data types, text columns
- image columns [ODBC]
ms.assetid: 7b543556-ff36-4d35-ac08-de96223d92cd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5990ec13333c34fd5d737ea60658960d5530552a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737427"
---
# <a name="managing-text-and-image-columns"></a>管理 Text 和 Image 列
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**text**、 **ntext** 和 **image** 数据 (也称为长数据) 是可以保存数据值太大，无法放入 **char**、 **varchar**、 **binary** 或 **varbinary** 列的字符或二进制字符串数据类型。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Text** 数据类型映射到 ODBC SQL_LONGVARCHAR 数据类型;**ntext** 映射到 SQL_WLONGVARCHAR;和 **图像** 映射到 SQL_LONGVARBINARY。 某些数据项（例如很长的文档或大位图）可能因太大而无法在内存中合理存储。 若要从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连续部分中检索长数据， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] NATIVE Client ODBC 驱动程序允许应用程序调用 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md)。 若要以连续部分发送长数据，应用程序可以调用 [SQLPutData](../../relational-databases/native-client-odbc-api/sqlputdata.md)。 在执行时发送其数据的参数称为执行时数据参数。  
  
 尽管只能在部分中发送或检索 **字符** 和 **二进制** 数据，但应用程序实际上可以使用 **SQLPutData** 或 **SQLGetData** 来写入或检索任何类型的数据， (不只是长数据) 。 但是，如果数据太小，足以容纳在单个缓冲区中，通常没有理由使用 **SQLPutData** 或 **SQLGetData**。 将单一缓冲区绑定到参数或列更简单。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [绑定与未绑定的 Text 和 Image 列](../../relational-databases/native-client-odbc-text-image-columns/bound-vs-unbound-text-and-image-columns.md)  
  
-   [有日志记录的修改与无日志记录的修改](../../relational-databases/native-client-odbc-text-image-columns/logged-vs-unlogged-modifications.md)  
  
-   [执行时数据和 Text、ntext 或 Image 列](../../relational-databases/native-client-odbc-text-image-columns/data-at-execution-and-text-ntext-or-image-columns.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (ODBC)](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  
