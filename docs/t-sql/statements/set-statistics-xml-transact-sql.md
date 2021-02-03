---
description: SET STATISTICS XML (Transact-SQL)
title: SET STATISTICS XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_STATISTICS_XML_TSQL
- SET STATISTICS XML
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- STATISTICS XML option
- SET STATISTICS XML statement
- statements [SQL Server], statistical information
- XML [SQL Server], statement execution information
ms.assetid: 2b6d4c5a-a7f5-4dd1-b10a-7632265b1af7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f84e25eb3c24723a4fc46266fbbdb0d2c92dd635
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161079"
---
# <a name="set-statistics-xml-transact-sql"></a>SET STATISTICS XML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  使 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，并以定义完善的 XML 文档格式生成有关语句执行情况的详细信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET STATISTICS XML { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 SET STATISTICS XML 的设置是在执行或运行时设置的，而不是在分析时设置的。  
  
 当 SET STATISTICS XML 为 ON 时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在执行每条语句后返回该语句的执行信息。 该选项设置为 ON 后，将返回有关所有后续 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的信息，直到该选项设置为 OFF 为止。 请注意，SET STATISTICS XML 不必是批处理中的唯一语句。  
  
 SET STATISTICS XML 为应用程序（如 sqlcmd 实用工具）返回 nvarchar(max) 类型的输出，其中的 XML 输出随后将由其他工具用于显示和处理查询计划信息。  
  
 SET STATISTICS XML 返回的信息是一组 XML 文档。 SET STATISTICS XML ON 语句后面的每条语句都反映在输出的单个文档中。 每个文档都包含该语句的文本，后面是执行步骤的详细信息。 此输出显示了诸如开销、访问索引以及所执行操作的类型、联接顺序、某个物理操作的执行次数、每个物理运算符生成的行数等运行时信息。  
  
 在安装过程中，将把包含 XML 架构（该架构用于 SET STATISTICS XML 生成的 XML 输出）的文档复制到安装了 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的本地目录中。 它可以在包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装文件的驱动器上找到，具体位置如下：  
  
 \Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd  
  
 显示计划架构也可从[此网站](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)中找到。  
  
 SET STATISTICS PROFILE 和 SET STATISTICS XML 彼此互为对等物。 前者生成文本输出；后者生成 XML 输出。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，新的查询执行计划信息将只通过 SET STATISTICS XML 语句显示，而不通过 SET STATISTICS PROFILE 语句显示。  
  
> [!NOTE]  
>  如果在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中选择了“包括实际的执行计划”，则该 SET 选项不会生成 XML 显示计划输出。 请在使用该 SET 选项之前清除“包括实际的执行计划”按钮。  
  
## <a name="permissions"></a>权限  
 若要使用 SET STATISTICS XML 并查看输出，用户必须具有以下权限：  
  
-   执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的相应权限。  
  
-   对包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句引用的对象的所有数据库有 SHOWPLAN 权限。  
  
 对于不生成 STATISTICS XML 结果集的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，只需要具有执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的相应权限。 对于生成 STATISTICS XML 结果集的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，则要求对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句执行权限和 SHOWPLAN 权限的检查必须成功，否则将中止 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的执行，并且不生成任何 Showplan 信息。  
  
## <a name="examples"></a>示例  
 下面两条语句使用 SET STATISTICS XML 设置，显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在查询内对索引进行分析和优化的方法。 第一个查询在索引列上的 WHERE 子句中使用等于 (=) 比较运算符。 第二个查询在 WHERE 子句中使用 LIKE 运算符。 这将强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用聚集索引扫描并查找满足 WHERE 子句条件的数据。 EstimateRows 和 EstimatedTotalSubtreeCost 属性内用于第一个基于索引查询的值较小，这表示与非索引查询相比，该查询的处理速度要快得多，且使用的资源较少。  
  
```sql
USE AdventureWorks2012;  
GO  
SET STATISTICS XML ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, JobTitle   
FROM HumanResources.Employee  
WHERE JobTitle LIKE 'Production%';  
GO  
SET STATISTICS XML OFF;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET SHOWPLAN_XML (Transact-SQL)](../../t-sql/statements/set-showplan-xml-transact-sql.md)   
 [sqlcmd 实用工具](../../tools/sqlcmd-utility.md)  
  
  
