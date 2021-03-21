---
title: 用于大容量导入和导出的数据格式
description: SQL Server 接受字符或本机二进制格式的数据。 在 SQL Server 和其他应用之间使用字符格式，在 SQL Server 实例之间使用本机格式。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], choosing
- bulk importing [SQL Server], data formats
ms.assetid: 73fe6741-9437-4b26-b030-28b863e74399
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 9a71933adc9e99fadad04fb2c94d9b114a9005bc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749677"
---
# <a name="data-formats-for-bulk-import-or-bulk-export-sql-server"></a>用于大容量导入或导出的数据格式 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以接受字符数据格式或本机二进制数据格式的数据。 当在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和其他应用程序（例如， [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel）或其他数据库服务器（例如，Oracle 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）之间移动数据时，请使用字符格式。 只有在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例之间传输数据时才可以使用本机格式。  
  
 **本主题内容：**  
  
-   [用于大容量导入或导出的数据格式](#ComponentsAndConcepts)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="data-formats-for-bulk-import-or-export"></a><a name="ComponentsAndConcepts"></a> 用于大容量导入或导出的数据格式  
 下表列出了不同数据显示方式和操作的源或目标一般适合使用的数据格式。  
  
|Operation|本机|Unicode 本机|字符|Unicode 字符|  
|---------------|------------|--------------------|---------------|-----------------------|  
|使用不包含任何扩展字符或双字节字符集 (DBCS) 字符的数据文件在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间进行大容量的数据传输。 除非使用格式化文件，否则这些表的定义必须相同。|是*|-|-|-|  
|对于“sql_variant”  列，最好使用本机数据格式，因为本机数据格式可以保留每一个 **sql_variant** 值的元数据，这一点不同于字符格式或 Unicode 格式。|是|-|-|-|  
|使用包含扩展字符或 DBCS 字符的数据文件在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间进行大容量的数据传输。|-|是|-|-|  
|大容量导入其他程序生成的文本文件中的数据。|-|-|是|-|  
|将数据大容量导出到要在其他程序中使用的文本文件中。|-|-|是|-|  
|使用包含 Unicode 数据而不包含任何扩展字符或 DBCS 字符的数据文件在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间进行大容量的数据传输。|-|-|-|是|  
  
 \* 这是在使用 **bcp** 时从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 批量导出数据的最快方法。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [在使用 bcp 时指定数据格式以获得兼容性 (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)  
  
  
