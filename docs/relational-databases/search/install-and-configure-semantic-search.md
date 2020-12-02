---
description: 安装和配置语义搜索
title: 安装和配置语义搜索 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- semantic search [SQL Server], installing
- semantic search [SQL Server], configuring
ms.assetid: 2cdd0568-7799-474b-82fb-65d79df3057c
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 9d3339525dbf67ee6dd1a4e4ae3b75215dd2c05d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91868060"
---
# <a name="install-and-configure-semantic-search"></a>安装和配置语义搜索
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  说明统计语义搜索的必备组件以及如何安装或检查它们。  
  
## <a name="install-semantic-search"></a>安装语义搜索  
  
###  <a name="check-whether-semantic-search-is-installed"></a><a name="HowToCheckInstalled"></a> 检查是否安装了语义搜索  
 查询 [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md) 元数据函数的 **IsFullTextInstalled** 属性。  
  
 返回值 1 表示安装了全文搜索和语义搜索；返回值 0 表示未安装它们。  
  
```sql  
SELECT SERVERPROPERTY('IsFullTextInstalled');  
GO  
```  
  
###  <a name="install-semantic-search"></a><a name="BasicsSemanticSearch"></a> 安装语义搜索  
 若要安装语义搜索，在 SQL Server 安装过程中，请在“要安装的功能”页上选择“全文和语义提取搜索”。  
  
 统计语义搜索依赖于全文搜索。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的这两个可选功能是一起安装的。  
  
## <a name="install-the-semantic-language-statistics-database"></a>安装语义语言统计数据库  
 语义搜索具有一个称为语义语言统计数据库的附加外部依赖项。 此数据库包含语义搜索所需的统计语言模型。 单个语义语言统计数据库包含语义索引支持的所有语言的语言模型。  
  
