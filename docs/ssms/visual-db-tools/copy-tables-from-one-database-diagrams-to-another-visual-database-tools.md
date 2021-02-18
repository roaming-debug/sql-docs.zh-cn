---
description: 将表从一个数据库关系图复制到另一个数据库关系图 (Visual Database Tools)
title: 将表从一个数据库关系图复制到另一个数据库关系图
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- copying tables
- duplicating tables
ms.assetid: 155a4f09-9321-4887-a7d4-aa2ce6b51277
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: eb8e231f06fe133b76be26098af07c428eeb638e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344115"
---
# <a name="copy-tables-from-one-database-diagrams-to-another-visual-database-tools"></a>将表从一个数据库关系图复制到另一个数据库关系图 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
可以将表从一个数据库关系图复制到同一数据库的另一个数据库关系图。  
  
如果将一个表从一个数据库关系图复制到另一个关系图，则会在第二个关系图中添加对该表的引用。 该表并未在您的数据库中重复。 例如，如果将 `authors` 表从一个数据库关系图复制到另一个关系图，则每个关系图都引用数据库中的同一 `authors` 表。  
  
### <a name="to-copy-a-table-from-another-database-diagram"></a>从另一个数据库关系图复制表  
  
1.  确保已连接到要复制其中的表的数据库。  
  
2.  打开源数据库关系图和目标数据库关系图，并在源关系图中选择要复制到目标关系图的表。  
  
3.  单击工具栏上的“复制”按钮。 此操作会将所选表的定义放置到剪贴板上。  
  
4.  切换到目标关系图。 此关系图必须与源关系图位于同一数据库中。  
  
5.  单击工具栏上的“粘贴”按钮。 剪贴板内容将出现在新的位置，而且在单击其他位置之前保持突出显示状态。 如果选定的表与目标关系图中的其他表之间存在关系，则将自动绘制关系线。  
  
当在任一关系图中编辑该表时，所做更改将同时反映在这两个关系图中。 同样，在任一关系图中保存表之后，在任一关系图中都不再将该表视为“已修改”。  
  
## <a name="see-also"></a>另请参阅  
[使用数据库关系图 (Visual Database Tools)](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
[向关系图中添加表 (Visual Database Tools)](../../ssms/visual-db-tools/add-tables-to-diagrams-visual-database-tools.md)  
  
