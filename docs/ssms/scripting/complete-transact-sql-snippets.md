---
title: 完成 Transact-SQL 代码段
description: Transact-SQL 代码片段是一个代码模板。 了解如何通过在其替换点插入内容并将语法元素添加到模板来自定义其使用。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], completing snippets
- snippets [Transact-SQL], completing
- Transact-SQL snippets, completing
ms.assetid: a8316a58-bb57-485e-845f-84c23360314c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a601f7fe6192a7a5fd1395cb95c2eabf6285dfc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354723"
---
# <a name="complete-transact-sql-snippets"></a>完成 Transact-SQL 代码段
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码段插入脚本后，可以编辑代码段的内容以生成完整的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
## <a name="completing-snippets"></a>完成代码段  
 将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码段添加到脚本时，插入的代码段语句具有一个或多个替换点，这些替换点会突出显示。 如果您将鼠标指针放在替换点上，会出现一个工具提示，其中包含您可以指定的语法元素的说明。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器会将代码段与周围的脚本区分开，直到您关闭源文件。 替换点将保持活动状态，直到您关闭源文件。  
  
 您还可以向代码段所插入的模板代码中添加其他语法元素。 例如，创建表代码段模板生成两个列定义；您必须添加其他列定义才能创建具有两个以上列的表格。  
  
#### <a name="completing-the-snippet-statement"></a>完成代码段语句  
  
1.  使用 Tab 键从一个替换点移到下一个替换点。 使用 Shift+Tab 移到前一个替换点。  
  
2.  按 Ctrl+空格键以调用 IntelliSense。  
  
3.  从列表中选择一个项目，或键入您选择的替换内容。  
  
## <a name="see-also"></a>另请参阅  
 [插入 Transact-SQL 代码段](./insert-transact-sql-snippets.md)   
 [插入外侧 Transact-SQL 代码片段](./insert-surround-with-transact-sql-snippets.md)  
  
