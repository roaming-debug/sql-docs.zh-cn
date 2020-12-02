---
description: 第 1 课：将表转换为层次结构
title: 第 1 课：将表转换为层次结构 | Microsoft Docs
ms.custom: ''
ms.date: 08/22/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- HierarchyID
ms.assetid: 5ee6f19a-6dd7-4730-a91c-bbed1bd77e0b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4a56b34301386287ef954edae0528decd4d03fee
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91809473"
---
# <a name="lesson-1-converting-a-table-to-a-hierarchical-structure"></a>第 1 课：将表转换为层次结构
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
具有使用自联接表示层次结构关系的表的客户可以将本课程作为指南，将他们的表转换为层次结构。 相对而言，从这种表示形式迁移为使用 **hierarchyid** 的表示形式较为容易。 迁移之后，用户将拥有一个精简且易于理解的层次结构表示形式，可以采用多种方式对其进行索引以进行有效查询。  
  
本课程将检查现有表、创建一个包含 **hierarchyid** 列的新表、使用源表中的数据填充此表，然后再演示三个索引策略。 本课程包含以下主题：  
 
  
## <a name="prerequisites"></a>必备条件  
若要完成本教程，需要 SQL Server Management Studio、针对运行 SQL Server 的服务器的访问权限以及 AdventureWorks 数据库。

