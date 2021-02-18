---
description: Visual Database Tool 设计器
title: Visual Database Tool 设计器
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- data sources [SQL Server]
- View Designer
- Visual Database Tools [SQL Server]
- Database Diagram Designer
- Query Designer [SQL Server]
- design tools [Visual Database Tools]
- Table Designer
- Visual Database Tools [SQL Server], designers
- Properties window [Visual Database Tools]
ms.assetid: bd0ca68e-6f69-42dd-bcb5-ce511673769c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 129de2adfa1b3b033551d3b41db4eac8f34891fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338772"
---
# <a name="visual-database-tool-designers"></a>Visual Database Tool 设计器
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Visual Database Tools 包含可用来处理数据源的多种设计工具。 您可以使用这些工具创建查询，设计或修改数据库结构，或者更新数据。 这些工具包括数据库关系图设计器、表设计器以及查询和视图设计器。  
  
## <a name="properties-window"></a>“属性”窗口  
属性窗口并不是 Visual Database Tools 所特有的，但是在其属性窗口中可以进行许多修改。 该窗口显示当前所选项（如表）的属性，并允许您编辑这些属性（从属性名称到列排序规则的所有属性）。 某些属性可以在属性窗口中查看，但必须使用其他工具进行修改。  
  
## <a name="database-diagram-designer"></a>数据库关系图设计器  
数据库关系图设计器提供了一个窗口，您可以在其中直观地创建、编辑和显示数据库中的表及关系。  
  
若要显示数据库关系图设计器，请打开现有关系图，或右键单击对象资源管理器中的“数据库”节点并从下拉菜单上选择“新建数据库关系图”。  
  
设计器打开后，就会在主菜单上显示“数据库关系图”菜单。 可通过此菜单访问设计器的特殊功能。  
  
> [!NOTE]  
> 此设计器可与 Microsoft SQL Server 数据库配合使用。  
>   
> 此版本的 Visual Database Tools 不支持 Microsoft SQL Server version 7 及更早版本。  
  
## <a name="table-designer"></a>表设计器  
表设计器是一种可视化工具，在其中可对所连接的 Microsoft SQL Server 数据库中的单个表进行设计和可视化处理。  
  
表设计器有两个窗格。 上部区域显示一个网格；网格的每一行描述一个数据库列。 对于每个数据库列，该网格都会显示其基本属性：列名、数据类型和“允许空”设置。  
  
表设计器的下部区域是“列属性”选项卡，它显示在上部区域中突出显示的任何列的其他属性。  
  
在表设计器中，还可以通过在网格区域中右键单击来访问对话框，通过这些对话框可以创建和修改表的关系、约束、索引以及键。  
  
若要显示表设计器，请打开一个现有表，或在对象资源管理器中右键单击“表”节点，再从下拉菜单中选择“添加新表”。  
  
设计器一旦打开，就会在主菜单上显示“表设计器”菜单。 可通过此菜单访问设计器的特殊功能。  
  
> [!NOTE]  
> 此设计器可与 Microsoft SQL Server 数据库配合使用。  
>   
> 此版本的 Visual Database Tools 不支持 Microsoft SQL Server version 7 及更早版本。  
  
## <a name="query-and-view-designer"></a>查询和视图设计器  
查询和视图设计器实际上是两个工作方式极其类似的工具。 它们的主要区别包括：  
  
-   视图随数据库保存，而查询则随 Visual Studio 数据库对象保存。  
  
-   查询设计器几乎可以处理任何数据源，而视图设计器只能与 SQL Server 一起使用。  
  
-   查询设计器允许设计 SELECT、INSERT、UPDATE 和 DELETE DML 语句，而视图只能包含 SELECT 语句。  
  
### <a name="view-designer"></a>视图设计器  
使用视图设计器可以对现有视图进行设计和可视化处理，或者在所连接的 Microsoft SQL Server 数据库中创建新的视图。  
  
视图设计器包含四个窗格：“关系图”窗格、“条件”窗格、“SQL”窗格和“结果”窗格。 有关上述各个窗格的详细信息，请参阅[查询和视图设计器工具 (Visual Database Tools)](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)。  
  
若要显示视图设计器，请打开一个现有视图，或在对象资源管理器中右键单击“视图”节点，再从下拉菜单中选择“添加新视图”。  
  
打开该设计器后，就会在主菜单上显示“查询设计器”菜单。 可通过此菜单访问设计器的特殊功能。  
  
> [!NOTE]  
> 此设计器可与 Microsoft SQL Server 数据库配合使用。  
>   
> 此版本的 Visual Database Tools 不支持 Microsoft SQL Server version 7 及更早版本。  
  
## <a name="see-also"></a>另请参阅  
[设计数据库关系图 (Visual Database Tools)](../../ssms/visual-db-tools/design-database-diagrams-visual-database-tools.md)  
[设计表 (Visual Database Tools)](../../ssms/visual-db-tools/design-tables-visual-database-tools.md)  
[设计查询和视图操作指南主题 (Visual Database Tools)](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
