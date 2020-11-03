---
title: 创建和管理全文索引目录
description: 创建和管理全文索引目录
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
f1_keywords:
- sql13.swb.fulltextsearch.ftcatalog.general.f1
- sql13.swb.fulltextsearch.fulltextindexproperties.general.f1
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- full-text search [SQL Server], using SQL Server Management Studio
ms.assetid: 824b7131-44a6-4815-89e6-62b7bab060e3
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a431635fdca556023a5e598502919bfdc9e40db6
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067329"
---
# <a name="create-and-manage-full-text-catalogs"></a>创建和管理全文索引目录

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
全文目录是适用于一组全文索引的逻辑容器。 在创建全文索引之前，必须创建全文目录。

全文目录是虚拟对象，不属于任何文件组。
  
##  <a name="create-a-full-text-catalog"></a><a name="creating"></a> 创建全文目录  

### <a name="create-a-full-text-catalog-with-transact-sql"></a>使用 Transact-SQL 创建全文目录
使用 [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)。 例如：

```sql 
USE AdventureWorks;  
GO  
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;  
GO  
``` 

### <a name="create-a-full-text-catalog-with-management-studio"></a>使用 Management Studio 创建全文目录
1.  在对象资源管理器中，展开服务器，展开“数据库”，然后展开要在其中创建全文目录的数据库。  
  
2.  展开“存储”，然后右键单击“全文目录”。  
  
3.  选择“新建全文目录”。  
  
4.  在“新建全文目录”对话框中，指定要重新创建的目录的信息。 有关详细信息，请参阅[新建全文目录（常规页）](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)。  
  
    > [!NOTE]  
    >  全文目录 ID 从 00005 开始，每创建一个新目录，其 ID 值就会递增 1。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

##  <a name="get-the-properties-of-a-full-text-catalog"></a><a name="props"></a> 获取全文目录的属性  
使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数 **FULLTEXTCATALOGPROPERTY** 获取与全文目录相关的各种属性的值。 有关详细信息，请参阅 [FULLTEXTCATALOGPROPERTY](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)。

例如，运行以下查询可获取全文目录 `Catalog1` 中的索引计数。

```sql 
USE <database>;  
GO  
SELECT fulltextcatalogproperty('Catalog1', 'ItemCount');  
GO  
```  
  
下表列出了与全文目录相关的属性。 此信息可用于全文搜索的管理和故障排除。 
  
|properties|说明|  
|--------------|-----------------|  
|**AccentSensitivity**|区分重音设置。|
|**ImportStatus**|是否将导入全文目录。|  
|**IndexSize**|全文目录的大小，以 MB 为单位。| 
|**ItemCount**|全文目录中当前包含的全文索引项的数目。|  
|**MergeStatus**|是否正在进行主合并。| 
|**PopulateCompletionAge**|上一次全文索引填充的完成时间与 01/01/1990 00:00:00 之间的时间差（秒）。| 
|**PopulateStatus**|填充状态。<br /><br /> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|  
|**UniqueKeyCount**|全文目录中的唯一键数。| 

##  <a name="rebuild-a-full-text-catalog"></a><a name="rebuildone"></a> 重新生成全文目录  

运行 Transact-SQL 语句 [ALTER FULLTEXT CATALOG ...REBUILD](
../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)，或者在 SQL Server Management Studio (SSMS) 中执行以下操作。

1.  在 SSMS 的对象资源管理器中，依次展开服务器、“数据库”、包含要重新生成的全文目录的数据库。  
  
2.  展开 **“存储”** ，然后展开 **“全文目录”** 。  
  
3.  右键单击要重新生成的全文目录的名称，并选择“重新生成”。  
  
4.  对于问题 **是否要删除并重新生成全文目录?** ，请单击“确定”。  
  
5.  在“重新生成全文目录”对话框中，单击“关闭”。  
   
##  <a name="rebuild-all-full-text-catalogs-for-a-database"></a><a name="rebuildall"></a> 为数据库重新生成所有全文目录  

1.  在 SSMS 的对象资源管理器中，依次展开服务器、“数据库”、包含要重新生成的全文目录的数据库。  
  
2.  展开“存储”，然后右键单击“全文目录”。  
  
3.  选择 **“全部重新生成”** 。  
  
4.  对于问题 **是否要删除并重新生成所有全文目录?** ，请单击“确定”。  
  
5.  在“重新生成所有全文目录”对话框中，单击“关闭”。  
  
  
  
##  <a name="remove-a-full-text-catalog-from-a-database"></a><a name="removing"></a> 从数据库中删除全文目录  

运行 Transact-SQL 语句 [DROP FULLTEXT CATALOG](
../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)，或在 SQL Server Management Studio (SSMS) 中执行以下操作。

1.  在 SSMS 的对象资源管理器中，依次展开服务器、“数据库”、包含要删除的全文目录的数据库。  
  
2.  展开 **“存储”** ，然后展开 **“全文目录”** 。  
  
3.  右键单击要删除的全文目录，然后选择“删除”。  
  
4.  在 **“删除对象”** 对话框中，单击 **“确定”** 。  

## <a name="next-step"></a>后续步骤
[创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)