---
title: 使用 Transact-SQL 编辑器编辑和执行脚本
description: 熟悉 Transact-SQL 编辑器。 了解如何打开编辑器，查看其窗格显示的信息，以及如何查看其功能的资源。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.SQLEDITOR
- sql.data.tools.sqleditor
ms.assetid: fa78e2cf-3c64-49f5-93cc-a3d50b1e7d05
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 4c9bb76ddd3ee5f5c0828f5c9ef181f98e26f94d
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067069"
---
# <a name="use-transact-sql-editor-to-edit-and-execute-scripts"></a>使用 Transact-SQL 编辑器编辑和执行脚本

在使用脚本时，Transact\-SQL 编辑器为你提供了丰富的编辑和调试体验。 在使用“查看代码”上下文菜单在连接的数据库或项目中打开某一数据库实体时，将调用该编辑器。 在从 SQL Server 对象资源管理器使用“新建查询”上下文菜单或者向数据库项目添加新的脚本对象时，该编辑器也自动打开。  
  
如果没有连接到某一数据库，但想要对其执行查询，则也可以使用“SQL” -> “Transact\-SQL 编辑器”菜单选项中的“新建查询连接”对话框以连接到某一数据库，然后启动 Transact\-SQL 编辑器。  
  
Transact\-SQL 编辑器包含一个主 T-SQL 窗格，在其中可以编写和编辑 Transact\-SQL 脚本。 该编辑器支持 IntelliSense 以及语法的颜色编码，以便提高复杂语句的可读性。 它还支持查找和替换、大量标注、自定义字体和颜色以及行编号。 您还可以更改编辑器中的脚本将对其执行的数据库。 有关详细信息，请参阅[操作说明：克隆现有数据库](../ssdt/how-to-clone-an-existing-database.md)。 “结果”窗格将在网格或文本中显示查询结果。 您还可以将查询结果重定向到某一文件。 “消息”窗格显示在运行脚本时返回的错误、警告和信息性消息。 在启用客户端统计信息时，“统计信息”窗格将显示与查询执行有关的信息并且这些信息按类别进行分组。 “执行计划”窗格显示 SQL Server 选择的数据检索方法，并且显示特定语句和查询的执行系统开销。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|描述|  
|---------|---------------|  
|[如何：显示 Transact-SQL 脚本的大纲并向 Transact-SQL 脚本添加代码片段](../ssdt/how-to-outline-and-add-snippets-to-transact-sql-script.md)|使用代码段选择器可向查询中插入现成的 Transact\-SQL 代码。|  
|[如何：在脚本之间导航](../ssdt/how-to-navigate-between-scripts.md)|使用“转到定义”和“查找所有引用”在脚本之间进行导航。|  
|[如何：使用重命名和重构更改数据库对象](../ssdt/how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects.md)|跨所有脚本重命名对象和预览任何更改。|  
|[如何：执行部分查询](../ssdt/how-to-execute-a-partial-query.md)|突出显示特定的脚本段落并将其作为单个查询执行。|  
|[如何：调试存储过程](../ssdt/how-to-debug-stored-procedures.md)|通过单步执行创建和调试 Transact\-SQL 存储过程。|  
|[分析脚本性能](../ssdt/analyze-script-performance.md)|使用执行计划、客户端统计信息和代码分析确定是否可以提高您的查询、存储过程或脚本的性能。|  
  
## <a name="see-also"></a>另请参阅

[如何：使用查询创建新的数据库对象](../ssdt/how-to-create-new-database-objects-using-queries.md)