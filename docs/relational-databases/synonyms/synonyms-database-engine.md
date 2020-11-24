---
description: 同义词（数据库引擎）
title: 同义词（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- synonyms [SQL Server], about synonyms
ms.assetid: 6210e1d5-075f-47e4-ac8d-f84bcf26fbc0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 74f5c5dcf2f2e1891daca22d70ebb9d9f1d9119f
ms.sourcegitcommit: a49a66dbda0cb16049e092b49c8318ac3865af3c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94983111"
---
# <a name="synonyms-database-engine"></a>同义词（数据库引擎）
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  同义词是用来实现下列用途的数据库对象：  
  
-   为可以存在于本地或远程服务器上的其他数据库对象（称为基对象）提供备用名称。  
  
-   提供抽象层以免对客户端应用程序基对象的名称或位置进行更改。  
  
例如，名为 **Server1** 的服务器上有 [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)]的 **Employee** 表。 若要从其他服务器 **Server2** 引用此表，则客户端应用程序必须使用由四个部分构成的名称 **Server1.AdventureWorks.Person.Employee**。 另外，如果更改表的位置（例如，更改为其他服务器），则必须修改客户端应用程序以反映此更改。  
  
若要解决这两个问题，您可以在 **Server2** 上为 **Server1** 上的 **Employee** 表创建一个同义词 **EmpTable**。 这样，客户端应用程序只需使用由一个部分构成的名称 **EmpTable** 来引用 **Employee** 表。 另外，如果 **Employee** 表的位置发生变化，则必须修改同义词 **EmpTable** 以指向 **Employee** 表的新位置。 由于不存在 ALTER SYNONYM 语句，因此必须首先删除同义词 **EmpTable**，然后重新创建同名的同义词，但是要将同义词指向 **Employee** 的新位置。  
  
同义词从属于架构，并且与架构中的其他对象一样，其名称必须是唯一的。 可以为下列数据库对象创建同义词：  

:::row:::
    :::column:::
        程序集 (CLR) 存储过程

        程序集 (CLR) 标量函数

        复制筛选过程

        SQL 标量函数

        SQL 内联表值函数

        查看
    :::column-end:::
    :::column:::
        程序集 (CLR) 表值函数

        程序集 (CLR) 聚合函数

        SQL 表值函数

        SQL 存储过程

        表*（用户定义）
    :::column-end:::
:::row-end:::

*包括本地和全局临时表  
  
> [!NOTE]  
> 不支持使用函数基对象的四部分名称。  
  
同义词不能是另一个同义词的基对象，也不能引用用户定义聚合函数。  
  
同义词与其基对象之间只是按名称绑定。 对基对象的存在性、类型和权限检查都在运行时执行。 因此，可以修改或删除基对象，也可以先删除基对象，然后用与原始基对象同名的另一个对象来替换该基对象。 例如，有一个引用 **中的** Person.Contact **表的同义词** MyContacts [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)]。 如果将 **Contact** 表删除，并用名为 **Person.Contact** 的视图替换该表，则 **MyContacts** 将引用 **Person.Contact** 视图。  
  
同义词的引用不是绑定到架构的。 因此，可以随时删除同义词。 但删除同义词后，会留下已删除同义词的无关联引用， 而只有在运行时才会发现这些引用。  
  
## <a name="synonyms-and-schemas"></a>同义词和架构  
如果具有不属于您的默认架构并要创建同义词，则必须使用属于您的架构名称限定同义词名称。 例如，如果拥有架构 **x**，但 **y** 是默认架构，那么使用 CREATE SYNONYM 语句时，必须在同义词名称前添加架构 **x**，而不是使用由一个部分构成的名称来命名同义词。 有关如何创建同义词的详细信息，请参阅 [CREATE SYNONYM (Transact-SQL)](../../t-sql/statements/create-synonym-transact-sql.md)表。  
  
## <a name="granting-permissions-on-a-synonym"></a>授予同义词的有关权限  
只有同义词所有者、 **db_owner** 的成员或 **db_ddladmin** 的成员才能授予同义词的有关权限。  
  
可以`GRANT`、`DENY`和`REVOKE`对同义词的下列所有权限或任一权限：  

