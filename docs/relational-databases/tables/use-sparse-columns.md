---
description: 使用稀疏列
title: 使用稀疏列 | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- sparse columns, described
- null columns
- sparse columns
ms.assetid: ea7ddb87-f50b-46b6-9f5a-acab222a2ede
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ba2d4bc789c324e3e6153dc971a983638af603f4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482339"
---
# <a name="use-sparse-columns"></a>使用稀疏列

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  稀疏列是对 Null 值采用优化的存储方式的普通列。 稀疏列减少了 Null 值的空间需求，但代价是检索非 Null 值的开销增加。 当至少能够节省 20% 到 40% 的空间时，才应考虑使用稀疏列。 稀疏列和列集是通过使用 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 或 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 语句定义的。  
  
 稀疏列可以与列集和筛选索引一起使用：  
  
-   列集  
  
     INSERT、UPDATE 和 DELETE 语句可以通过名称来引用稀疏列。 但是，您也可以查看并处理表中组合为一个 XML 列的所有稀疏列。 此列称为列集。 有关列集的详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。  
  
-   筛选索引  
  
     因为稀疏列有许多 Null 值行，所以尤其适用于筛选索引。 稀疏列的筛选索引可以仅仅对已填充值的行编制索引。 这会创建一个更小、更有效的索引。 有关详细信息，请参阅 [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md)。  
  
 稀疏列和筛选索引使应用程序（如 [!INCLUDE[winSPServ](../../includes/winspserv-md.md)]）可以通过 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]有效地存储和访问大量的用户定义属性。  
  
## <a name="properties-of-sparse-columns"></a>稀疏列的属性  
 稀疏列具有以下特征：  
  
