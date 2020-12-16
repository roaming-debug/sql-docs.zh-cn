---
title: 利用 BCP 导入和导出批量数据
description: 使用 bcp 可从使用 SELECT 语句的 SQL Server 数据库中的任意位置导出数据。 从表或查询批量导出数据，并从文件批量导入。
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], bcp utility
- bulk importing [SQL Server], bcp utility
- bcp utility [SQL Server], about bcp utility
ms.assetid: 73e949de-67a3-4c84-9735-7da1ad4ba34a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 09/28/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 1bd365ee0af1bd1e8064b9bb6a5ba486abdf06cf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473978"
---
# <a name="import-and-export-bulk-data-using-bcp-sql-server"></a>使用 BCP 导入和导出批量数据 (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题概述了如何使用 [bcp 实用工具](../../tools/bcp-utility.md) 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中可使用 SELECT 语句的任意位置（包括分区视图）导出数据。  
  
 bcp 实用工具 (Bcp.exe) 是一个使用大容量复制程序 (BCP) API 的命令行工具。 bcp 实用工具可执行以下任务：  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的数据批量导出到数据文件中。  
  
-   从查询中批量导出数据。  
  
-   将数据文件中的数据批量导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。  
  
-   生成格式化文件。  
  
 bcp 实用工具可通过 **bcp** 命令进行访问。 使用 **bcp** 命令批量导入数据时，除非使用已有的格式化文件，否则必须了解表的架构及其各列的数据类型。  
  
 bcp 实用工具可将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的数据导出到数据文件，以供其他程序使用。 此实用工具还可将其他程序（通常为另一数据库管理系统 (DBMS)）中的数据导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 数据首先从源程序导出到数据文件，然后再通过单独的操作将数据文件中的数据复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。  
  
 **bcp** 命令提供可指定数据文件的数据类型和其他信息的开关。 如果未指定这些开关，则此命令会提示您指定格式信息，例如数据文件中数据字段的类型。 然后此命令会询问您是否要创建包含交互式响应的格式化文件。 如果希望在以后的批量导入或批量导出操作中具有灵活性，格式化文件通常会很有用。 可以在稍后对同等数据文件使用 **bcp** 命令时指定该格式化文件。 有关详细信息，请参阅[在使用 bcp 时指定数据格式以获得兼容性 (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)。  
  
>**注意!!** bcp 实用工具通过使用 ODBC 大容量复制进行编写。
  
 有关 **bcp** 命令语法的说明，请参阅 [bcp Utility](../../tools/bcp-utility.md)。  
  
## <a name="examples"></a>示例  

|以下主题包含有关使用 bcp的示例： |
|---|
|[bcp 实用工具](../../tools/bcp-utility.md)<br /><br />用于批量导入或导出的数据格式 (SQL Server)<br />&emsp;&#9679;&emsp;[使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[大容量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />用于导入或导出数据的格式化文件 (SQL Server)）<br />&emsp;&#9679;&emsp;[创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[批量导入和导出 XML 文档的示例 (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>                                                                                                                                                                                                                  </p>|

## <a name="more-examples-and-information"></a>更多示例和信息

- [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)

- [SELECT 子句 (Transact-SQL)](../../t-sql/queries/select-clause-transact-sql.md)

- [bcp 实用工具](../../tools/bcp-utility.md)

- [准备批量导入数据 (SQL Server)](../../relational-databases/import-export/prepare-to-bulk-import-data-sql-server.md)

- [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)

- [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)

- [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)

- [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)