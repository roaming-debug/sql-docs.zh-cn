---
title: SQL Server 文档 include 文件 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
author: MashaMSFT
ms.author: mathoma
ms.topic: conceptual
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: 97ff19bf81a574c4b6eb89a7c65e39ddfb4c74cc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97410502"
---
# <a name="sql-server-include-files-for-versioning-and-applies-to"></a>SQL Server include 文件，用于版本控制和 applies-to

通过使用 Markdown 中的 Include 文件，可以轻松修改文档中的引用，而无需更改各个文章的实际文本。 在 SQL 内容中有三种类型的 include 文件：SQL 版本、applies-to 和引用文本。 SQL Server 版本 include 文件用于指示讨论的 SQL 版本，例如 SQL Server 2016 或 2017  。 applies-to include 文件指示文档适用于什么 SQL 产品和服务，例如 Linux 上的 SQL Server 或 Azure SQL 数据库  。 引用文本 include 文件不属于其他两个类别，例如“Get Help” include - 客户可用于获得 SQL Server 相关帮助的链接列表  。

本文仅用作前两种类型的 include 文件的参考点。 可通过 [sql-docs 存储库](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes)浏览 include 文件的完整列表。

## <a name="sql-server-version-include-files"></a>SQL Server 版本 include 文件

SQL 内容写入程序经常需要添加 SQL Server 的产品名称和版本。 这样一来，如果名称发生了某些更改，将更新 include 文件，而不是手动更新每个文章的值。 这些 include 文件用作产品名称的占位符，但尚未在所有 SQL 文档中一致地使用。 SQL Server vNext 是指尚不具有版本号的 SQL 的未来版本，它在此情况中是一个特例。  

|SQL 版本| 文件名| Markdown 示例 |文本|
| :------------  | :-------------| :----------| :-------------------|
| SQL | ssnoversion-md.md | `[!INCLUDE[ssSQL11](../includes/ssnoversion-md.md)]` | SQL Server |
| SQL 2000 | ssversion2000-md.md | `[!INCLUDE[ssSQL11](../includes/ssversion2000-md.md)]` | SQL Server 2000 (8.x) |
| SQL 2005 | ssversion2005-md.md | `[!INCLUDE[ssSQL11](../includes/ssversion2005-md.md)]` | SQL Server 2005 (9.x) |
| SQL 2012 | sssql11-md.md | `[!INCLUDE[ssSQL11](../includes/sssql11-md.md)]` | SQL Server 2012 (11.x) |
| SQL 2012 SP1 | sssql11sp1-md.md | `[!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)]` | SQL Server 2012 SP1 (11.0.3x) |
| SQL 2014 | sssql14-md.md | `[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]` | SQL Server 2014 (12.x) |
| SQL 2016 | sssql15-md.md | `[!INCLUDE[sssql15-md](../includes/sssql15-md.md)]` | SQL Server 2016 (13.x) |
| SQL 2017 | sssql17-md.md | `[!INCLUDE[sssql17-md](../includes/sssql17-md.md)]` | SQL Server 2017 (14.x) |
| SQL 2017 | sssqlv14-md.md | `[!INCLUDE[sssqlv14](../includes/sssqlv14-md.md)]` | SQL Server 2017 (14.x) |
| SQL vNext | sssqlv15-md.md | `[!INCLUDE[sssqlv15-md](../includes/sssqlv15-md.md)]` | SQL Server vNext |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |  

## <a name="sql-server-applies-to-non-version-specific"></a>SQL Server Applies-to（非特定版本）

这些 applies-to include 文件忽略 SQL Server 的版本。

