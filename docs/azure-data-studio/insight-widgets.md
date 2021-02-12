---
title: 使用见解小组件监视服务器和数据库
description: 了解如何使用 Azure Data Studio 见解小组件将监视服务器和数据库的查询转换为有深刻见解的可视化效果。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19, seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: 2b8f54b66852df0ce0300e950ba712a803a7a012
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040167"
---
# <a name="manage-servers-and-databases-with-insight-widgets-in-azure-data-studio"></a>使用 Azure Data Studio 中的见解小组件管理服务器和数据库

见解小组件使用用于监视服务器和数据库的 Transact-SQL (T-SQL) 查询，并将其转化为富有见解力的可视化效果。

见解是指添加到服务器和数据库监视仪表板的可自定义图表和图形。 查看服务器和数据库的一览式见解，然后深入了解更多详细信息，并启动所定义的管理操作。

可以生成类似于以下示例的出色服务器和数据库管理仪表板：

![数据库仪表板](media/insight-widgets/database-dashboard.png)

若要直入正题，开始创建不同类型的见解小组件，请查看以下教程：

- [生成自定义见解小组件](tutorial-build-custom-insight-sql-server.md)
- *启用内置见解小组件*
  - [启用性能监视见解](tutorial-qds-sql-server.md)
  - [启用表空间使用情况见解](tutorial-table-space-sql-server.md)

## <a name="sql-queries"></a>SQL 查询

Azure Data Studio 试图避免引入另一种语言或繁杂的用户界面，因此它尽可能多地使用 JSON 配置最少的 T-SQL。 通过使用 T-SQL 配置见解小组件，可以利用无数个现有的有用 T-SQL 查询源，这些查询可转化为富有见解力的小组件。

见解小组件由一个或两个 T-SQL 查询组成：
* *见解小组件查询* 是必需的，并且是返回小组件中所示数据的查询。
* 仅当创建见解详细信息页面时，才需要 *见解详细信息查询*。

见解小组件查询定义用于呈现计数、图表或图形的数据集。 见解详细信息查询用于在见解详细信息面板中以表格格式列出相关见解详细信息。 

Azure Data Studio 执行见解小组件查询，并将查询结果集映射到某个图表的数据集，然后呈现它。 当用户打开某个见解的详细信息时，它会执行见解详细信息查询，并在对话框的网格视图中输出结果。

基本思路是以某种方式编写 T-SQL 查询，以便将其用作计数、图表和图形小组件的数据集。 

## <a name="summary"></a>总结

T-SQL 查询及其结果集决定了见解小组件的行为。 为图表类型编写查询或为现有查询映射正确的图表类型是生成有效见解小组件的关键考虑因素。



## <a name="additional-resources"></a>其他资源
- [查询编辑器](tutorial-sql-editor.md)

