---
title: 数据库引擎：中断性变更 | Microsoft Docs
titleSuffix: SQL Server 2016
description: 了解 SQL Server 2016 (13.x) 及更早版本中的数据库引擎变更，这些变更可能会在升级后破坏之前版本的功能。
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], what's new
- breaking changes [SQL Server]
ms.assetid: 47edefbd-a09b-4087-937a-453cd5c6e061
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea6799570df43f5d49cadd93b98dc89528acacdd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171049"
---
# <a name="breaking-changes-to-database-engine-features-in-sql-server-2016"></a>SQL Server 2016 中数据库引擎功能的中断性变更

[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

  本主题介绍 [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] 和更低版 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的中断性变更。 这些更改可能导致基于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的早期版本的应用程序、脚本或功能无法继续使用。 在进行升级时可能会遇到这些问题。  
  
##  <a name="breaking-changes-in-sssql15"></a><a name="SQL15"></a>[!INCLUDE[ssSQL15](../includes/sssql16-md.md)] 中的重大更改  
  
-   `sys.dm_io_virtual_file_stats` 的 sample_ms  列已从 int  扩展到 bigint  数据类型。  
  
-   `sys.fn_virtualfilestats` 的 TimeStamp  列已从 int  扩展到 bigint  数据类型。  

-   在数据库兼容级别 130 以下，通过考虑导致不同转换值的毫秒小数部分，从 **datetime** 到 **datetime2** 数据类型的隐式转换显得更加准确。 只要 datetime 和 datetime2 数据类型之间存在混合比较情况，就需要使用 datetime2 数据类型的隐式转换。 有关详细信息，请参阅此 [Microsoft 支持文章](https://support.microsoft.com/help/4010261)。

-   数据库兼容性级别为 130 时，在某些数字和日期时间数据类型之间执行隐式转换的操作显示出更高的准确性，并且可能导致不同的转换值。 这包括使用需要计算的函数，例如，`DATEDIFF` 和 `ROUND`。 有关详细信息，请参阅此 [Microsoft 支持文章](https://support.microsoft.com/help/4010261)。

## <a name="previous-versions"></a><a name="previous-versions"></a> 先前版本  

有关 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 和一些早期版本中重大更改的信息，请参阅 [SQL Server 2014 中数据库引擎功能的重大更改](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016)。

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>较早版本的 SQL Server 的存档文档

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 中不推荐使用的数据库引擎功能](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [SQL Server 2016 中废止的数据库引擎功能](./discontinued-database-engine-functionality-in-sql-server.md)   
 [SQL Server 数据库引擎的向后兼容性](./discontinued-database-engine-functionality-in-sql-server.md)   
 [ALTER DATABASE 兼容级别 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
 [适用于 Windows 的 SQL Server 2016 或 SQL Server 2017 在处理某些数据类型和不常见操作方面所做的改进](https://support.microsoft.com/help/4010261)