-   [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 使用列定义中的 SPARSE 关键字来优化该列中的值的存储。 因此，当表中的任意行的列值为 NULL 时，该值将不需要存储。  
  
-   具有稀疏列的表的目录视图与典型表的目录视图相同。 sys.columns 目录视图对于表中的每一列都包含一个对应的行，并包括一个列集（如果定义了列集）。  
  
-   稀疏列是存储层（而不是逻辑表）的一个属性。 因此，SELECT…INTO 语句不会将稀疏列属性复制到新表中。  
  
-   COLUMNS_UPDATED 函数返回一个 **varbinary** 值，以指示在 DML 操作期间更新的所有列。 COLUMNS_UPDATED 函数返回的位如下：  
  
    -   显式更新了稀疏列后，该稀疏列的对应位将设置为 1，列集的位将设置为 1。  
  
    -   显式更新了列集后，列集的位将设置为 1，该表中的所有稀疏列的位将设置为 1。  
  
    -   对于插入操作，所有位都将设置为 1。  
  
     有关列集的详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。  
  
 下面的数据类型不能指定为 SPARSE：  
  
|||  
|-|-|  
|**地理**|**text**|  
|**geometry**|**timestamp**|  
|**图像**|**用户定义数据类型**|  
|**ntext**||  
  
## <a name="estimated-space-savings-by-data-type"></a>按数据类型所估算的空间节省量  
 相对于未标记为 SPARSE 的相同数据所需的空间，稀疏列在存储非 Null 值时需要的存储空间更多。 下表说明了每种数据类型的空间使用情况。 **NULL 百分比** 列指示数据必须有多少比例为 NULL，才能实现 40% 的净空间节省。  
  
 **固定长度的数据类型**  
  
|数据类型|非稀疏字节|稀疏字节|NULL 百分比|  
|---------------|---------------------|------------------|---------------------|  
|**bit**|0.125|5|98%|  
|**tinyint**|1|5|86%|  
|**smallint**|2|6|76%|  
|**int**|4|8|64%|  
|**bigint**|8|12|52%|  
|**real**|4|8|64%|  
|**float**|8|12|52%|  
|**smallmoney**|4|8|64%|  
|**money**|8|12|52%|  
|**smalldatetime**|4|8|64%|  
|**datetime**|8|12|52%|  
|**uniqueidentifier**|16|20|43%|  
|**date**|3|7|69%|  
  
 **长度依赖于精度的数据类型**  
  
|数据类型|非稀疏字节|稀疏字节|NULL 百分比|  
|---------------|---------------------|------------------|---------------------|  
|**datetime2(0)**|6|10|57%|  
|**datetime2(7)**|8|12|52%|  
|**time(0)**|3|7|69%|  
|**time(7)**|5|9|60%|  
|**datetimetoffset(0)**|8|12|52%|  
|**datetimetoffset (7)**|10|14|49%|  
|**decimal/numeric(1,s)**|5|9|60%|  
|**decimal/numeric(38,s)**|17|21|42%|  
|**vardecimal(p,s)**|使用 **decimal** 类型作为保守的估计。|||  
  
 **长度依赖于数据的数据类型**  
  
|数据类型|非稀疏字节|稀疏字节|NULL 百分比|  
|---------------|---------------------|------------------|---------------------|  
|**sql_variant**|随基础数据类型而异|||  
|**varchar** 或 **char**|2*|4*|60%|  
|**nvarchar** 或 **nchar**|2*|4*+|60%|  
|**varbinary** 或 **binary**|2*|4*|60%|  
|**xml**|2*|4*|60%|  
|**hierarchyid**|2*|4*|60%|  
  
 *长度等于该类型中包含的数据的平均长度再多出 2 个或 4 个字节。  
  
## <a name="in-memory-overhead-required-for-updates-to-sparse-columns"></a>更新稀疏列所需的内存中开销  
 当使用稀疏列设计表时，请记住，在更新行时，表中的每个非 NULL 值稀疏列需要 2 个字节的额外开销。 由于此额外的内存要求，当总的行大小（包括此内存开销）超过 8019 但没有列可以推送到行外时，更新可能意外失败，错误为 576。  
  
 以具有 600 个 bigint 类型的稀疏列的表为例。 如果有 571 个非 Null 列，则磁盘上的总大小为 571 * 12 = 6852 字节。 在包含额外行开销和稀疏列标题之后，这会导致增加大约 6895 个字节。 在磁盘上，该页仍有约 1124 字节可用。 这可以给出其他列可以成功更新的印象。 但是，在更新过程中，内存中有额外开销，此开销为 2\*(非 NULL 值的稀疏列数目)。 在此示例中，包含额外开销 - 2 \* 571 = 1142 字节 - 将磁盘上的行大小增加到约 8037 个字节。 该大小超过最大允许的 8019 个字节。 由于所有列都是固定长度的数据类型，所以无法将其推送到行外。 因此，更新将会失败，错误为 576。  
  
## <a name="restrictions-for-using-sparse-columns"></a>使用稀疏列的限制  
 稀疏列可以是任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型，其行为与任何其他列类似，但有以下限制：  
  
-   稀疏列必须可为 Null，并且不能有 ROWGUIDCOL 或 IDENTITY 属性。 稀疏列不可以是以下数据类型： **text**、 **ntext**、 **image**、 **timestamp**、用户定义的数据类型、 **geometry** 或 **geography**；或者有 FILESTREAM 属性。  
  
-   稀疏列不能有默认值。  
  
-   稀疏列不能绑定到规则。  
  
-   虽然计算列可以包含稀疏列，但计算列不能标记为 SPARSE。  
  
-   可以在稀疏列上定义数据掩码，但属于列集的稀疏列除外。  
  
-   稀疏列不能是聚集索引或唯一主键索引的一部分。 但是，在稀疏列上定义的持久化和非持久化计算列可以是聚集键的一部分。  
  
-   稀疏列不能用作聚集索引或堆的分区键。 但是，稀疏列可以用作非聚集索引的分区键。  
  
-   稀疏列不能是用户定义的表类型的一部分，用户定义的表类型用在表变量和表值参数中。  
  
-   稀疏列与数据压缩不兼容。 因此稀疏列不能添加到压缩表，也不可以压缩包含稀疏列的任何表。  
  
-   将稀疏列更改为非稀疏列或者将非稀疏列更改为稀疏列都需要更改该列的存储格式。 SQL Server 数据库引擎使用以下过程进行这种更改：  
  
    1.  以新的存储大小和格式向表中添加一个新列。  
  
    2.  对于表中的每一行，更新存储在旧列中的值并将值复制到新列中。  
  
    3.  从表架构中删除旧列。  
  
    4.  重新生成表（如果没有聚集索引）或重新生成聚集索引以回收旧列所用的空间。  
  
    > [!NOTE]  
    >  当行中数据的大小超过允许的最大行大小时，步骤 2 将失败。 此大小包括存储在旧列中的数据和存储在新列中的已更新数据的大小。 对于不包含任何稀疏列的表，此限制为 8060 个字节；对于包含稀疏列的表为 8018 个字节。 即使已将所有合格列推送到行外，仍会出现此错误。  
  
-   将将非稀疏列改为稀疏列时，稀疏列将占用更多的空间来存储非 Null 值。 当行接近行大小的最大值限制时，操作将失败。  
  
## <a name="sql-server-technologies-that-support-sparse-columns"></a>支持稀疏列的 SQL Server 技术  
 本部分介绍下列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 技术如何支持稀疏列：  
  
-   事务复制  
  
     事务复制支持稀疏列，但它不支持可以与稀疏列一起使用的列集。 有关列集的详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。  
  
     SPARSE 属性的复制由架构选项来决定，架构选项是通过使用 [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 或者使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“项目属性”对话框来指定的。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本不支持稀疏列。 如果必须将数据复制到早期版本，需指定不应复制 SPARSE 属性。  
  
     对于发布的表，不能向表中添加任何新的稀疏列，也不能更改现有列的稀疏属性。 如果需要执行此类操作，则应删除然后再重新创建发布。  
  
-   合并复制  
  
     合并复制不支持稀疏列或列集。  
  
-   更改跟踪  
  
     更改跟踪支持稀疏列和列集。 当在表中更新列集时，更改跟踪将此视为整个行的更新。 由于不提供详细的更改跟踪，因此不能获取通过列集更新操作更新的稀疏列的准确集合。 如果通过 DML 语句显式更新稀疏列，则这些稀疏列上的更改跟踪将像平常一样工作，并且可以准确地识别出已更改的列的集合。  
  
-   更改数据捕获  
  
     变更数据捕获支持稀疏列，但不支持列集。  
  
-   复制表时，不保留列的稀疏属性。  
  
## <a name="examples"></a>示例  
 在本示例中，文档表包含具有 `DocID` 和 `Title`列的公共集合。 生产组希望所有生产文档都有一个 `ProductionSpecification` 列和一个 `ProductionLocation` 列。 市场组希望所有市场文档都有一个 `MarketingSurveyGroup` 列。 本示例中的代码创建一个使用稀疏列的表，向该表中插入两行，然后从该表中选择数据。  
  
> [!NOTE]  
>  该表只有五列，以便易于显示和读取。 如果设置了 ANSI_NULL_DFLT_ON 选项，则将稀疏列声明为可为 Null 是可选的。  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE TABLE DocumentStore  
    (DocID int PRIMARY KEY,  
     Title varchar(200) NOT NULL,  
     ProductionSpecification varchar(20) SPARSE NULL,  
     ProductionLocation smallint SPARSE NULL,  
     MarketingSurveyGroup varchar(20) SPARSE NULL ) ;  
GO  
  
INSERT DocumentStore(DocID, Title, ProductionSpecification, ProductionLocation)  
VALUES (1, 'Tire Spec 1', 'AXZZ217', 27);  
GO  
  
INSERT DocumentStore(DocID, Title, MarketingSurveyGroup)  
VALUES (2, 'Survey 2142', 'Men 25 - 35');  
GO  
```  
  
 从表中选择所有列会返回普通的结果集。  
  
```  
SELECT * FROM DocumentStore ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation  MarketingSurveyGroup`  
  
 `1      Tire Spec 1  AXZZ217                  27                  NULL`  
  
 `2      Survey 2142  NULL                     NULL                Men 25-35`  
  
 因为生产部门对市场数据不感兴趣，所以他们希望使用一个仅仅返回感兴趣的列的列列表，如下面的查询所示。  
  
```  
SELECT DocID, Title, ProductionSpecification, ProductionLocation   
FROM DocumentStore   
WHERE ProductionSpecification IS NOT NULL ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `DocID  Title        ProductionSpecification  ProductionLocation`  
  
 `1      Tire Spec 1  AXZZ217                  27`  
  
## <a name="see-also"></a>另请参阅  
 [使用列集](../../relational-databases/tables/use-column-sets.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
