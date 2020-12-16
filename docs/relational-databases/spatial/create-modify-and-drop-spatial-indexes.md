---
description: 创建、修改和删除空间索引
title: 创建、修改和删除空间索引 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], creating
- spatial indexes [SQL Server], dropping
- spatial indexes [SQL Server], creating
- indexes [SQL Server], dropping
- indexes [SQL Server], modifying
- spatial indexes [SQL Server], modifying
ms.assetid: 00c1b927-8ec5-44cf-87c2-c8de59745735
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 03187f310687792ebb07d4fd3d67e7a58ce83323
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473178"
---
# <a name="create-modify-and-drop-spatial-indexes"></a>创建、修改和删除空间索引
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  空间索引可以更高效地对数据类型为“geometry”或“geography”的列（空间数据列）执行特定操作。 可对空间数据列指定多个空间索引。 这非常有用，例如，对单一列中的不同分割参数建立索引时，就是如此。  
  
 创建空间索引时有许多限制。 有关详细信息，请参阅本主题中的 [对空间索引的限制](#restrictions) 。  
  
> [!NOTE]  
>  有关空间索引与分区和文件组的关系的信息，请参阅 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)中的“备注”部分。  
  
##  <a name="creating-modifying-and-dropping-spatial-indexes"></a><a name="creating"></a> 创建、修改和删除空间索引  
  
###  <a name="to-create-a-spatial-index"></a><a name="create"></a> 创建空间索引  
 **使用 Transact-SQL 创建空间索引**  
 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)  
  
 **在 Management Studio 中使用“新建索引”对话框创建空间索引**  
 ##### <a name="to-create-a-spatial-index-in-management-studio"></a>在 Management Studio 中创建空间索引  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，展开包含具有指定索引的表的数据库，再展开 **“表”** 。  
  
3.  展开要为其创建索引的表。  
  
4.  右键单击“索引”，再选择“新建索引”。  
  
5.  在 **“索引名称”** 字段中，输入索引的名称。  
  
6.  在“索引类型”下拉列表中，选择“空间”。  
  
7.  若要指定想为其创建索引的空间数据列，请单击 **“添加”** 。  
  
8.  在“从 \<table name> 中选择列”对话框中，通过选中相应的复选框，选择类型为 geometry 或 geography 的列 。 然后，任何其他空间数据列将变为不可编辑状态。 如果要选择其他空间数据列，必须首先清除当前选定的列。 完成后，单击 **“确定”** 。  
  
9. 请在 **“索引键列”** 网格中验证您的列选择。  
  
10. 在 **“索引属性”** 对话框的 **“选择页”** 窗格中，单击 **“空间”** 。  
  
11. 在 **“空间”** 页上，指定要用于索引的空间属性的值。  
  
     在对类型为“geometry”的列创建索引时，必须指定范围框的（X-min、Y-min）和（X-max、 Y-max）坐标。        对于“geography”类型列的索引，当你指定“地理网格”分割方案后，边界框字段变为只读状态，因为地理网格分割不使用边界框。  
  
     您还可以指定任意级别的分割方案的 **“每个对象的单元数”** 字段和网格密度的非默认值。 对于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ，每个对象的默认单元数为 16；对于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或更高版本，则为 8。对于 **，默认网格密度为** “中” [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]。  
  
     在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，可以为分割方案选择 GEOMETRY_AUTO_GRID 或 GEOGRAPHY_AUTO_GRID。 选择 GEOMETRY_AUTO_GRID 或 GEOGRAPHY_AUTO_GRID 时，禁用级别 1、级别 2、级别 3 和级别 4 网格密度选项。  
  
     有关这些属性的详细信息，请参阅 [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md)。  
  
12. 单击“确定”。  
  
> [!NOTE]  
>  若要对同一空间数据列或另一个空间数据列再创建一个空间索引，请重复上述步骤。  
  
  
 **在 Management Studio 中使用表设计器创建空间索引**  
 ##### <a name="to-create-a-spatial-index-in-table-designer"></a>在表设计器中创建空间索引  
  
