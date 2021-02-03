---
description: SET STATISTICS IO (Transact-SQL)
title: SET STATISTICS IO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_STATISTICS_IO_TSQL
- IO
- IO_TSQL
- SET STATISTICS IO
dev_langs:
- TSQL
helpviewer_keywords:
- disk I/O statistics [SQL Server]
- I/O [SQL Server], disk activity information
- disks [SQL Server], statement statistics
- STATISTICS IO option
- statements [SQL Server], statistical information
- SET STATISTICS IO statement
- statistical information [SQL Server], disk activity
ms.assetid: 7033aac9-a944-4156-9ff4-6ef65717a28b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27084c92d8c3dfcd8b85317d1d2a9d0a70dd713d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206935"
---
# <a name="set-statistics-io-transact-sql"></a>SET STATISTICS IO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句所生成的磁盘活动量的相关信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET STATISTICS IO { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 当 STATISTICS IO 为“开”时，显示统计信息；为“关”时，不显示信息。   
  
 如果将此选项设置为“开”，则所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句均返回统计信息，直至将该选项设置为“关”。  
  
 下表列出并说明了各个输出项。  
  
|输出项|含义|  
|-----------------|-------------|  
|**表**|表的名称。|  
|**扫描计数**|在任意方向到达叶级别之后开始的搜索或扫描次数，搜索/扫描目的是检索所有用于构造输出的最终数据集的值。<br /><br /> 如果使用的索引是主键上的唯一索引或聚集索引，且只搜索一个值，则扫描计数为 0。 例如，`WHERE Primary_Key_Column = <value>`。<br /><br /> 当使用对非主键列定义的非唯一的聚集索引搜索一个值时，扫描计数为 1。 此过程的目的是针对你正在搜索的键值检查重复值。 例如，`WHERE Clustered_Index_Key_Column = <value>`。<br /><br /> 当 N 为通过使用索引键定位键值后，在叶级别的左侧或右侧启动的不同查找或扫描数时，则扫描计数为 N。|  
|**逻辑读取次数**|从数据缓存读取的页数。|  
|**物理读取次数**|从磁盘读取的页数。|  
|**预读次数**|为进行查询而放入缓存的页数。|  
|**lob 逻辑读取次数**|从数据缓存读取的页数。 包括 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max) 或列存储索引页。|  
|**lob 物理读取次数**|从磁盘读取的页数。 包括 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max) 或列存储索引页。|  
|**lob 预读次数**|为进行查询而放入缓存的页数。 包括 text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max) 或列存储索引页。|

 SET STATISTICS IO 是在执行或运行时设置，而不是在分析时设置。

> [!NOTE]  
> 当 Transact-SQL 语句检索 LOB 列时，有些 LOB 检索操作可能需要多次遍历 LOB 树。 这可能会导致 SET STATISTICS IO 报告的次数比预期的逻辑读取次数更高。

## <a name="permissions"></a>权限  
 若要使用 SET STATISTICS IO，用户必须具有执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的适当权限。 但不需要 SHOWPLAN 权限。  
  
## <a name="examples"></a>示例  
 此示例显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处理语句时，进行了多少次逻辑读和物理读操作。  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS IO ON;  
GO  
SELECT *   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS IO OFF;  
GO  
```  
  
 下面是结果集：  
  
```  
Table 'ProductCostHistory'. Scan count 1, logical reads 5, physical   
reads 0, read-ahead reads 0, lob logical reads 0, lob physical reads 0,   
lob read-ahead reads 0.  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL (Transact-SQL)](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME (Transact-SQL)](../../t-sql/statements/set-statistics-time-transact-sql.md)  
  
  