:::row:::
    :::column:::
      CONTROL

      EXECUTE

      SELECT

      UPDATE
    :::column-end:::
    :::column:::
      DELETE

      INSERT

      TAKE OWNERSHIP

      VIEW DEFINITION
    :::column-end:::
:::row-end:::

## <a name="using-synonyms"></a>使用同义词  
 可以在一些 SQL 语句和表达式上下文中使用同义词替代其引用的基对象。 以下列包含这些语句和表达式上下文的列表：  

:::row:::
    :::column:::
        SELECT

        UPDATE

        EXECUTE
    :::column-end:::
    :::column:::
        INSERT

        DELETE

        嵌套的 SELECT
    :::column-end:::
:::row-end:::
 
 在以前说明的上下文中使用同义词时，该基对象会受到影响。 例如，如果某个同义词引用了基对象（表）并且将行插入同义词中，则实际上将行插入到引用的表中。  
  
> [!NOTE]  
> 无法引用位于链接服务器上的同义词。  
  
 可以使用同义词作为 OBJECT_ID 函数的参数；但是，函数返回同义词的对象 ID，而不是基对象。  
  
 不能在 DDL 语句中引用同义词。 例如，引用名为 `dbo.MyProduct`的同义词的下列语句将生成错误：  
  
```sql  
ALTER TABLE dbo.MyProduct  
   ADD NewFlag int null;  
EXEC ('ALTER TABLE dbo.MyProduct  
   ADD NewFlag int null');  
```  
  
下列权限语句仅与同义词（而不是基对象）关联：  

:::row:::
    :::column:::
        GRANT

        REVOKE
    :::column-end:::
    :::column:::
        DENY
    :::column-end:::
:::row-end:::

因为同义词不绑定到架构，所以无法通过下列架构绑定表达式上下文引用：  

:::row:::
    :::column:::
        CHECK 约束

        默认的表达式

        绑定到架构的视图
    :::column-end:::
    :::column:::
        计算列

        规则表达式

        绑定到架构的函数
    :::column-end:::
:::row-end:::
  
有关绑定到架构的函数的详细信息，请参阅[创建用户定义的函数（数据库引擎）](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)。  
  
## <a name="getting-information-about-synonyms"></a>获取有关同义词的信息  
`sys.synonyms` 目录视图包含给定的数据库中的所有同义词项。 该目录视图将显示同义词元数据，例如同义词的名称和基对象的名称。 有关详细信息，请参阅 [sys.synonyms (Transact-SQL)](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)。  
  
使用扩展属性，您可以将描述性或说明性文本、输入掩码以及格式化规则添加为同义词的属性。 因为属性存储在数据库中，因此所有读取属性的应用程序都能以相同的方式评估对象。 有关详细信息，请参阅 [sp_addextendedproperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)。  
  
若要查找同义词基对象的基类型，请使用 OBJECTPROPERTYEX 函数。 有关详细信息，请参阅 [OBJECTPROPERTYEX (Transact-SQL) ](../../t-sql/functions/objectpropertyex-transact-sql.md)。  
  
### <a name="examples"></a>示例  
下面的示例将返回同义词基对象（本地对象）的基类型。  
  
```sql  
USE tempdb;  
GO  
CREATE SYNONYM MyEmployee   
FOR AdventureWorks2012.HumanResources.Employee;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('MyEmployee'), 'BaseType') AS BaseType;  
```  
  
下面的示例将返回同义词基对象（位于名为 `Server1`的服务器上的远程对象）的基类型。  
  
```sql  
EXECUTE sp_addlinkedserver Server1;  
GO  
CREATE SYNONYM MyRemoteEmployee  
FOR Server1.AdventureWorks2012.HumanResources.Employee;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('MyRemoteEmployee'), 'BaseType') AS BaseType;  
GO  
```  
  
## <a name="related-content"></a>相关内容  
 [创建同义词](../../relational-databases/synonyms/create-synonyms.md)    
 [CREATE SYNONYM (Transact-SQL)](../../t-sql/statements/create-synonym-transact-sql.md)    
 [DROP SYNONYM (Transact-SQL)](../../t-sql/statements/drop-synonym-transact-sql.md)    
  