1.  在对象资源管理器中，右键单击要为其创建空间索引的表，然后单击“设计”。  
  
     此时，将在表设计器中打开该表。  
  
2.  为索引选择 **geometry** 列或 **geography** 列。  
  
3.  在 **表设计器** 菜单上，单击 **“空间索引”** 。  
  
4.  在 **“空间索引”** 对话框中，单击 **“添加”** 。  
  
5.  在 **“所选空间索引”** 列表中选择新的索引，然后在右侧的网格中设置空间索引的属性。 有关属性的信息，请参阅[空间索引对话框 (Visual Database Tools)](../../ssms/visual-db-tools/spatial-indexes-dialog-box-visual-database-tools.md)。  
  
  
###  <a name="to-alter-a-spatial-index"></a><a name="alter"></a> 更改空间索引  
  
-   [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)  
  
    > [!IMPORTANT]  
    >  若要更改特定于某个空间索引的选项（例如 BOUNDING_BOX 或 GRID），您可以使用 CREATE SPATIAL INDEX 语句指定 DROP_EXISTING = ON，或删除该空间索引并创建一个新的空间索引。 有关示例，请参阅 [CREATE SPATIAL INDEX (Transact-SQL)](../../t-sql/statements/create-spatial-index-transact-sql.md)中的“备注”部分。  
  
-   [修改索引](../../relational-databases/indexes/modify-an-index.md)  
  
-   [将现有索引移动到其他文件组中](../../relational-databases/indexes/move-an-existing-index-to-a-different-filegroup.md)  
  
  
###  <a name="to-drop-a-spatial-index"></a><a name="drop"></a> 删除空间索引  
 **使用 Transact-SQL 删除空间索引**  
 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)  
  
 **使用 Management Studio 删除索引**  
 [删除索引](../../relational-databases/indexes/delete-an-index.md)  
  
 **在 Management Studio 中使用表设计器删除空间索引**  
 ##### <a name="to-drop-a-spatial-index-in-table-designer"></a>在表设计器中删除空间索引  
  
1.  在对象资源管理器中，右键单击具有要删除的空间索引的表，再单击“设计”。  
  
     此时，将在表设计器中打开该表。  
  
2.  在 **表设计器** 菜单上，单击 **“空间索引”** 。  
  
     **“空间索引”** 对话框随即打开。  
  
3.  在 **“所选空间索引”** 列中单击要删除的索引。  
  
4.  单击 **“删除”** 。  
  
  
##  <a name="restrictions-on-spatial-indexes"></a><a name="restrictions"></a> 对空间索引的限制  
 只能对类型为 **geometry** 或 **geography** 的列创建空间索引。  
  
### <a name="table-and-view-restrictions"></a>针对表和视图的限制  
 只能对具有主键的表定义空间索引。 表中主键列的最大数目为 15。  
  
 索引键记录的最大大小为 895 字节。 超过此大小会引发错误。  
  
> [!NOTE]  
>  对表定义空间索引时，不能更改主键元数据。  
  
 不能对索引视图指定空间索引。  
  
### <a name="multiple-spatial-index-restrictions"></a>多空间索引限制  
 最多可对支持的表中的任何空间数据列创建 249 个空间索引。 对同一空间数据列创建多个空间索引可能很有用，例如，在要对单个列中的不同分割参数建立索引时。  
  
 一次只能创建一个空间索引。  
  
### <a name="spatial-indexes-and-process-parallelism"></a>空间索引和处理并行度  
 索引的生成过程可以利用可用的处理并行度。  
  
### <a name="version-restrictions"></a>版本限制  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中引入的空间分割方案不能复制到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]中。 当要求向后与 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 数据库兼容时，您必须将 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 空间分割方案用于空间索引。  
  
  
## <a name="see-also"></a>另请参阅  
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)  
  
