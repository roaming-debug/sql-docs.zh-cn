---
title: Transact-SQL 引用（数据库引擎）
description: Transact-SQL 引用（数据库引擎）
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfe9a07eb1b37f0bddf128b1cbdf39de3d74a643
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689076"
---
# <a name="transact-sql-reference-database-engine"></a>Transact-SQL 引用（数据库引擎）
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

本主题提供有关如何查找和使用 Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)] (T-SQL) 的基础知识。 T-SQL 是使用 Microsoft SQL 产品和服务的关键所在。 与 SQL 数据库通信的所有工具和应用程序均通过发送 T-SQL 命令使用这些产品和服务。  

## <a name="t-sql-compliance-with-sql-standard"></a>T-SQL 符合 SQL 标准
有关如何在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中实现某些标准的详细技术文档，请参阅 [Microsoft SQL Server 标准支持文档](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2)。

## <a name="tools-that-use-t-sql"></a>使用 T-SQL 的工具
发出 T-SQL 命令的一些 Microsoft 工具如下：

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)

## <a name="locate-the-transact-sql-reference-topics"></a>查找 Transact-SQL 参考主题  
要查找 T-SQL 主题，请使用本页右上角的搜索功能或使用本页左侧的目录。 还可以在 Management Studio 查询编辑器窗口中键入 T-SQL 关键字并按 F1。

## <a name="find-system-views"></a>查找系统视图
要查找系统表、视图、函数和过程，请参阅 SQL 文档[使用关系数据库](../relational-databases/databases/databases.md)部分中的这些链接。

- [系统目录视图](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [系统兼容性视图](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [系统动态管理视图](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [系统函数](../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [系统信息架构视图](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [系统存储过程](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [系统表](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>“适用范围”参考  

T-SQL 参考主题涵盖 SQL Server 的多个版本（2008 版及更高版本），还包含其他 Azure SQL 服务。 每个主题的顶部附近都有一个指示支持该主题要点的产品和服务的部分。 

例如，主题适用于所有版本，具有以下标签。

[!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]

再如，以下标签指示仅适用于 Azure Synapse Analytics 和并行数据仓库的主题。

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

在某些情况下，某个产品或服务使用了主题，但并非所有参数都受到支持。 在此情况下，将其他“适用于”部分插入到该主题正文中相应的参数说明中。

## <a name="get-help-from-microsoft-q--a"></a>从 Microsoft 问答获取帮助

有关联机帮助的信息，请参阅 [Microsoft 问答 Transact-SQL 论坛](/answers/topics/sql-server-transact-sql.html)。

## <a name="see-other-language-references"></a>查看其他语言参考

SQL 文档包括包括如下其他语言参考：

- [XQuery 语言参考](../xquery/xquery-language-reference-sql-server.md)
- [Integration Services 语言参考](../integration-services/integration-services-language-reference.md)
- [复制语言参考](../relational-databases/replication/replication-language-reference.md)
- [Analysis Services 语言参考](../mdx/multidimensional-expressions-mdx-reference.md)

## <a name="next-steps"></a>后续步骤
现在，你已了解了如何查找 T-SQL 参考主题，可以：

- 学习有关如何编写 T-SQL 的简短教程，请参阅[教程：编写 Transact-SQL 语句](../t-sql/tutorial-writing-transact-sql-statements.md)。
- 查看 [Transact-SQL 语法约定 (Transact-SQL)](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。