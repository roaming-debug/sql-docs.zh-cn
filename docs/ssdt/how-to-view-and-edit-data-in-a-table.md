---
title: 查看和编辑表中的数据
description: 了解如何使用数据编辑器查看、编辑和删除现有表中的数据。 了解如何以脚本形式查看更改并将更改保存为脚本文件。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.QUERYRESULTS.F1
- sql.data.tools.queryresults.executequerydeletingrow
ms.assetid: bb67ce83-a87a-4e14-84cd-9a5930fe74c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: ebe23b534590bfb45c4a0bffce78d2139ef8912c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017987"
---
# <a name="how-to-view-and-edit-data-in-a-table"></a>如何：查看和编辑表中的数据

您可以使用直观的数据编辑器查看、编辑和删除现有表中的数据。  
  
> [!WARNING]  
> 下面的过程使用了前面[连接的数据库开发](../ssdt/connected-database-development.md)一节介绍的过程中创建的实体。  
  
### <a name="to-edit-data-in-a-table-visually-using-the-data-editor"></a>使用数据编辑器以直观的方式编辑表中的数据  
  
1.  右键单击“SQL Server 对象资源管理器”中的“Products”表，然后选择“查看数据”。  
  
2.  数据编辑器随即启动。 请注意我们在前面的过程中添加到该表的行。  
  
3.  右键单击“SQL Server 对象资源管理器”中的“Fruits”表，然后选择“查看数据”。  
  
4.  在数据编辑器中，为 Id 键入 1，为 Perishable 键入 True，然后按 ENTER 键或 TAB 键，将焦点从新行移出以便将其提交到数据库。  
  
5.  重复上述步骤以便向表中输入 2、False 和 3、False。  
  
    请注意，在您编辑某一行时，始终可以通过按 ESC 键还原对某一单元的更改。  
  
6.  你可以通过单击工具栏上的“脚本”按钮，将所做的编辑作为脚本查看。 或者，你可以使用“将脚本保存到文件”按钮将所做编辑保存到某个 .sql 脚本中以便在以后执行。  
  
7.  右键单击“SQL Server 对象资源管理器”中的“Trade”数据库，然后选择“新建查询”。 在编辑器中，键入 `select * from dbo.PerishableFruits` 并按下“执行查询”按钮以便返回 `PerishableFruits` 视图表示的数据。  
  