- 安装 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
- 安装 [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。
- 下载 [AdventureWorks2017 示例数据库](../../samples/adventureworks-install-configure.md)。

此处提供在 SSMS 中还原数据库的说明：[还原数据库](../backup-restore/restore-a-database-backup-using-ssms.md)。  

## <a name="examine-the-current-structure-of-the-employee-table"></a>检查 Employee 表的当前结构
示例 Adventureworks2017（或更高版本）数据库包含基于“HumanResources”架构的“Employee”表。 为了避免更改原始表，此步骤将对名为 **EmployeeDemo** 的 **Employee** 表创建一个副本。 若要简化此示例，你只需从原始表中复制五列数据。 然后，查询 **HumanResources.EmployeeDemo** 表以查看在不使用 **hierarchyid** 数据类型的情况下表中数据的结构。  
  
### <a name="copy-the-employee-table"></a>复制 Employee 表  
  
1.  在查询编辑器窗口中，运行下列代码以将 **Employee** 表中的表结构和数据复制到名为 **EmployeeDemo** 的新表中。 由于原始表已使用 hierarchyid，本次查询实质上合会并层次结构，以便检索 employee 的管理员。 在本课程的后续部分中，我们将重新构建该层次结构。

   ```sql  
   USE AdventureWorks2017;  
   GO  
     if OBJECT_ID('HumanResources.EmployeeDemo') is not null
    drop table HumanResources.EmployeeDemo 

    SELECT emp.BusinessEntityID AS EmployeeID, emp.LoginID, 
     (SELECT  man.BusinessEntityID FROM HumanResources.Employee man 
        WHERE emp.OrganizationNode.GetAncestor(1)=man.OrganizationNode OR 
            (emp.OrganizationNode.GetAncestor(1) = 0x AND man.OrganizationNode IS NULL)) AS ManagerID,
          emp.JobTitle, emp.HireDate
   INTO HumanResources.EmployeeDemo   
   FROM HumanResources.Employee emp ;
   GO
   ```  
  
### <a name="examine-the-structure-and-data-of-the-employeedemo-table"></a>检查 EmployeeDemo 表的结构和数据  
  
-   这个新的 **EmployeeDemo** 表表示现有数据库中你可能要迁移到新结构的典型表。 在查询编辑器窗口中，运行下列代码以显示表如何使用自联接来显示雇员/经理关系：  
  
    ```sql  
    SELECT   
        Mgr.EmployeeID AS MgrID, Mgr.LoginID AS Manager,   
        Emp.EmployeeID AS E_ID, Emp.LoginID, Emp.JobTitle  
    FROM HumanResources.EmployeeDemo AS Emp  
    LEFT JOIN HumanResources.EmployeeDemo AS Mgr  
    ON Emp.ManagerID = Mgr.EmployeeID  
    ORDER BY MgrID, E_ID  
    ```  
  
    [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
    ```  
    MgrID Manager                 E_ID LoginID                  JobTitle  
    NULL    NULL                    1   adventure-works\ken0        Chief Executive Officer
    1   adventure-works\ken0        2   adventure-works\terri0      Vice President of Engineering
    1   adventure-works\ken0       16   adventure-works\david0    Marketing Manager
    1   adventure-works\ken0       25   adventure-works\james1    Vice President of Production
    1   adventure-works\ken0      234   adventure-works\laura1    Chief Financial Officer
    1   adventure-works\ken0    263 adventure-works\jean0       Information Services Manager
    1   adventure-works\ken0      273   adventure-works\brian3    Vice President of Sales
    2   adventure-works\terri0    3 adventure-works\roberto0    Engineering Manager
    3   adventure-works\roberto0    4   adventure-works\rob0        Senior Tool Designer
    ...  
    ```  
  
    结果在继续，共 290 行。  
  
请注意， **ORDER BY** 子句会使输出将每个管理级别的直接下属都列在一起。 例如，MgrID 1 (ken0) 的所有七个直接下属都彼此紧挨着列出。 虽然有可能实现，但是要将最终向 MgrID 1 负责的所有雇员进行分组将会更加困难。  


## <a name="populate-a-table-with-existing-hierarchical-data"></a>使用现有层次结构数据填充表
此任务将创建新表，然后使用 EmployeeDemo 表中的数据填充该表。 此任务包含以下步骤：  
  
-   创建一个包含 **hierarchyid** 列的新表。 此列可替换现有的 **EmployeeID** 和 **ManagerID** 列。 但是，您将保留这些列。 这是因为现有应用程序可能会引用这些列，而且它们还有助于您了解传输后的数据。 表定义将 **OrgNode** 指定为主键，这就要求该列包含唯一值。 **OrgNode** 列的聚集索引将使用 **OrgNode** 序列存储日期。    
-   创建一个用于跟踪直接向每个经理报告的雇员人数的临时表。 
-   使用 **EmployeeDemo** 表中的数据填充新表。  
  
### <a name="to-create-a-new-table-named-neworg"></a>创建一个名为 NewOrg 的新表  
  
-   在查询编辑器窗口中，运行下列代码以创建名为 **HumanResources.NewOrg** 的新表：  
  
    ```sql   
    CREATE TABLE HumanResources.NewOrg  
    (  
      OrgNode hierarchyid,  
      EmployeeID int,  
      LoginID nvarchar(50),  
      ManagerID int  
    CONSTRAINT PK_NewOrg_OrgNode  
      PRIMARY KEY CLUSTERED (OrgNode)  
    );  
    GO  
    ```  
  
### <a name="create-a-temporary-table-named-children"></a>创建一个名为 #Children 的临时表  
  
1.  创建一个名为 **#Children** 的临时表，其中有一个名为 **Num** 的列，该列将包含每个节点的子级数：  
  
    ```sql  
    CREATE TABLE #Children   
       (  
        EmployeeID int,  
        ManagerID int,  
        Num int  
    );  
    GO  
    ```  
  
2.  添加一个索引，该索引将显著提高用于填充 **NewOrg** 表的查询的速度：  
  
    ```sql  
    CREATE CLUSTERED INDEX tmpind ON #Children(ManagerID, EmployeeID);  
    GO  
    ```  
  
### <a name="populate-the-neworg-table"></a>填充 NewOrg 表  
  
1.  递归查询禁止使用聚合进行子查询。 而是使用下列代码填充 **#Children** 表，此代码使用 [ROW_NUMBER()](../../t-sql/functions/row-number-transact-sql.md) 方法填充 **Num** 列：  
  
    ```sql 
    INSERT #Children (EmployeeID, ManagerID, Num)  
    SELECT EmployeeID, ManagerID,  
      ROW_NUMBER() OVER (PARTITION BY ManagerID ORDER BY ManagerID)   
    FROM HumanResources.EmployeeDemo  
    GO 
    ```  
  
2.  查看 **#Children** 表。 请注意 **Num** 列包含每个经理的序号的方式。  
  
    ```sql  
    SELECT * FROM #Children ORDER BY ManagerID, Num  
    GO  
  
    ```  
  
    [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

    ```
    EmployeeID  ManagerID   Num
    1   NULL    1
    2        1    1
    16     1      2
    25     1      3
    234    1      4
    263    1      5
    273    1      6
    3        2    1
    4        3    1
    5        3    2
    6        3    3
    7        3    4
    ```


3.  填充 **NewOrg** 表。 使用 GetRoot 和 ToString 方法将 **Num** 值连接为 **hierarchyid** 格式，然后再使用生成的层次结构值填充 **OrgNode** 列：  
  
    ```sql  
    WITH paths(path, EmployeeID)   
    AS (  
    -- This section provides the value for the root of the hierarchy  
    SELECT hierarchyid::GetRoot() AS OrgNode, EmployeeID   
    FROM #Children AS C   
    WHERE ManagerID IS NULL   

    UNION ALL   
    -- This section provides values for all nodes except the root  
    SELECT   
    CAST(p.path.ToString() + CAST(C.Num AS varchar(30)) + '/' AS hierarchyid),   
    C.EmployeeID  
    FROM #Children AS C   
    JOIN paths AS p   
       ON C.ManagerID = P.EmployeeID   
    )  
    INSERT HumanResources.NewOrg (OrgNode, O.EmployeeID, O.LoginID, O.ManagerID)  
    SELECT P.path, O.EmployeeID, O.LoginID, O.ManagerID  
    FROM HumanResources.EmployeeDemo AS O   
    JOIN Paths AS P   
       ON O.EmployeeID = P.EmployeeID  
    GO 
    ```  
  
4.  将 **hierarchyid** 列转换为字符格式会更易于理解。 执行下列代码查看 **NewOrg** 表中的数据，该表包含 **OrgNode** 列的两种表示形式：  
  
    ```sql  
    SELECT OrgNode.ToString() AS LogicalNode, *   
    FROM HumanResources.NewOrg   
    ORDER BY LogicalNode;  
    GO  
    ```  
  
    **LogicalNode** 列将 **hierarchyid** 列转换为一种表示层次结构的更易读的文本形式。 在其余的任务中，你将使用 `ToString()` 方法显示 **hierarchyid** 列的逻辑格式。  
  
5.  删除不再需要的临时表：  
  
    ```sql  
    DROP TABLE #Children  
    GO  
    ```  
  
## <a name="optimizing-the-neworg-table"></a>优化 NewOrg 表
在[使用现有层次结构数据填充表]()任务中创建的“NewOrd”表包含所有雇主信息，该表使用 hierarchyid 数据类型表示层次结构。 此任务添加了新的索引，以便支持对“hierarchyid”列的搜索。  
  

“hierarchyid”列 (**OrgNode**) 是“NewOrg”表的主键。 此表创建时，其内包含了一个名为 **PK_NewOrg_OrgNode** 的聚集索引，用于强制实现“OrgNode”列的唯一性。 此聚集索引还支持对表进行深度优先搜索。  
  
  
### <a name="create-index-on-neworg-table-for-efficient-searches"></a>为 NewOrg 表创建索引以提高搜索效率  
  
1.  若要改善在层次结构中同一级别的查询，可以使用 [GetLevel](../../t-sql/data-types/getlevel-database-engine.md) 方法创建一个包含此层次结构中的此级别的计算列。 然后，对此级别和 **Hierarchyid** 创建一个组合索引。 运行下列代码以创建计算列和广度优先索引：  
  
    ```sql  
    ALTER TABLE HumanResources.NewOrg   
       ADD H_Level AS OrgNode.GetLevel() ;  
    CREATE UNIQUE INDEX EmpBFInd   
       ON HumanResources.NewOrg(H_Level, OrgNode) ;  
    GO  
    ```  
  
2.  对“EmployeeID”列创建一个唯一索引。 即采用传统方式通过 **EmployeeID** 号单独查找一个雇员。 运行下列代码以便对 **EmployeeID** 创建索引：  
  
    ```sql  
    CREATE UNIQUE INDEX EmpIDs_unq ON HumanResources.NewOrg(EmployeeID) ;  
    GO
    ```  
  
3.  运行下列代码以分别按照三个索引的顺序从表中检索数据：  
  
    ```sql  
    SELECT OrgNode.ToString() AS LogicalNode,  
    OrgNode, H_Level, EmployeeID, LoginID  
    FROM HumanResources.NewOrg   
    ORDER BY OrgNode;  

    SELECT OrgNode.ToString() AS LogicalNode,  
    OrgNode, H_Level, EmployeeID, LoginID   
    FROM HumanResources.NewOrg   
    ORDER BY H_Level, OrgNode;  

    SELECT OrgNode.ToString() AS LogicalNode,  
    OrgNode, H_Level, EmployeeID, LoginID   
    FROM HumanResources.NewOrg   
    ORDER BY EmployeeID;  
    GO  
    ```  
  
4.  比较结果集以查看每类索引中的存储顺序。 下面只显示了各输出的前四行。  
  
    [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
    深度优先索引：雇员记录存储在与相应经理的记录相邻的位置。  

    ```
    LogicalNode OrgNode H_Level EmployeeID  LoginID
    /   0x  0   1   adventure-works\ken0
    /1/ 0x58    1   2   adventure-works\terri0
    /1/1/   0x5AC0  2   3   adventure-works\roberto0
    /1/1/1/ 0x5AD6  3   4   adventure-works\rob0
    /1/1/2/ 0x5ADA  3   5   adventure-works\gail0
    /1/1/3/ 0x5ADE  3   6   adventure-works\jossef0
    /1/1/4/ 0x5AE1  3   7   adventure-works\dylan0
    /1/1/4/1/   0x5AE158    4   8   adventure-works\diane1
    /1/1/4/2/   0x5AE168    4   9   adventure-works\gigi0
    /1/1/4/3/   0x5AE178    4   10  adventure-works\michael6
    /1/1/5/ 0x5AE3  3   11  adventure-works\ovidiu0
    ```

    **EmployeeID** 优先索引：各行按照 **EmployeeID** 顺序存储。  

    ```
    LogicalNode OrgNode H_Level EmployeeID  LoginID
    /   0x  0   1   adventure-works\ken0
    /1/ 0x58    1   2   adventure-works\terri0
    /1/1/   0x5AC0  2   3   adventure-works\roberto0
    /1/1/1/ 0x5AD6  3   4   adventure-works\rob0
    /1/1/2/ 0x5ADA  3   5   adventure-works\gail0
    /1/1/3/ 0x5ADE  3   6   adventure-works\jossef0
    /1/1/4/ 0x5AE1  3   7   adventure-works\dylan0
    /1/1/4/1/   0x5AE158    4   8   adventure-works\diane1
    /1/1/4/2/   0x5AE168    4   9   adventure-works\gigi0
    /1/1/4/3/   0x5AE178    4   10  adventure-works\michael6
    /1/1/5/ 0x5AE3  3   11  adventure-works\ovidiu0
    /1/1/5/1/   0x5AE358    4   12  adventure-works\thierry0
    ```
  
> [!NOTE]  
> 有关显示深度优先索引和广度优先索引之间差异的关系图，请参阅[分层数据 (SQL Server)](../../relational-databases/hierarchical-data-sql-server.md)。  
  
### <a name="drop-the-unnecessary-columns"></a>删除不需要的列  
  
1.  “ManagerID” 列用于表示雇员/经理关系，现在由“OrgNode”列来表示。 如果其他应用程序不需要“ManagerID”列，可以考虑使用下列语句删除该列：  
  
    ```sql  
    ALTER TABLE HumanResources.NewOrg DROP COLUMN ManagerID ;  
    GO  
    ```  
  
2.  “EmployeeID”列也是冗余列。 “OrgNode”列可以唯一标识每个雇员。 如果其他应用程序不需要“EmployeeID”列，可以考虑使用下列代码先删除索引再删除该列：  
  
    ```sql  
    DROP INDEX EmpIDs_unq ON HumanResources.NewOrg ;  
    ALTER TABLE HumanResources.NewOrg DROP COLUMN EmployeeID ;  
    GO  
    ```  
  
### <a name="replace-the-original-table-with-the-new-table"></a>使用新表替换原始表  
  
1.  如果原始表包含任何其他索引或约束，请将它们添加到“NewOrg”表中。  
  
2.  将旧的“EmployeeDemo”表替换为新表。 运行下列代码以删除旧表，然后使用旧表的名称重新命名新表：  
  
    ```sql  
    DROP TABLE HumanResources.EmployeeDemo ;  
    GO  
    sp_rename 'HumanResources.NewOrg', 'EmployeeDemo' ;  
    GO  
    ```  
  
3.  运行下列代码以检查最终表：  
  
    ```sql  
    SELECT * FROM HumanResources.EmployeeDemo ;  
    ```  
  
## <a name="next-steps"></a>后续步骤
下一篇文章将介绍如何在分层表中创建和管理数据。 

转到下一篇文章，了解详细信息：
> [!div class="nextstepaction"]
> [后续步骤](lesson-2-creating-and-managing-data-in-a-hierarchical-table.md)