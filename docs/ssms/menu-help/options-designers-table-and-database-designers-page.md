---
description: 选项（“设计器”-“表设计器和数据库设计器”页）
title: 选项（“设计器”-“表设计器和数据库设计器”页）
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Designers.Table_Designer
ms.assetid: b43f4b97-17b9-4004-a824-f77b9e145741
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 36e62c2d30e4678c8b41223003db35b13e8d15ac
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679238"
---
# <a name="options-designers---table-and-database-designers-page"></a>选项（“设计器”-“表设计器和数据库设计器”页）
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
使用此页可确定设计器的默认行为。 若要访问这些设置，请在“工具”  菜单上，单击“选项  ”，展开“设计器”  文件夹，再单击“表设计器”  。  
  
## <a name="ui-element-list"></a>UI 元素列表  
**为表设计器更新重写连接字符串的超时值**  
允许为表设计器的操作设置新的超时值。 这在表设计器影响大型表，需要更多的时间完成表修改时非常有用。  
  
**事务超时时间**  
为表设计器设置超时值。  
  
**自动生成更改脚本**  
自动创建脚本，以实现表设计器中定义的更改。  
  
**出现空主键时警告**  
如果为主键选择的字段包含 Null 值，则提供警告对话框。  
  
**检测到差异时警告**  
如果选中此框，在保存更改时将出现一个消息框，其中列出您所做的更改与其他用户所做的更改之间存在的所有冲突。  
  
**表受到影响时警告**  
提供警告对话框，列出受操作影响的表，并提示确认。  
  
**阻止保存要求重新创建表的更改**  
禁止用户进行要求重新创建表的更改。 以下操作可能要求重新创建表：  
  
-   在表中间添加一个新列  
  
-   删除列  
  
-   更改列为 Null 性  
  
-   更改列的顺序  
  
-   更改列的数据类型  
  
**默认表视图**  
选择要在设计器中查看表的方式：  
  
-   **Standard**  
  
    显示表头、所有列名、数据类型和“允许空值”设置。  
  
-   **列名**  
  
    显示列名称。  
  
-   **Key**  
  
    显示表头和主键列。  
  
-   **仅名称**  
  
    仅显示表头及其名称。  
  
-   **自定义**  
  
    允许选择要查看的列。  
  
**在新建关系图时启动“添加表”对话框**  
在打开设计器时自动打开“添加表”  对话框。  
  