###  <a name="check-whether-the-semantic-language-statistics-database-is-installed"></a><a name="HowToCheckDatabase"></a> 检查是否安装了语义语言统计数据库  
 查询目录视图 [sys.fulltext_semantic_language_statistics_database (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md)。  
  
 如果为该实例安装并注册了语义语言统计数据库，则查询结果将包含有关该数据库的单行信息。  
  
```sql  
SELECT * FROM sys.fulltext_semantic_language_statistics_database;  
GO  
```  
  
###  <a name="install-attach-and-register-the-semantic-language-statistics-database"></a><a name="HowToInstallModel"></a> 安装、附加和注册语义语言统计数据库  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不安装语义语言统计数据库。 若要将语义语言统计数据库设置为语义索引的必备组件，请执行以下操作：  
  
 **1.安装语义语言统计数据库。**  
 
 1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质上找到语义语言统计数据库，或者从 Web 上下载它。  
  
        1.  在 **安装介质上找到名为** SemanticLanguageDatabase.msi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Windows 安装程序包。  
  
        2.  从 [!INCLUDE[msCoName](../../includes/msconame-md.md)]下载中心的 [Microsoft® SQL Server® 2016 语义语言统计](https://www.microsoft.com/download/details.aspx?id=52681)页上下载安装程序包。  
  
2.  运行 **SemanticLanguageDatabase.msi** Windows 安装程序包，以提取数据库和日志文件。  
  
     也可以选择更改目标目录。 默认情况下，安装程序将文件提取到 Program Files 文件夹中名为 **Microsoft Semantic Language Database** 的文件夹。 MSI 文件包含压缩的数据库文件和日志文件。  
  
3.  将提取的数据库文件和日志文件移到文件系统中的合适位置。  
  
     如果将文件放入默认位置，则不可能为另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例提取数据库的另一个副本。  
  
    > [!IMPORTANT]  
    >  提取语义语言统计数据库时，向文件系统默认位置中的数据库文件和日志文件分配受限权限。 因此，如果将文件放入默认位置，您可能没有附加该数据库的权限。 如果在尝试附加数据库时引发了错误，请删除这些文件，或检查并根据需要修复文件系统权限。  
  
 **2.附加语义语言统计数据库。**
   
 使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 或通过 **FOR ATTACH** 语法调用 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md) 将数据库附加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅[数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)。  
  
 默认情况下，该数据库的名称为 **semanticsdb**。 也可以选择在附加数据库时为数据库提供其他名称。 当使用后续步骤注册数据库时，必须提供此名称。  
  
```sql  
CREATE DATABASE semanticsdb  
            ON ( FILENAME = 'C:\Microsoft Semantic Language Database\semanticsdb.mdf' )  
            LOG ON ( FILENAME = 'C:\Microsoft Semantic Language Database\semanticsdb_log.ldf' )  
            FOR ATTACH;  
GO  
```  
  
 此代码示例假定您将数据库从其默认位置移动到新位置。  
  
 **3.注册语义语言统计数据库。** 
  
 调用 [sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-register-language-statistics-db-transact-sql.md) 存储过程，并提供在附加数据库时向该数据库提供的名称。  
  
```sql  
EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = N'semanticsdb';  
GO  
```  

##  <a name="requirements-and-restrictions-for-the-semantic-language-statistics-database"></a><a name="reqinstall"></a> 语义语言统计数据库的要求和限制  
  
-   在一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上只能附加和注册一个语义语言统计数据库。  
  
     单个计算机上的每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例都需要一个单独的语义语言统计数据库的物理副本。 对每个实例附加一个副本。  
  
-   您不能分离注册的有效语义语言统计数据库，并将其替换为一个任意同名的数据库。 这样做将导致当前或未来的索引填充失败。  
  
-   语义语言统计数据库是只读的。 您不能自定义此数据库。 如果您以任何方式更改数据库的内容，未来语义索引的结果将是不确定的。 若要恢复此数据的原始状态，您可以删除已更改的数据库，然后下载并附加未更改的数据库新副本。  
  
-   可以分离或删除语义语言统计数据库。 如果存在任何对数据库具有读锁定的当前索引操作，则分离或删除操作将失败或超时。这与现有行为一致。 删除该数据库后，任何语义索引操作都将失败。  
 
##  <a name="remove-the-semantic-language-statistics-database"></a><a name="HowToUnregister"></a> 删除语义语言统计数据库  

###  <a name="unregister-detach-and-remove-the-semantic-language-statistics-database"></a>取消注册、分离和删除语义语言统计数据库 

 **1.取消注册语义语言统计数据库。**
   
 调用 [sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-unregister-language-statistics-db-transact-sql.md) 存储过程。 由于一个实例仅有一个语义语言统计数据库，因此您不必提供该数据库的名称。  
  
```sql  
EXEC sp_fulltext_semantic_unregister_language_statistics_db;  
GO  
```  
  
 **2.分离语义语言统计数据库。**  
 
 调用 [sp_detach_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) 存储过程并提供该数据库的名称。  
  
```sql  
USE master;  
GO  
  
EXEC sp_detach_db @dbname = N'semanticsdb';  
GO  
```  
  
 **3.删除语义语言统计数据库。**  
 
 撤消注册并分离该数据库后，您可以删除数据库文件。 不提供卸载程序，在“控制面板”的 **“程序和功能”** 中没有相应条目。  
  
## <a name="install-optional-support-for-newer-document-types"></a>安装对较新文档类型的可选支持  
  
###  <a name="install-the-latest-filters-for-microsoft-office-and-other-microsoft-document-types"></a><a name="office"></a> 为 Microsoft Office 和其他 Microsoft 文档类型安装最新筛选器  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装最新的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 断字符和词干分析器，但是不为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office 文档和其他 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 文档类型安装最新的筛选器。 要为使用最新版本的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office 和其他 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 应用程序创建的文档编制索引，必须安装这些筛选器。 若要下载最新的筛选器，请参阅 [Microsoft Office 2010 Filter Packs](https://www.microsoft.com/download/details.aspx?id=17062)。 （对于 Office 2013 或 Office 2016，不会显示为筛选器包版本。）
  
