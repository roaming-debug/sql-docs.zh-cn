---
description: MSSQLSERVER_2020
title: MSSQLSERVER_2020 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2020 (Database Engine error)
ms.assetid: 4a8bf90f-a083-4c53-84f0-d23c711c8081
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3898acf2b29719b2fa17566c579c1f350d6524cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196467"
---
# <a name="mssqlserver_2020"></a>MSSQLSERVER_2020
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|2020|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称||  
|消息正文|为实体“%.*ls”报告的依赖关系不包含对列的引用。 这是由于此实体引用的对象不存在，或由于此实体中的一个或多个语句有错误。  在重新运行该查询之前，请确保该实体中没有错误并且该实体引用的所有对象都存在。|  
  
## <a name="explanation"></a>说明  
**sys.dm_sql_referenced_entities** 系统函数将报告绑定到架构的引用的所有列级依赖关系。 例如，此函数将报告索引视图的所有列级依赖关系，因为索引视图需要架构绑定。 但是，如果被引用实体不绑定到架构，则仅当引用列的所有语句可以绑定时，才会报告列依赖关系。 只有在分析语句时所有对象都存在，才可以成功绑定语句。 如果实体中有任何已定义的语句无法进行绑定，将不会报告列依赖关系，并且 **referenced_minor_id** 列将返回 0。 无法解析列依赖关系时，将引发错误 2020。 此错误不会阻止查询返回对象级依赖关系。  
  
## <a name="user-action"></a>用户操作  
更正消息中在错误 2020 之前标识的所有错误。 例如，在以下代码示例中，视图 `Production.ApprovedDocuments` 在 `Title` 表的列`ChangeNumber`、`Status` 和 `Production.Document` 中定义。 随后向 **sys.dm_sql_referenced_entities** 系统函数查询 `ApprovedDocuments` 视图所依赖的对象和列。 因为视图不是使用 WITH SCHEMA_BINDING 子句创建的，所以可以在被引用表中修改视图中引用的列。 该示例通过将 `ChangeNumber` 表中的列 `Production.Document` 重命名为 `TrackingNumber` 对其进行更改。 将再次向目录视图查询 `ApprovedDocuments` 视图；但是它不能绑定到该视图中定义的所有列。 将返回错误 207 和 2020 来标识问题。 若要解决问题，必须改变视图，以反映列的新名称。  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE VIEW Production.ApprovedDocuments  
AS  
SELECT Title, ChangeNumber, Status  
FROM Production.Document  
WHERE Status = 2;  
GO  
SELECT referenced_schema_name AS schema_name  
,referenced_entity_name AS table_name  
,referenced_minor_name AS referenced_column  
FROM sys.dm_sql_referenced_entities ('Production.ApprovedDocuments', 'OBJECT');  
GO  
EXEC sp_rename 'Production.Document.ChangeNumber', 'TrackingNumber', 'COLUMN';  
GO  
SELECT referenced_schema_name AS schema_name  
,referenced_entity_name AS table_name  
,referenced_minor_name AS referenced_column  
FROM sys.dm_sql_referenced_entities ('Production.ApprovedDocuments', 'OBJECT');  
GO
```
  
该查询返回以下错误消息。  
  
```
Msg 207, Level 16, State 1, Procedure ApprovedDocuments, Line 3  
Invalid column name 'ChangeNumber'.  
Msg 2020, Level 16, State 1, Line 1  
The dependencies reported for entity  
"Production.ApprovedDocuments" do not include references to  
columns. This is either because the entity references an  
object that does not exist or because of an error in one or  
more statements in the entity. Before rerunning the query,  
ensure that there are no errors in the entity and that all  
objects referenced by the entity exist.
```
  
以下示例更正了视图中的列名。  
  
```sql
USE AdventureWorks2012;  
GO  
ALTER VIEW Production.ApprovedDocuments  
AS  
SELECT Title,TrackingNumber, Status  
FROM Production.Document  
WHERE Status = 2;  
GO
```
  
## <a name="see-also"></a>另请参阅  
[sys.dm_sql_referenced_entities (Transact-SQL)](~/relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