| 文件名| Markdown 示例 |映像|
| :-------------| :----------| :-------------------|
| appliesto-ss-asdb-asdw-xxx-md.md | `[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]` | [!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)] |
| appliesto-ss-asdb-asdw-pdw-md.md | `[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]` | [!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)] |
| appliesto-ss-asdb-xxxx-pdw-md.md | `[!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md.md](../includes/appliesto-ss-asdb-xxxx-pdw-md.md)]` | [!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md.md](../includes/appliesto-ss-asdb-xxxx-pdw-md.md)] |
| appliesto-ss-asdb-xxxx-xxx-md.md | `[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md.md](../includes/applies-to-version/sql-asdb.md)]` | [!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md.md](../includes/applies-to-version/sql-asdb.md)] |
| applies-to-version/sql-asdbmi.md | `[!INCLUDE[applies-to-version/sql-asdbmi.md](../includes/applies-to-version/sql-asdbmi.md)]` | [!INCLUDE[applies-to-version/sql-asdbmi.md](../includes/applies-to-version/sql-asdbmi.md)] |
| appliesto-ss-xxxx-asdw-pdw-md.md | `[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md.md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md.md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)] |
| appliesto-ss-xxxx-xxxx-pdw-md.md | `[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md.md](../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]` | [!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md.md](../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)] |
| appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md | `[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]` | [!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)] |
| appliesto-ss-xxxx-xxxx-xxx-md-winonly.md | `[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]` | [!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)] |
| applies-to-version/_ssnoversion.md | `[!INCLUDE[applies-to-version/_ssnoversion.md](../includes/applies-to-version/sqlserver.md)]` | [!INCLUDE[applies-to-version/_ssnoversion.md](../includes/applies-to-version/sqlserver.md)] |
| appliesto-xx-asdb-asdw-xxx-md.md | `[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md.md](../includes/appliesto-xx-asdb-asdw-xxx-md.md)]` | [!INCLUDE[appliesto-xx-asdb-asdw-xxx-md.md](../includes/appliesto-xx-asdb-asdw-xxx-md.md)] |
| appliesto-xx-asdb-xxxx-xxx-md.md | `[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md.md](../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md.md](../includes/appliesto-xx-asdb-xxxx-xxx-md.md)] |
| appliesto-xx-xxxx-asdw-pdw-md.md | `[!INCLUDE[appliesto-xx-xxxx-asdw-pdw-md.md](../includes/appliesto-xx-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[appliesto-xx-xxxx-asdw-pdw-md.md](../includes/appliesto-xx-xxxx-asdw-pdw-md.md)] |
| appliesto-xx-xxxx-asdw-xxx-md.md | `[!INCLUDE[appliesto-xx-xxxx-asdw-xxx-md.md](../includes/appliesto-xx-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[appliesto-xx-xxxx-asdw-xxx-md.md](../includes/appliesto-xx-xxxx-asdw-xxx-md.md)] |
|&nbsp; | &nbsp; | &nbsp; |  
 
## <a name="sql-server-applies-to-version-specific"></a>SQL Server Applies-to（特定版本）

这些 applies-to include 文件指定文档适用的 SQL 版本。

| 文件名| Markdown 示例 |映像|
| :-------------| :----------| :-------------------|
| tsql-appliesto-ss2008-all-md.md | `[!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)] |
| tsql-appliesto-ss2008-all-md.md | `[!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)] |
| tsql-appliesto-ss2008-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)] |
| tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)] |
| tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)] |
| tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)] |
| tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver.md)]` | [!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver.md)] |
| tsql-appliesto-ss2012-all-md.md | `[!INCLUDE[tsql-appliesto-ss2012-all-md.md](tsql-appliesto-ss2012-all-md.md)]` | [!INCLUDE[../includes/tsql-appliesto-ss2012-all-md.md](../includes/tsql-appliesto-ss2012-all-md.md)] |
| tsql-appliesto-ss2012-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)] |
| tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)] |
| tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2016-all-md.md | `[!INCLUDE[tsql-appliesto-ss2016-all-md.md](tsql-appliesto-ss2016-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-all-md.md](../includes/tsql-appliesto-ss2016-all-md.md)] |
| tsql-appliesto-ss2016-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)] |
| tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md)] |
| tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md)] |
| tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2017-all-md.md | `[!INCLUDE[tsql-appliesto-ss2017-all-md.md](tsql-appliesto-ss2017-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2017-all-md.md](../includes/tsql-appliesto-ss2017-all-md.md)] |
| tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver2017.md)]` | [!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver2017.md)] |
| sqlserver2017-asdb.md | `[!INCLUDE[sqlserver2017-asdb.md](../includes/applies-to-version/sqlserver2017-asdb.md)]` | [!INCLUDE[sqlserver2017-asdb.md](../includes/applies-to-version/sqlserver2017-asdb.md)] |
| tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)] |
| applies-to-version/asa-pdw.md | `[!INCLUDE[applies-to-version/asa-pdw.md](../includes/applies-to-version/asa-pdw.md)]` | [!INCLUDE[applies-to-version/asa-pdw.md](../includes/applies-to-version/asa-pdw.md)] |
| tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)] |
| tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)] |
|&nbsp; | &nbsp; | &nbsp; |  

## <a name="analysis-services-applies-to"></a>Analysis Services Applies-to

这些 applies-to include 文件用于 Analysis Services 文档。

