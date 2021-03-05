---
title: 幕后的 Azure Synapse Pathway 预览版。
description: 深入了解 Azure Synapse 通道如何转换代码。
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-concept
ms.openlocfilehash: 9f23aa23ef40ee7df5ad601b73ad526df7bcf0da
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186295"
---
# <a name="azure-synapse-pathway-preview-behind-the-scenes"></a>幕后的 Azure Synapse Pathway 预览版
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway 的目标是在针对 Synapse SQL 进行优化的同时，保留原始代码的功能意图。 Synapse Pathway 使用一个三阶段过程将 SQL 代码从源系统转换到 Azure Synapse SQL。

每一个阶段都保留并增加了源的知识（包括特定于源的元数据），以确保最高质量的转换。

 ![Azure Synapse Pathway。](./media/synapse-pathway-behind-the-scenes/behind-the-scene.png)

## <a name="stage-1--lexing-and-parsing"></a>阶段 1 - 词法和句法分析

SQL 语言分析是一个已经解决了很多次的问题。 许多商业和开源分析程序都有助于执行以下基础过程：获取一个源语句，将其分解成逻辑标记，然后针对集或分析程序规则执行，以确保语言的一致性。 

Synapse Pathway 定义了源语法，该语法允许工具识别并处理输入到用于进一步处理的已扩充抽象语法树 (AST) 中的 SQL。 

## <a name="stage-2---augmented-abstract-syntax-tree-ast"></a>阶段 2 - 已扩充的抽象语法树 (AST)

Synapse Pathway 定义了已扩充抽象语法树 (AST) 中所有对象的常见表示形式。 Synapse Pathway AST 包含附加的语句或分段元数据，用于在两个系统之间转换代码时做出正确的决策。

脚本生成组件不仅会跟踪令牌是一个函数，还会跟踪源系统类型需求，从而可对转换到 Synapse SQL 做出更明智的决策。

例如，绝对函数的源函数被定义为：

```sql  
ABS( float_expression ) 
```

Azure Synapse SQL 将绝对函数定义为：

```sql  
ABS ( numeric_expression )  
```

在此简单示例中，Synapse Pathway 理解 Synapse SQL 中从浮点到数值的转换是隐式的[转换](../../t-sql/functions/cast-and-convert-transact-sql.md?view=azure-sqldw-latest&preserve-view=true#implicit-conversions)，不需要进一步的类型强制转换。 简单、简洁且有效的代码转换。

保留关于源语句和片段的元数据信息有助于区分平台之间的结构差异 - 例如，WHERE 子句中搜索条件谓词的选择退出逻辑中的转换。

## <a name="stage-3---syntax-generation"></a>阶段 3 - 语法生成

该过程的最后一个阶段是生成 Synapse SQL 的语法。 使用从源文件生成的 AST 结构，Synapse Pathway 将每个 DDL 对象写入单个文件。 语法生成器使用目标平台的深度知识来优化语句。

例如，在数据加载场景中常见的一种模式是先删除一个临时表中的所有内容，然后以 INSERT/SELECT 方式从另一个临时表中加载数据。

```sql  
DELETE staging.table1 ALL;
INSERT INTO staging.table1…
FROM staging.table2;
```

Synapse SQL 具有针对此场景的优化路径 - [CREATE TABLE AS SELECT](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas)。 CTAS 语句是一种基于批处理的操作，通过使用所有可用的计算基础结构来实现最低限度的记录，从而驱动高性能。 如果没有对 Synapse SQL 的这种见解，工具通常会生成一个截断和 INSERT/SELECT 语句。

```sql  
TRUNCATE TABLE staging.table1;
INSERT INTO staging.table1…
FROM staging.table2;
```

虽然这尚算不错，但此代码可以优化为 DROP TABLE 和 CTAS，以获得更高的性能。

```sql  
DROP TABLE staging.table1;
CREATE TABLE staging.table1
WITH
(
    -- Derived from the original table definition 
    DISTRIBUTION = HASH(column1),
    -- Derived from the original table definition
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT  * FROM staging.table2;
```

## <a name="next-steps"></a>后续步骤

- [下载 Azure Synapse Pathway](synapse-pathway-download.md)
- [常见问题](pathway-faq.md)

