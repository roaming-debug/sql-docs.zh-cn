---
title: 准备用于批量导出或导入的数据
description: 本文介绍如何计划批量导入和批量导出操作，包括数据文件格式要求以及何时使用 bcp 实用工具。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], planning
- bulk importing [SQL Server], from a CSV file
- data formats [SQL Server], planning operations
- CSV files [SQL Server]
- quoted fields in CSV files [SQL Server]
ms.assetid: 783fd581-2e5f-496b-b79c-d4de1e09ea30
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: b33c6d9587ddb06d5315d7198663e4a9da84b656
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737707"
---
# <a name="prepare-data-for-bulk-export-or-import"></a>准备用于批量导出或导入的数据
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本部分讨论计划大容量导出操作时的相关注意事项以及大容量导入操作的要求。  
  
> [!NOTE]  
>  如果你不确定应如何针对批量导入设置数据文件的格式，则可以使用 **bcp** 实用工具将数据从表导出到数据文件中。 此文件中每个数据字段的格式均显示了将数据大容量导入对应表列时所要求的格式。 对数据文件的各个字段使用相同的数据格式。  
  
## <a name="data-file-format-considerations-for-bulk-export"></a>大容量导出的数据文件格式注意事项  
 在使用 **bcp** 命令执行批量导出操作之前，请先考虑下列事项：  
  
-   将数据导出到文件时， **bcp** 命令使用指定的文件名自动创建数据文件。 如果该文件名已经存在，正在大容量复制到数据文件的数据将覆盖文件中的现有内容。  
  
-   从表或视图大容量导出到数据文件要求对正在大容量复制的表或视图具有 SELECT 权限。  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以使用并行扫描来检索数据。 因此，通常不保证从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例大容量导出的表行在数据文件中按特定顺序排列。 为了确保批量导出的表行在数据文件中按特定顺序排列，请使用 **queryout** 选项来通过查询进行批量导出，并指定一个 ORDER BY 子句。  
  
## <a name="data-file-format-requirements-for-bulk-import"></a>大容量导入的数据文件格式要求  
 为了导入数据文件中的数据，该文件必须满足以下基本要求：  
  
-   数据必须以行和列的格式表示。  
  
> [!NOTE]  
>  数据文件的结构不必与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的结构一致，因为大容量导入过程中可以跳过列或对列重新排序。  
  
-   数据文件中的数据格式必须是支持的格式，例如字符格式或本机格式。  
  
-   数据可以是字符格式或本机二进制格式（包括 Unicode）。  
  
-   若要使用 **bcp** 命令、BULK INSERT 语句或 INSERT...SELECT * FROM OPENROWSET(BULK...) 语句导入数据，目标表必须已经存在。  
  
-   数据文件中的每个字段都必须与目标表中的对应列兼容。 例如，不能将 **int** 字段加载到 **datetime** 列。 有关详细信息，请参阅[用于批量导入或导出的数据格式 (SQL Server)](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)[在使用 bcp 时指定数据格式以获得兼容性 (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)  
  
    > [!NOTE]  
    >  要指定从数据文件导入的行子集而并非整个文件，可以使用带有 -F first_row 开关和/或 -L last_row 开关的 bcp 命令 。 有关详细信息，请参阅 [bcp Utility](../../tools/bcp-utility.md)。  
  
-   若要从包含固定长度或固定宽度字段的数据文件导入数据，请使用格式化文件。 有关详细信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)。  
  
-  从 SQL Server 2017 开始，CSV 文件可在将数据大容量导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时用作数据文件。 请注意，CSV 文件的字段终止符不一定是逗号。 若要能够用作大容量导入的数据文件，CSV 文件必须满足以下限制条件：  
  
    -   数据字段不包含任何字段终止符。  
  
    -   数据字段中的值可全部使用引号 ("") 引起或全部都不使用引号。  
  
     若要从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] FoxPro、Visual FoxPro 表 (.dbf) 文件或 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 工作表 (.xls) 文件大容量导入数据，需要将数据转换为 CSV 文件以符合前面的限制条件。 文件扩展名通常将为 .csv。 然后便可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量导入操作中使用 .csv 文件作为数据文件。  
  
     在 32 位系统（SQL Server 2014 及更低版本）上，通过使用 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 和 OLE DB Provider for Jet 可以将 CSV 数据导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，但是没有大容量导入优化。 通过由位于数据源所在目录的 schema.ini 文件定义的架构，Jet 将文本文件作为表处理。  对于 CSV 数据，schema.ini 文件内的其中一个参数将是“FORMAT=CSVDelimited”。 若要使用此解决方案，需要了解 Jet Text IISAM 如何工作（它的连接字符串语法、schema.ini 用法、注册表设置选项等等）。此信息的最佳来源是 Microsoft Access 帮助和知识库 (KB) 文章。 有关详细信息，请参阅[初始化文本数据源驱动程序](/office/client-developer/access/desktop-database-reference/initializing-the-text-data-source-driver)、[如何通过链接服务器使用 SQL Server 7.0 分布式查询来查询安全的 Access 数据库](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/246255)、[如何使用 Jet OLE DB Provider 4.0 连接到 ISAM 数据库](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/326548)，以及[如何使用 Jet Provider 的文本 IIsam 打开带分隔符的文本文件](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/262537)。  
  
 此外，将数据文件中的数据大容量导入表还有以下要求：  
  
-   用户必须对表具有 INSERT 和 SELECT 权限。 如果用户使用要求执行数据定义语言 (DDL) 操作（例如禁用约束）的选项时，他们还要具有 ALTER TABLE 权限。  
  
-   使用 BULK INSERT 或 INSERT ... SELECT * FROM OPENROWSET(BULK...) 大容量导入数据时，必须可以通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的安全性配置文件（如果用户使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的登录名进行登录）或在委托安全性下使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录名对数据文件进行读取操作。 此外，用户还必须具有 ADMINISTER BULK OPERATIONS 权限以读取文件。  
  
> [!NOTE]  
>  由于不支持大容量导入到分区视图，因此无法将数据大容量导入到分区视图。  
  
## <a name="external-resources"></a>外部资源  
 [如何将 Excel 数据导入 SQL Server](https://support.microsoft.com/kb/321686)  
  
## <a name="change-history"></a>更改历史记录  
  
|更新的内容|  
|---------------------|  
|添加了有关使用 OLE DB Provider for Jet 导入 CSV 数据的信息。|  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)   
 [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
