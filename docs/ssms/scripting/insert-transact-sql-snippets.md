---
title: 插入 Transact-SQL 代码段
description: 了解如何选择、插入和修改可在数据库引擎查询编辑器中编写新的 Transact-SQL 语句时将其用作起点的 Transact-SQL 代码片段。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], snippets
- Transact-SQL snippets, code
- snippets [Transact-SQL], how to insert
ms.assetid: d66c96f4-2e84-4d79-9bfd-3635fdd98425
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3eca2cf0492a3676a715c94a3b195e6c509678d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271138"
---
# <a name="insert-transact-sql-snippets"></a>插入 Transact-SQL 代码段
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码段是一个模板，您可将其作为在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器中编写新 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 语句的起点。  
  
## <a name="inserting-snippets"></a>插入代码段  
 可以使用 **“插入代码段”** 菜单打开可供选择的代码段的分类列表。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码段包含替换点：建议与该点相关的语法的文本。 例如，CREATE TABLE 代码段包含元素（如表名称、列名称和列数据类型）的替换点。 插入代码段后，必须更改替换文本，以形成有效的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 有关详细信息，请参阅 [完成 Transact-SQL 代码段](./complete-transact-sql-snippets.md)。  
  
#### <a name="inserting-a-snippet-by-using-the-insert-snippet-menu"></a>通过使用“插入代码段”菜单插入代码段  
  
1.  在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口中，将光标放在要插入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码段的位置。  
  
2.  使用下列三种方法之一启动代码段选择器工具提示：  
  
    -   按 Ctrl + K、Ctrl + X。  
  
    -   在 **“编辑”** 菜单上，指向 **“IntelliSense”** ，然后单击 **“插入代码段”** 。  
  
    -   单击鼠标右键，然后从快捷菜单中选择“插入代码段”命令。  
  
3.  双击代码段，或从代码段选择器中选择代码段，然后按 Tab 或 Enter。  
  
## <a name="see-also"></a>另请参阅  
 [插入外侧 Transact-SQL 代码片段](./insert-surround-with-transact-sql-snippets.md)  
  
