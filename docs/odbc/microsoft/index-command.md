---
description: INDEX 命令
title: INDEX 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- index command [ODBC]
ms.assetid: 694e8cf5-2f69-4001-9c1e-b735a4da3aff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ac225132e2d89f1edd4baab012b2383cd508588
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183397"
---
# <a name="index-command"></a>INDEX 命令
创建索引文件以按逻辑顺序显示和访问表记录。  
  
## <a name="syntax"></a>语法  
  
```  
  
INDEX ON eExpression TO IDXFileName | TAG TagName [OF CDXFileName]  
   [FOR lExpression]  
   [COMPACT]  
   [ASCENDING | DESCENDING]  
   [UNIQUE | CANDIDATE]  
   [ADDITIVE]  
```  
  
## <a name="arguments"></a>参数  
 *eExpression*  
 指定一个索引表达式，该表达式可包含当前表中的一个或多个字段的名称。 将在表中每个记录的索引文件中创建一个基于索引表达式的索引键。 Visual FoxPro 使用这些键显示和访问表中的记录。  
  
> [!NOTE]  
>  尽管不建议这样做， *eExpression* 也可以是内存变量、数组元素或来自其他工作区中的表的字段或字段表达式。 备注字段不能单独用于索引文件表达式中;它们必须与其他字符表达式结合。 如果访问的索引包含的变量或字段不再存在或无法找到，则 Visual FoxPro 会生成错误消息。  
  
 如果尝试生成的索引的键长度不同，则会用空格填充该键。 Visual FoxPro 中不支持可变长度索引键。  
  
 可以创建长度为零的索引键。 例如，当索引表达式为空备注型字段的子字符串时，将创建零长度索引键。 零长度索引键生成错误消息。 当 Visual FoxPro 创建索引时，它将计算表的第一条记录中的字段。 如果字段为空，则可能需要在第一条记录的字段中输入一些临时数据，以防止出现0长度的索引键。  
  
 到 *IDXFileName*  
 创建 idx 索引文件。 为索引文件提供默认扩展名 idx。  
  
 标记 *TagName*[of *CDXFileName*]  
 创建复合索引文件。 复合索引文件是单个索引文件，其中包含任意数量的单独标记 (索引项) 。 每个标记都由其唯一标记名称标识。 标记名称必须以字母或下划线开头，可包含最多10个字母、数字或下划线的任意组合。 复合索引文件中的标记数量仅受可用内存和磁盘空间的限制。  
  
 多项复合索引文件始终为 compact。 创建复合索引文件时，无需包含 COMPACT。 复合索引文件的名称为 cdx 扩展名。  
  
 可创建两种类型的复合索引文件：结构化和 nonstructural。  
  
 **结构化复合索引文件** 可以通过排除可选的 *CDXFileName* 子句来创建具有标记 *TagName* 的结构化复合索引文件。 结构化复合索引文件始终与表具有相同的基名称，并在打开表时自动打开。  
  
 **Nonstructural 复合索引文件** 可以通过在标记 *TagName* 后包含 *CDXFileName* ，来创建 nonstructural 复合索引文件。 与结构化复合索引文件不同，nonstructural 复合索引文件必须使用索引子句显式打开。  
  
 如果已创建并打开复合索引文件，则使用标记 *TagName* 发出索引会将标记添加到复合索引文件中。  
  
 对于 *lExpression*  
 指定一个条件，该条件仅满足筛选器表达式 *lExpression* 的记录可用于显示和访问;索引键是在索引文件中创建的，只是与筛选器表达式匹配的那些记录。  
  
 Visual FoxPro 雕像技术优化索引 .。。对于 *lExpression* 命令，如果 *lExpression* 是可优化的表达式。 为了获得最佳性能，请在 FOR 子句中使用可优化表达式。  
  
 COMPACT  
 创建一个 compact idx 文件。  
  
 竖条  
 指定 cdx 文件的升序排序。 默认情况下，以升序创建 cdx 标记。  (可以将升序作为索引文件顺序的提醒包括在内。通过包含降序 ) 可以按相反顺序对表进行索引。  
  
 排序  
 指定 cdx 文件的降序顺序。 创建 idx 索引文件时，不能包括降序。  
  
 UNIQUE  
 指定仅在 idx 文件或 cdx 标记中包含带有特定索引键值的第一条记录。 UNIQUE 可用于阻止显示或访问重复记录。 将从索引文件中排除添加了重复索引键的所有记录。 在发出索引或重新索引之前，使用 INDEX 的 UNIQUE 选项与执行 SET UNIQUE 选项完全相同。  
  
 当唯一索引或索引标记处于活动状态，并且以更改其索引键的方式更改重复记录时，将更新索引或索引标记。 但是，在使用索引重新索引文件之前，无法访问或显示下一个具有原始索引键的重复记录。  
  
 者  
 创建候选结构索引标记。 只有在创建结构索引标记时才可以包含候选关键字;否则，Visual FoxPro 会生成错误消息。  
  
 候选索引标记可防止在索引表达式 *eExpression* 中指定的字段或字段组合中出现重复值。 *候选* 字词是指索引的类型;由于候选索引会阻止重复的值，因此它们可以作为 "候选项" 作为主要索引。  
  
 如果为已包含重复值的字段或字段组合创建候选索引标记，Visual FoxPro 将生成错误。  
  
 微  
 保留打开任何以前打开的索引文件。 如果在为具有索引的表创建索引文件或文件时省略加法子句，则除了结构化复合索引) 关闭外，所有以前打开的索引文件 (。  
  
## <a name="remarks"></a>备注  
 具有索引文件的表中的记录以索引表达式指定的顺序显示和访问。 索引文件不会更改表中记录的物理顺序。  
  
## <a name="index-types"></a>索引类型  
 利用 Visual FoxPro，你可以创建两种类型的索引文件：  
  
-   包含多个称为标记的索引条目的 cdx 索引文件  
  
-   idx 包含一个索引条目的索引文件  
  
 你还可以创建结构化复合索引文件，该文件将自动使用表打开。  
  
> [!NOTE]  
>  由于结构化复合索引文件是在打开表时自动打开的，因此它们是首选的索引类型。  
  
 若要创建 compact. idx 索引文件，请包含 COMPACT。 复合索引文件始终为 compact。  
  
## <a name="index-order-and-updating"></a>索引顺序和更新  
 只有一个索引文件 (主索引文件) 或标记 (主标记) 控制显示或访问表的顺序。 某些命令 (查找，例如) 使用主索引文件或标记来搜索记录。 但是，当对表进行更改时，将更新所有打开的 idx 和. cdx 索引文件。  
  
## <a name="user-defined-functions"></a>用户定义函数  
 尽管索引表达式可以包含用户定义函数，但不应在索引表达式中使用用户定义函数。 索引表达式中的用户定义函数增加了创建或更新索引所用的时间。 此外，当用户定义函数用于索引表达式时，可能不会发生索引更新。  
  
 如果在索引表达式中使用用户定义函数，则 Visual FoxPro 必须能够找到用户定义的函数。 当 Visual FoxPro 创建索引时，索引表达式将保存在索引文件中，但索引表达式中只包含对用户定义函数的引用。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE-SQL 命令](../../odbc/microsoft/alter-table-sql-command.md)   
 [删除标记命令](../../odbc/microsoft/delete-tag-command.md)   
 [设置整理命令](../../odbc/microsoft/set-collate-command.md)   
 [SET UNIQUE 命令](../../odbc/microsoft/set-unique-command.md)
