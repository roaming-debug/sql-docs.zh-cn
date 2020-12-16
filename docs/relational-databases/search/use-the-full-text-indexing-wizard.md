---
description: 使用全文索引向导
title: 使用全文索引向导 | Microsoft Docs
ms.date: 08/19/2016
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
f1_keywords:
- sql13.swb.fulltextindexingwizard.welcome.f1
- sql13.swb.fulltextindexingwizard.selectorcreatepopschedules.f1
- sql13.swb.fulltextindexingwizard.progress.f1
- sql13.swb.fulltextindexingwizard.selectchangetracking.f1
- sql13.swb.fulltextindexingwizard.selectacatalog.f1
- sql13.swb.fulltextindexingwizard.selectatableorview.f1
- sql13.swb.fulltextindexingwizard.selectanindex.f1
- sql13.swb.fulltextindexingwizard.summary.f1
- sql13.swb.fulltextindexingwizard.selecttablecolumns.f1
helpviewer_keywords:
- Full-Text Indexing Wizard
- full-text search [SQL Server], Full-Text Indexing Wizard
ms.assetid: 3e9d9605-6525-4781-9168-fdaa06db3459
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 17fd1c0c884f606d1ede8e6dfa392fd0645185f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479388"
---
# <a name="use-the-full-text-indexing-wizard"></a>使用全文索引向导
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  SSMS 中的全文索引向导可引导你完成一系列步骤，以帮助你创建全文索引。  
  
## <a name="create-a--full-text-index"></a>创建全文索引 

1. 在对象资源管理器中，右键单击要对其创建全文索引的表，指向“全文索引”，然后单击“定义全文索引”。 此操作将在一个单独的窗口中启动向导。
   单击“下一步” 
  
2. **唯一索引。**  从下拉列表中选择索引。 索引必须是唯一且不为 Null 的单键列索引。 为全文唯一键选择最小的唯一键索引。 为了获得最佳性能，建议使用聚集索引。  
  
3.  **可用列。** 选中要包含的列的所有列名称旁边的复选框。  选中列名称旁边的复选框。 不符合要求的列将灰显，其复选框处于禁用状态。  
  
4. **断字符语言。** 从下拉列表中选择语言。 此选项用于为索引标识正确的断字符。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用断字符在全文索引的数据中标识词的边界。  
  
5.  **类型列。** 选择存储作为全文索引列的文档类型的列名称。  

> **注意：** 只有当“可用列”列中命名的列为 **varbinary(max)** 或 **image** 类型时，才会启用“类型列”。  
  
6. **统计语义。** 选择是否为所选列启用语义索引。 有关详细信息，请参阅[语义搜索 (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md)。  
  
>**说明** 
>
>如果所选语言没有关联的语义语言模型，则不会启用“统计语义”复选框。 如果你在选择“语言”前选择“统计语义”，则下拉组合框中提供的语言将限制为存在语义语言模型支持的那些语言。  
>
> 语义搜索 **不适用于 Azure SQL 数据库。** 在 Azure SQL 数据库上运行此向导时，不会显示“统计语义”选项。
  
7. 选择更改跟踪选项。  
  
     **自动**  
     选中此单选按钮后，当基础数据发生更改时，全文索引将自动更新。  
  
     **手动**  
     如果不希望基础数据发生更改时自动更新全文索引，请选中此单选按钮。 对基础数据的更改将保留下来。 不过，若要将更改应用到全文索引，必须手动启动或安排此进程。  
  
     **不跟踪更改**  
     如果不希望使用基础数据的更改对全文索引进行更新，请选中此单选按钮。  
  
8.  创建索引时启动完全填充（仅在不跟踪更改时可用）。
  
     选中此单选按钮，可以在此向导成功完成后启动完全填充。 这将包括在目录中创建全文索引结构，并用全文索引的数据对其进行填充。  
     
     单击“下一步”
  
## <a name="catalog-index-filegroup-and-stoplist"></a>目录、索引文件组和非索引字表   
  
9.  **选择全文目录**  

     **选择目录：** 从列表中选择全文目录。 默认情况下，数据库的默认目录为该列表中选定的项。 如果没有可用的目录，则该列表将处于禁用状态，并且 **“创建新目录”** 复选框将处于选中状态并被禁用。  
  
  OR
  
 10. **“创建新目录”**
 - 选择全文目录。  
  
    a. **名称**  
     为新的全文目录输入一个名称。  
  
     b. **设置为默认目录**  
     选中此项可以将该目录设为此数据库的默认目录。  
  
     c. **区分重音**  
     指定新目录是区分重音还是不区分重音。 如果数据库区分重音，默认情况下会选中“区分”。  
  
     d. **选择索引文件组**  
     指定对其创建全文索引的文件组。  
  
     e. 选择值：  
      |值|描述|  
      |-----------|-----------------|
      |**<default>**| 如果表或视图尚未分区，则选择此值，将与基础表或视图使用相同的文件组。 如果表或视图已分区，则使用主文件组|
      |**PRIMARY**|选择此值可将主文件组用于新全文索引。|
      *用户指定的默认文件组*|如果存在用户定义的默认非索引字表，请从列表选择其名称，以便将该文件组用于新全文检索。|   
  
     
 11. **选择全文非索引字表**  
     指定要用于全文索引的非索引字表，或者禁用非索引字表。  
  
     使用称为“非索引字表”的对象在数据库中管理非索引字。 “非索引字表”是一个由非索引字组成的列表，这些非索引字在与全文检索关联时会应用于该索引的全文查询。 有关详细信息，请参阅 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)。  
  
     选择下列值之一：  
  
   |值|描述|  
    |-----------|-----------------|  
    |**<system>**|选择此值将对新全文索引使用系统非索引字表。 这是默认值。|  
    |**<off>**|选择此值将禁用新全文索引的非索引字表。|  
    |*用户定义的非索引字表名称*|该列表显示已对数据库创建的用户定义的每个非索引字表（如果有）的名称。 选择任何要用于新全文索引的用户定义的非索引字表。|  
  
  单击“下一步”
  
11. （可选）SQL Server 仅定义填充计划。 索引操作将立即开始，除非已将其安排为在将来执行。 此时将立即创建计划，尽管在计划时间之前并不会运行这些计划。  
  
     **新建表计划**  
     定义表的填充计划。  
  
     **新建目录计划**  
     定义全文目录的填充计划。  
  
     **编辑**  
     编辑计划。  
  
     **删除**  
     删除计划。  
  
5.  查看或控制全文索引向导的进度。  
  
     **Stop**  
     中断当前操作，并阻止该向导在此会话期间执行后续全文操作。  
  
     **Report**  
     当执行完所有操作以后，单击此按钮可以访问所执行操作的报告。 您可以查看该报告，将其打印为文件，复制到剪贴板，或用电子邮件发送该报告。  
  
  
