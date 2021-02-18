---
description: 设计数据库关系图 (Visual Database Tools)
title: 设计数据库关系图
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65536
- vdt.DatabaseDesigner
helpviewer_keywords:
- Database Diagram Designer
- Visual Database Tools [SQL Server], database diagrams
- database diagrams [SQL Server], designing
- diagrams [SQL Server], designing
ms.assetid: 6d2c14e1-3d73-4d10-ae5b-7f2b5d6c4ea8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 335fd01a3eb1eeb36dc9c025a19b7ae8c1e87302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346631"
---
# <a name="design-database-diagrams-visual-database-tools"></a>设计数据库关系图 (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
数据库设计器是一种可视化工具，它允许您对所连接的数据库进行设计和可视化处理。 设计数据库时，您可以使用数据库设计器创建、编辑或删除表、列、键、索引、关系和约束。 为使数据库可视化，您可创建一个或多个关系图，以显示数据库中的部分或全部表、列、键和关系。  
  
![阐释表关系的数据库关系图](../../ssms/visual-db-tools/media/dv3w7c1.gif "阐释表关系的数据库关系图")  
  
对于任何数据库，您都可以创建任意数目的数据库关系图；每个数据库表都可以出现在任意数目的关系图中。 这样，便可以创建不同的关系图使数据库的不同部分可视化，或强调设计的不同方面。 例如，可以创建一个大型关系图来显示所有表和列，并创建一个较小的关系图来显示所有表但不显示列。  
  
所创建的每个数据库关系图都存储在关联数据库中。  
  
## <a name="tables-and-columns-in-a-database-diagram"></a>数据库关系图中的表和列  
在数据库关系图中，每个表都可以带有三种不同的功能：标题栏、行选择器和一组属性列。  
  
**标题栏** 标题栏显示表的名称  
  
如果修改了某个表，但尚未保存该表，则表名末尾将显示一个星号 (*)，表示未保存更改。 有关保存修改后的表和关系图的详细信息，请参阅 [使用数据库关系图 (Visual Database Tools)](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
  
**行选择器** 可以通过单击行选择器来选择表中的数据库列。 如何该列是表的主键，则行选择器将显示一个键符号。 有关主键的信息，请参阅[使用键](../../relational-databases/tables/primary-and-foreign-key-constraints.md)。  
  
**属性列** 属性列组仅在表的某些视图中可见。 您可以在五个不同视图中的任何一个视图中查看表，以帮助您管理关系图的大小和布局。  
  
有关表视图的详细信息，请参阅[自定义关系图中显示的信息量 (Visual Database Tools)](../../ssms/visual-db-tools/customize-the-amount-of-information-displayed-in-diagrams-visual-database-tools.md)。  
  
## <a name="relationships-in-a-database-diagram"></a>数据库关系图中的关系  
在数据库关系图中，每个关系都可以带有三种不同的功能：终结点、线型和相关表。  
  
**终结点** 线的终结点表示关系是一对一还是一对多关系。 如果某个关系在一个终结点处有键，在另一个终结点处有无穷符号，则该关系是一对多关系。 如果某个关系在每个终结点处都有键，则该关系是一对一关系。  
  
**线型** 线本身（非其终结点）表示当向外键表添加新数据时，数据库管理系统 (DBMS) 是否强制关系的引用完整性。 如果为实线，则在外键表中添加或修改行时，DBMS 将强制关系的引用完整性。 如果为点线，则在外键表中添加或修改行时，DBMS 不强制关系的引用完整性。  
  
**相关表** 关系线表示两个表之间存在外键关系。 对于一对多关系，外键表是靠近线的无穷符号的那个表。 如果线的两个终结点连接到同一个表，则该关系是自反关系。 有关详细信息，请参阅[绘制自反关系 (Visual Database Tools)](../../ssms/visual-db-tools/draw-reflexive-relationships-visual-database-tools.md)。  
  
## <a name="in-this-section"></a>本节内容  
[了解数据库关系图所有权 (Visual Database Tools)](../../ssms/visual-db-tools/understand-database-diagram-ownership-visual-database-tools.md)  
  
[在数据库关系图设计器中导航 (Visual Database Tools)](../../ssms/visual-db-tools/navigate-in-database-diagram-designer-visual-database-tools.md)  
  
[设置数据库关系图设计器 (Visual Database Tools)](../../ssms/visual-db-tools/set-up-database-diagram-designer-visual-database-tools.md)  
  
[从以前的版本升级数据库关系图 (Visual Database Tools)](../../ssms/visual-db-tools/upgrade-database-diagrams-from-previous-editions-visual-database-tools.md)  
  
[打开数据库关系图设计器 (Visual Database Tools)](../../ssms/visual-db-tools/open-database-diagram-designer-visual-database-tools.md)  
  
## <a name="see-also"></a>另请参阅  
[使用数据库关系图 (Visual Database Tools)](../../ssms/visual-db-tools/work-with-database-diagrams-visual-database-tools.md)  
[使用数据库关系图中的表 (Visual Database Tools)](../../ssms/visual-db-tools/work-with-tables-in-database-diagram-visual-database-tools.md)  
[使用关系图布局 (Visual Database Tools)](../../ssms/visual-db-tools/work-with-diagram-layout-visual-database-tools.md)  