| 文件名| Markdown 示例 |映像|
| :-------------| :----------| :-------------------|
| ssas-appliesto-sql2016.md | `[!INCLUDE[ssas-appliesto-sql2016.md](../includes/ssas-appliesto-sql2016.md)]` | [!INCLUDE[ssas-appliesto-sql2016.md](../includes/ssas-appliesto-sql2016.md)] |
| ssas-appliesto-sql2016-later.md | `[!INCLUDE[ssas-appliesto-sql2016-later.md](../includes/ssas-appliesto-sql2016-later.md)]` | [!INCLUDE[ssas-appliesto-sql2016-later.md](../includes/ssas-appliesto-sql2016-later.md)] |
| ssas-appliesto-sql2016-later-aas.md | `[!INCLUDE[ssas-appliesto-sql2016-later-aas.md](../includes/ssas-appliesto-sql2016-later-aas.md)]` | [!INCLUDE[ssas-appliesto-sql2016-later-aas.md](../includes/ssas-appliesto-sql2016-later-aas.md)] |
| ssas-appliesto-sql2017.md | `[!INCLUDE[ssas-appliesto-sql2017.md](../includes/ssas-appliesto-sql2017.md)]` | [!INCLUDE[ssas-appliesto-sql2017.md](../includes/ssas-appliesto-sql2017.md)] |
| ssas-appliesto-sql2017-later-aas.md | `[!INCLUDE[ssas-appliesto-sql2017-later-aas.md](../includes/ssas-appliesto-sql2017-later-aas.md)]` | [!INCLUDE[ssas-appliesto-sql2017-later-aas.md](../includes/ssas-appliesto-sql2017-later-aas.md)] |
| ssas.md | `[!INCLUDE[ssas.md](../includes/applies-to-version/ssas.md)]` | [!INCLUDE[ssas.md](../includes/applies-to-version/ssas.md)] |
| ssas-appliesto-sqlas-aas.md | `[!INCLUDE[ssas-aas.md](../includes/ssas-appliesto-sqlas-aas.md)]` | [!INCLUDE[ssas-appliesto-sqlas-aas.md](../includes/ssas-appliesto-sqlas-aas.md)] |
| ssas-appliesto-sqlas-all.md | `[!INCLUDE[ssas-all.md](../includes/ssas-appliesto-sqlas-all.md)]` | [!INCLUDE[ssas-appliesto-sqlas-all.md](../includes/ssas-appliesto-sqlas-all.md)] |
| ssas-appliesto-sqlas-all-aas.md | `[!INCLUDE[ssas-all-aas.md](../includes/ssas-appliesto-sqlas-all-aas.md)]` | [!INCLUDE[ssas-appliesto-sqlas-all-aas.md](../includes/ssas-appliesto-sqlas-all-aas.md)] |
|&nbsp; | &nbsp; | &nbsp; |  

## <a name="reporting-services-applies-to"></a>Reporting Services Applies-to

这些 applies-to include 文件用于 Reporting Services 文档。

| 文件名| Markdown 示例 |映像|
| :-------------| :----------| :-------------------|
| ssrs-appliesto-2017-and-later.md | `[!INCLUDE[ssrs-appliesto-2017-and-later.md](../includes/ssrs-appliesto-2017-and-later.md)]` | [!INCLUDE[ssrs-appliesto-2017-and-later.md](../includes/ssrs-appliesto-2017-and-later.md)] |
| ssrs-appliesto-not-pbirs.md | `[!INCLUDE[ssrs-appliesto-not-pbirs.md](../includes/ssrs-appliesto-not-pbirs.md)]` | [!INCLUDE[ssrs-appliesto-not-pbirs.md](../includes/ssrs-appliesto-not-pbirs.md)] |
| ssrs-appliesto-pbirs.md | `[!INCLUDE[ssrs-appliesto-pbirs.md](../includes/ssrs-appliesto-pbirs.md)]` | [!INCLUDE[ssrs-appliesto-pbirs.md](../includes/ssrs-appliesto-pbirs.md)] |
| ssrs-appliesto-sharepoint-2013-2016.md | `[!INCLUDE[ssrs-appliesto-sharepoint-2013-2016.md](../includes/ssrs-appliesto-sharepoint-2013-2016.md)]` | [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016.md](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] |
| ssrs-appliesto-sql2016-preview.md | `[!INCLUDE[ssrs-appliesto-sql2016-preview.md](../includes/ssrs-appliesto-sql2016-preview.md)]` | [!INCLUDE[ssrs-appliesto-sql2016-preview.md](../includes/ssrs-appliesto-sql2016-preview.md)] |
|&nbsp; | &nbsp; | &nbsp; |  

## <a name="next-steps"></a>后续步骤

要详细了解如何使用这些 include 文件，请参阅 [Applies-to include](sql-server-docs-contribute.md#applies-to-includes)。

这是测试
