---
description: 列属性（“常规”页）
title: 列属性（“常规”页）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.columnproperties.general.f1
ms.assetid: a745890b-994e-4c23-8028-5c83751e60c4
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cf180c58a2313f991be8ed0b89f40e53686b378e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748647"
---
# <a name="column-properties-general-page"></a>列属性（“常规”页）
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  使用此页可以查看所选列的属性。  
  
 此页上的信息为只读信息。 若要修改列，请关闭“列属性”对话框，然后在对象资源管理器中展开表和列，右键单击该列，再单击“设计”。  
  
## <a name="options"></a>选项  
 **名称**  
 列的名称。  
  
 **数据类型**  
 此列所能包含的数据类型。 如果数据类型为用户定义数据类型，则显示用户定义数据类型。 如果数据类型不是用户定义数据类型，则显示系统数据类型。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。  
  
 **系统类型**  
 此列所能包含的数据类型。 如果数据类型为系统数据类型，则显示系统数据类型。 如果数据类型为用户定义数据类型，则显示组成用户定义数据类型的系统数据类型。  
  
 **主键**  
 指示该列是否为主键。 可能的值包括 **True** 和 **False**。  
  
 **允许 Null 值**  
 指示列是否接受空值。 可能的值包括 **True** 和 **False**。  
  
 **是计算**  
 指示该列的值是否为计算表达式的结果。  
  
 **计算文本**  
 指示用于计算列文本的语句。 有关详细信息，请参阅 [Specify Computed Columns in a Table](../../relational-databases/tables/specify-computed-columns-in-a-table.md)。  
  
 **标识**  
 指示列是否为表的标识列。 可能的值包括 **True** 和 **False**。  
  
 **标识种子**  
 指示标识列的初始行值。  
  
 **标识增量**  
 “标识增量”属性指定在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为插入的行生成标识值时，在现有的最大行标识值基础上所加的值。  
  
 **默认值绑定**  
 绑定到该列的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认值。 如果未绑定默认值，此选项为空白。  
  
 **默认架构**  
 标识拥有绑定到被引用列的默认值的数据库架构。 如果未绑定默认值，此选项为空白。  
  
 **规则**  
 标识绑定到该列的数据完整性约束。 如果未绑定规则，此选项为空白。  
  
 **规则架构**  
 显示拥有绑定到被引用列的规则的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库架构。 如果未绑定规则，此选项为空白。  
  
 **长度**  
 指示该列可接受的最大字符数或字节数。  
  
 **排序规则**  
 显示该列当前的排序规则。 如果为空，则从对象继承排序规则属性。  
  
 **数值精度**  
 指定固定精度数值数据类型中的最大位数。  
  
 **小数位数**  
 指示固定精度数值数据类型中小数点后的位数。  
  
 **XML 架构命名空间**  
 通过 XML 架构定义 (XSD) 语言验证定义 XML 列的类型。  
  
 **XML 架构命名空间架构**  
 拥有 XML 架构命名空间的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构。  
  
> [!NOTE]  
>  架构一词具有多种不同的常用含义。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用架构来组织数据库对象。 其含义类似于所有权。 XML 使用架构来定义 XML 信息在一系列命名空间内的结构。 架构是一种组织相关 XML 代码的方式。  
  
 **稀疏**  
 指示列是否为稀疏列。 可能的值包括 **True** 和 **False**。 有关详细信息，请参阅 [使用稀疏列](../../relational-databases/tables/use-sparse-columns.md)。  
  
 **是列集**  
 指示列是否为列集。 可能的值包括 **True** 和 **False**。 有关详细信息，请参阅 [使用列集](../../relational-databases/tables/use-column-sets.md)。  
  
 **ANSI 填充状态**  
 指示 ANSI 填充是启用还是禁用。 有关详细信息，请参阅 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)。  
  
 **全文**  
 显示该列是否参与全文查询。  
  
 **统计语义**  
 指示是否为列启用了统计语义搜索。 有关详细信息，请参阅[语义搜索 (SQL Server)](../../relational-databases/search/semantic-search-sql-server.md)。  
  
 **不用于复制**  
 指示该列是否可用于复制。 可能的值包括 **True** 和 **False**。  
  
  
