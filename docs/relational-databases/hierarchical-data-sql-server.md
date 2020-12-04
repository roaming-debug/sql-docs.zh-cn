---
description: 层次结构数据 (SQL Server)
title: 层次结构数据 (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 10/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [SQL Server], tables to support
- hierarchyid [Database Engine], concepts
- hierarchical tables [Database Engine]
- SqlHierarchyId
- hierarchyid [Database Engine]
- hierarchical queries [SQL Server], using hierarchyid data type
ms.assetid: 19aefa9a-fbc2-4b22-92cf-67b8bb01671c
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 994adada7ecef047967b07d03cd2a9a129c8f227
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91869051"
---
# <a name="hierarchical-data-sql-server"></a>层次结构数据 (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

  内置的 hierarchyid 数据类型使存储和查询层次结构数据变得更为容易。 针对表示树（最常见的层次结构数据类型）对 **hierarchyid** 进行了优化。  
  
 层次结构数据定义为一组通过层次结构关系互相关联的数据项。 在层次结构关系中，一个数据项是另一个项的父级。 通常存储在数据库中的层次结构数据示例包括以下内容：  
  
-   组织结构  
  
-   文件系统  
  
-   项目中的一组任务  
  
-   语言术语分类  
  
-   网页间链接图  
  
 使用 [hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md) 作为数据类型来创建具有层次结构的表，或描述存储在另一个位置的数据层次结构。 在 [中使用](../t-sql/data-types/hierarchyid-data-type-method-reference.md) hierarchyid 函数 [!INCLUDE[tsql](../includes/tsql-md.md)] 查询和管理层次结构数据。  
  
##  <a name="key-properties-of-hierarchyid"></a><a name="keyprops"></a> hierarchyid 的关键属性  
 **hierarchyid** 数据类型的值表示树层次结构中的位置。 **hierarchyid** 的值具有以下属性：  
  
-   非常紧凑  
  
     在具有 *n* 个节点的树中，表示一个节点所需的平均位数取决于平均端数（节点的平均子级数）。 端数较小时 (0-7)，大小约为 6\*logA *n* 位，其中 A 是平均端数。 对于平均端数为 6 级、包含 100,000 个人的组织层次结构，一个节点大约占 38 位。 存储时，此值向上舍入为 40 位，即 5 字节。  
  
-   按深度优先顺序进行比较  
  
     给定两个 **hierarchyid** 值 **a** 和 **b**，**a<b** 表示在对树进行深度优先遍历时，先找到 a，后找到 b。 **hierarchyid** 数据类型的索引按深度优先顺序排序，在深度优先遍历中相邻的节点的存储位置也相邻。 例如，一条记录的子级的存储位置与该记录的存储位置是相邻的。  
  
-   支持任意插入和删除  
  
     使用 [GetDescendant](../t-sql/data-types/getdescendant-database-engine.md) 方法，始终可以在任意给定节点的右侧、左侧或任意两个同级节点之间生成同级节点。 在层次结构中插入或删除任意数目的节点时，该比较属性保持不变。 大多数插入和删除操作都保留了紧凑性属性。 但是，对于在两个节点之间执行的插入操作，所产生的 hierarchyid 值的表示形式在紧凑性方面将稍微降低。  
  
  
##  <a name="limitations-of-hierarchyid"></a><a name="limits"></a> hierarchyid 的局限性  
 该 **hierarchyid** 数据类型具有以下局限性：  
  
-   类型为 **hierarchyid** 的列不会自动表示树。 由应用程序来生成和分配 **hierarchyid** 值，使行与行之间的所需关系反映在这些值中。 某些应用程序可能具有 **hierarchyid** 类型的列，该列指示在另一个表中定义的层次结构中的位置。  
  
-   由应用程序来管理生成和分配 **hierarchyid** 值时的并发情况。 不能保证列中的 **hierarchyid** 值是唯一的，除非应用程序使用唯一键约束或应用程序自身通过自己的逻辑来强制实现唯一性。  
  
-   由 **hierarchyid** 值表示的层次结构关系不是像外键关系那样强制实现的。 可能会出现下面这种层次结构关系而且有时这种关系是合理的：A 具有子级 B，然后删除了 A，导致 B 与一条不存在的记录之间存在关系。 如果这种行为不可接受，应用程序在删除父级之前必须先查询其是否有后代。  
  
  
##  <a name="when-to-use-alternatives-to-hierarchyid"></a><a name="alternatives"></a> 何时使用 hierarchyid 的替代项  
 用于表示层次结构数据的两个 **hierarchyid** 替代项为：  
  
-   父/子  
  
-   XML  
  
 **hierarchyid** 通常优于这些替代项。 但是，在下面详述的特定情况下，使用这些替代项可能更好。  
  
### <a name="parentchild"></a>父/子  
 使用父/子方法时，每一行都包含对父级的引用。 下表定义了一个用于在父/子关系中包含父行和子行的典型表：  
  
```sql
USE AdventureWorks2012 ;  
GO  
  
CREATE TABLE ParentChildOrg  
   (  
    BusinessEntityID int PRIMARY KEY,  
    ManagerId int REFERENCES ParentChildOrg(BusinessEntityID),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
```  
  
 针对一些常见操作比较父/子与 **hierarchyid**  
  
-   使用 **hierarchyid** 进行子树查询时速度明显加快。  
  
-   使用 **hierarchyid** 进行直接后代查询时速度稍慢。  
  
-   使用 **hierarchyid** 移动非叶节点时速度明显减慢。  
  
-   使用 **hierarchyid** 插入非叶节点和插入或移动叶节点具有相同的复杂度。  
  
 当存在以下情况时，使用父/子可能更好：  
  
-   键的大小非常重要。 在节点数相同的情况下， **hierarchyid** 值等于或大于整型系列（**smallint**、 **int**、 **bigint**）的值。 这只是在很少情况下使用父/子的一个原因，因为 **hierarchyid** 在 I/O 局部实用性和 CPU 复杂性方面明显优于使用父/子结构时所需的公用表表达式。  
  
-   很少跨层次结构的不同部分执行查询。 也就是说，通常仅对层次结构中的单个点进行查询。 在这些情况下，存储在一起并不重要。 例如，如果组织表仅用于为各个雇员处理工资单，则使用父/子更好。  
  
-   非叶子树移动频繁并且性能非常重要。 在父/子表示形式中，更改层次结构中行的位置将影响单个行。 使用 **hierarchyid** 时，更改行的位置将影响 *n* 行，其中 *n* 是要移动的子树中的节点数。  
  
     如果非叶子树移动频繁并且性能非常重要，但多数移动操作都是在比较明确的层次结构级别上进行的，请考虑将较高和较低的级别拆分成两个层次结构。 这样，所有的移动操作都是移到较高层次结构的叶级。 例如，假设有一个由服务承载的网站的层次结构。 各网站包含许多以分层方式排列的页面。 承载的网站可能移动到网站层次结构中的其他位置，但是从属的页面很少会重新排列。 这种情况可表示如下：  
  
    ```sql
    CREATE TABLE HostedSites   
       (  
        SiteId hierarchyid, PageId hierarchyid  
       ) ;  
    GO  
    ```  
  
  
### <a name="xml"></a>XML  
 XML 文档是一个树，因此单个 XML 数据类型实例可以表示一个完整的层次结构。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，创建 XML 索引时，会内部使用 hierarchyid 值以表示层次结构中的位置。  
  
 当满足以下所有条件时，使用 XML 数据类型会更好：  
  
-   始终存储并检索整个层次结构。  
  
-   应用程序以 XML 格式使用数据。  
  
-   谓词搜索有极大的局限性，并且性能不是特别重要。  
  
 例如，如果应用程序跟踪多个组织，始终存储并检索整个组织层次结构，并且不在单个组织内部进行查询，则使用如下形式的表可能较为合适：  
  
```sql
CREATE TABLE XMLOrg   
    (  
    Orgid int,  
    Orgdata xml  
    ) ;  
GO  
```  
  
  
##  <a name="indexing-strategies-for-hierarchical-data"></a><a name="indexing"></a> 层次结构数据的索引策略  
 用于对层次结构数据进行索引的策略有两种：  
  
-   **深度优先**  
  
     深度优先索引在子树中相邻位置存储行。 例如，一位经理管理的所有雇员都存储在其经理的记录附近。  
  
     在深度优先索引中，一个节点的子树中的所有节点存储在一起。 因此，深度优先索引能高效地响应有关子树的查询，“查找此文件夹及其子文件夹中的所有文件”就是这样的一个示例。  
  
-   **广度优先**  
  
     广度优先将层次结构中每个级别的各行存储在一起。 例如，同一经理直属的各雇员的记录存储在相邻位置。  
  
     在广度优先索引中，一个节点的所有直属子级存储在一起。 因此，广度优先索引在响应有关直属子级的查询方面效率很高，“查找此经理直属的所有雇员”就属于这类查询。  
  
 采用深度优先、广度优先还是结合使用这两种索引，以及将哪一种设为聚集键（如果有），取决于上述两种查询类型的相对重要性以及 SELECT 与DML 操作的相对重要性。 有关索引策略的详细示例，请参阅 [Tutorial: Using the hierarchyid Data Type](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)。  
  
  
### <a name="creating-indexes"></a>创建索引  
 GetLevel() 方法可用于创建广度优先排序。 在下例中，既创建了广度优先索引，又创建了深度优先索引：  
  
```sql
USE AdventureWorks2012 ;   -- wmimof
GO  
  
CREATE TABLE Organization  
   (  
    BusinessEntityID hierarchyid,  
    OrgLevel as BusinessEntityID.GetLevel(),   
    EmployeeName nvarchar(50) NOT NULL  
   ) ;  
GO  
  
CREATE CLUSTERED INDEX Org_Breadth_First   
    ON Organization(OrgLevel,BusinessEntityID) ;  
GO  
  
CREATE UNIQUE INDEX Org_Depth_First   
    ON Organization(BusinessEntityID) ;  
GO  
```  
  
  
## <a name="examples"></a>示例  
  
### <a name="simple-example"></a>简单示例  
 以下示例特意进行了简化，以帮助您入门。 首先创建一个表以保存一些地理数据。  
  
```sql
CREATE TABLE SimpleDemo  
(
    Level hierarchyid NOT NULL,  
    Location nvarchar(30) NOT NULL,  
    LocationType nvarchar(9) NULL
);
```  
  
 现在插入一些洲、国家/地区、州和城市的数据。  
  
```sql
INSERT SimpleDemo  
    VALUES   
('/1/', 'Europe', 'Continent'),  
('/2/', 'South America', 'Continent'),  
('/1/1/', 'France', 'Country'),  
('/1/1/1/', 'Paris', 'City'),  
('/1/2/1/', 'Madrid', 'City'),  
('/1/2/', 'Spain', 'Country'),  
('/3/', 'Antarctica', 'Continent'),  
('/2/1/', 'Brazil', 'Country'),  
('/2/1/1/', 'Brasilia', 'City'),  
('/2/1/2/', 'Bahia', 'State'),  
('/2/1/2/1/', 'Salvador', 'City'),  
('/3/1/', 'McMurdo Station', 'City');  
```  
  
 选择数据，从而添加将级别数据转换为易于理解的文本值的列。 此查询还按 **hierarchyid** 数据类型对结果排序。  
  
```sql
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], *   
    FROM SimpleDemo ORDER BY Level;  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
Converted Level  Level     Location         LocationType  
/1/              0x58      Europe           Continent  
/1/1/            0x5AC0    France           Country  
/1/1/1/          0x5AD6    Paris            City  
/1/2/            0x5B40    Spain            Country  
/1/2/1/          0x5B56    Madrid           City  
/2/              0x68      South America    Continent  
/2/1/            0x6AC0    Brazil           Country  
/2/1/1/          0x6AD6    Brasilia         City  
/2/1/2/          0x6ADA    Bahia            State  
/2/1/2/1/        0x6ADAB0  Salvador         City  
/3/              0x78      Antarctica       Continent  
/3/1/            0x7AC0    McMurdo Station  City  
```  
  
 请注意，层次结构具有有效的结构，即使其内部不一致。 Bahia 是唯一的州。 它作为城市 Brasilia 的对等方出现在层次结构中。 同样，McMurdo Station 没有父国家/地区。 用户必须确定此类型的层次结构是否适合于其用途。  
  
 添加另一行并选择结果。  
  
```sql
INSERT SimpleDemo  
    VALUES ('/1/3/1/', 'Kyoto', 'City'), ('/1/3/1/', 'London', 'City');  
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], * FROM SimpleDemo ORDER BY Level;  
```  
  
 这演示更多可能的问题。 Kyoto 可以作为级别 `/1/3/1/` 插入，即使没有父级别 `/1/3/`。 London 和 Kyoto 具有相同的 **hierarchyid** 值。 用户必须再次确定此类型的层次结构是否适合于其用途，并阻止对其用途无效的值。  
  
 此外，此表未使用层次结构顶层 `'/'`。 该层被省略，因为没有所有州的公共父级。 可以通过添加整个星球来添加一个顶层。  
  
```sql
INSERT SimpleDemo  
    VALUES ('/', 'Earth', 'Planet');  
```  
  
##  <a name="related-tasks"></a><a name="tasks"></a> 相关任务  
  
###  <a name="migrating-from-parentchild-to-hierarchyid"></a><a name="migrating"></a> 从父/子迁移到 hierarchyid  
 大多数树都使用父/子结构来表示。 从父/子结构迁移到使用 **hierarchyid** 的表的最简单方法是，使用临时列或临时表来跟踪各个层次结构级别的节点数。 有关迁移父/子表的示例，请参阅 [教程：使用 hierarchyid 数据类型](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)的第 1 课。  
  
  
###  <a name="managing-a-tree-using-hierarchyid"></a><a name="BKMK_ManagingTrees"></a> 使用 hierarchyid 管理树  
 尽管 **hierarchyid** 列不一定表示树，但应用程序可以很容易地确保此列表示树。  
  
-   生成新的值时，请执行下列操作之一：  
  
    -   跟踪父行中的最后一个子级编号。  
  
    -   计算最后一个子级。 若要高效地执行此操作，需要使用广度优先索引。  
  
-   通过对列创建可能属于聚集键的唯一索引，强制实现唯一性。 若要确保插入的值是唯一的，请执行下列操作之一：  
  
    -   检测唯一键冲突故障并重试。  
  
    -   确定每个新子节点的唯一性，并将其作为可序列化事务的一部分插入。  
  
  
#### <a name="example-using-error-detection"></a>使用错误检测的示例  
 在下例中，示例代码计算新子级的 **EmployeeId** 值，接着检测有无键冲突，然后返回 **INS_EMP** 标记以便重新计算新行的 **EmployeeId** 值：  
  
```sql
USE AdventureWorks ;  
GO  
  
CREATE TABLE Org_T1  
   (  
    EmployeeId hierarchyid PRIMARY KEY,  
    OrgLevel AS EmployeeId.GetLevel(),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
  
CREATE INDEX Org_BreadthFirst ON Org_T1(OrgLevel, EmployeeId);
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50) )   
AS  
BEGIN  
    DECLARE @last_child hierarchyid;
INS_EMP:   
    SELECT @last_child = MAX(EmployeeId) FROM Org_T1   
        WHERE EmployeeId.GetAncestor(1) = @mgrid;
    INSERT INTO Org_T1 (EmployeeId, EmployeeName)  
        SELECT @mgrid.GetDescendant(@last_child, NULL), @EmpName;
-- On error, return to INS_EMP to recompute @last_child  
IF @@error <> 0 GOTO INS_EMP   
END ;  
GO  
```  
  
  
#### <a name="example-using-a-serializable-transaction"></a>使用可序列化事务的示例  
 Org_BreadthFirst 索引可确保确定 \@last_child 使用范围查找。 除了应用程序可能需要检查的其他错误情况之外，插入后出现重复键冲突表示试图添加具有同一 ID 的多个雇员，因此必须重新计算 \@last_child。 以下代码计算可序列化事务中的新节点值：  
  
```sql
CREATE TABLE Org_T2  
    (  
    EmployeeId hierarchyid PRIMARY KEY,  
    LastChild hierarchyid,   
    EmployeeName nvarchar(50)   
    ) ;  
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50))   
AS  
BEGIN  
DECLARE @last_child hierarchyid  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION   
  
SELECT @last_child  =  EmployeeId.GetDescendant(LastChild,NULL)
FROM Org_T2
WHERE EmployeeId = @mgrid

UPDATE Org_T2 SET LastChild = @last_child  WHERE EmployeeId = @mgrid

INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(@last_child, @EmpName)  
COMMIT  
END ;  
```  
  
 下面的代码使用三行数据填充表，并返回结果：  
  
```sql
INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(hierarchyid::GetRoot(), 'David') ;  
GO  
AddEmp 0x , 'Sariya'  
GO  
AddEmp 0x58 , 'Mary'  
GO  
SELECT * FROM Org_T2  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
EmployeeId LastChild EmployeeName  
---------- --------- ------------  
0x        0x58       David  
0x58      0x5AC0     Sariya  
0x5AC0    NULL       Mary  
```  
  
  
###  <a name="enforcing-a-tree"></a><a name="BKMK_EnforcingTrees"></a> 强制表示树  
 以上示例说明了应用程序如何确保树得到维护。 若要通过使用约束强制表示树，创建定义各节点父级的计算列时可以为它创建一个反过来约束主键 ID 的外键。  
  
```sql
CREATE TABLE Org_T3  
(  
   EmployeeId hierarchyid PRIMARY KEY,  
   ParentId AS EmployeeId.GetAncestor(1) PERSISTED    
      REFERENCES Org_T3(EmployeeId),  
   LastChild hierarchyid,   
   EmployeeName nvarchar(50)  
)  
GO  
```  
  
 当用于维护层次结构树的不可信代码对表拥有直接 DML 访问权限时，将优先采用这种强制关系的方法。 但是，这种方法可能会降低性能，因为必须针对每个 DML 操作检查约束。  
  
  
###  <a name="finding-ancestors-by-using-the-clr"></a><a name="findclr"></a> 使用 CLR 查找祖先  
 查找级别最低的共同祖先就是一项涉及层次结构中两个节点的常用操作。 可以用 [!INCLUDE[tsql](../includes/tsql-md.md)] 或 CLR 编写此操作，因为它们都提供 **hierarchyid** 类型。 建议用 CLR，因为用它时查找速度会更快。  
  
 使用下面的 CLR 代码来列出祖先，并查找级别最低的共同祖先：  
  
```csharp
using System;  
using System.Collections;  
using System.Text;  
using Microsoft.SqlServer.Server;  // SqlFunction Attribute
using Microsoft.SqlServer.Types;   // SqlHierarchyId
  
public partial class HierarchyId_Operations  
{  
    [SqlFunction(FillRowMethodName = "FillRow_ListAncestors")]
    public static IEnumerable ListAncestors(SqlHierarchyId h)
    {  
        while (!h.IsNull)  
        {  
            yield return (h);  
            h = h.GetAncestor(1);  
        }  
    }  
    public static void FillRow_ListAncestors(
        Object obj,
        out SqlHierarchyId ancestor
        )
    {  
        ancestor = (SqlHierarchyId)obj;  
    }  
  
    public static HierarchyId CommonAncestor(
        SqlHierarchyId h1,
        HierarchyId h2
        )  
    {  
        while (!h1.IsDescendantOf(h2))  
            h1 = h1.GetAncestor(1);  
  
        return h1;  
    }  
}  
```  
  
 若要在以下 **示例中使用** ListAncestor **和** CommonAncestor [!INCLUDE[tsql](../includes/tsql-md.md)] 方法，请在 **中通过执行如下代码生成 DLL 并创建** HierarchyId_Operations [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 程序集：  
  
```sql
CREATE ASSEMBLY HierarchyId_Operations   
    FROM '<path to DLL>\ListAncestors.dll';
GO  
```  
  
  
###  <a name="listing-ancestors"></a><a name="ancestors"></a> 列出祖先  
 创建节点的祖先列表是一项常用操作，例如可用于显示组织中的职位。 此操作的其中一种实现方式就是使用表值函数和上面定义的 **HierarchyId_Operations** 类：  
  
 使用 [!INCLUDE[tsql](../includes/tsql-md.md)]：  
  
```sql
CREATE FUNCTION ListAncestors (@node hierarchyid)  
RETURNS TABLE (node hierarchyid)  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.ListAncestors  
GO  
```  
  
 用法示例：  
  
```sql
DECLARE @h hierarchyid  
SELECT @h = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0' -- /1/1/5/2/  
  
SELECT LoginID, OrgNode.ToString() AS LogicalNode  
FROM HumanResources.EmployeeDemo AS ED  
JOIN ListAncestors(@h) AS A   
   ON ED.OrgNode = A.Node  
GO  
```  
  
  
###  <a name="finding-the-lowest-common-ancestor"></a><a name="lowestcommon"></a> 查找级别最低的共同祖先  
 使用上面定义的 **HierarchyId_Operations** 类创建以下 [!INCLUDE[tsql](../includes/tsql-md.md)] 函数，以便查找涉及层次结构中两个节点的最低级别的共同祖先：  
  
```sql
CREATE FUNCTION CommonAncestor (@node1 hierarchyid, @node2 hierarchyid)  
RETURNS hierarchyid  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.CommonAncestor  
GO  
```  
  
 用法示例：  
  
```sql
DECLARE @h1 hierarchyid, @h2 hierarchyid;
  
SELECT @h1 = OrgNode   
FROM  HumanResources.EmployeeDemo   
WHERE LoginID = 'adventure-works\jossef0'; -- Node is /1/1/3/  
  
SELECT @h2 = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0'; -- Node is /1/1/5/2/  
  
SELECT OrgNode.ToString() AS LogicalNode, LoginID   
FROM HumanResources.EmployeeDemo    
WHERE OrgNode = dbo.CommonAncestor(@h1, @h2) ;  
```  
  
 最终得到的节点为 /1/1/  
  
  
###  <a name="moving-subtrees"></a><a name="BKMK_MovingSubtrees"></a> 移动子树  
 另一项常用操作是移动子树。 下面的过程采用 \@oldMgr 的子树作为参数，使其（包括 \@oldMgr）成为 \@newMgr的子树  。  
  
```sql
CREATE PROCEDURE MoveOrg(@oldMgr nvarchar(256), @newMgr nvarchar(256) )  
AS  
BEGIN  
DECLARE @nold hierarchyid, @nnew hierarchyid  
SELECT @nold = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @oldMgr ;  
  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION  
SELECT @nnew = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @newMgr ;  
  
SELECT @nnew = @nnew.GetDescendant(max(OrgNode), NULL)   
FROM HumanResources.EmployeeDemo WHERE OrgNode.GetAncestor(1)=@nnew ;  
  
UPDATE HumanResources.EmployeeDemo    
SET OrgNode = OrgNode.GetReparentedValue(@nold, @nnew)  
WHERE OrgNode.IsDescendantOf(@nold) = 1 ;  
  
COMMIT TRANSACTION;
END ;  
GO  
```  
  
  
## <a name="see-also"></a>另请参阅  
 [hierarchyid 数据类型方法引用](../t-sql/data-types/hierarchyid-data-type-method-reference.md)   
 [教程：使用 hierarchyid 数据类型](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)   
 [hierarchyid (Transact-SQL)](../t-sql/data-types/hierarchyid-data-type-method-reference.md)  
  
