---
description: DML 触发器
title: DML 触发器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], about triggers
- DML triggers, about DML triggers
- triggers [SQL Server]
ms.assetid: 298eafca-e01f-4707-8c29-c75546fcd6b0
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4105a3c3b0ba43d3ed142cba31b2f991f6dc5287
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461558"
---
# <a name="dml-triggers"></a>DML 触发器
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  DML 触发器为特殊类型的存储过程，可在发生数据操作语言 (DML) 事件时自动生效，以便影响触发器中定义的表或视图。 DML 事件包括 INSERT、UPDATE 或 DELETE 语句。 DML 触发器可用于强制业务规则和数据完整性、查询其他表并包括复杂的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 将触发器和触发它的语句作为可在触发器内回滚的单个事务对待。 如果检测到错误（例如，磁盘空间不足），则整个事务即自动回滚。  
  
## <a name="dml-trigger-benefits"></a>DML 触发器的优点  
 DML 触发器类似于约束，因为可以强制实体完整性或域完整性。 一般情况下，实体完整性总应在最低级别上通过索引进行强制，这些索引应是 PRIMARY KEY 和 UNIQUE 约束的一部分，或者是独立于约束而创建的。 域完整性应通过 CHECK 约束进行强制，而引用完整性 (RI) 则应通过 FOREIGN KEY 约束进行强制。 当约束支持的功能无法满足应用程序的功能要求时，DML 触发器非常有用。  
  
 下面的列表比较 DML 触发器和约束，并在 DML 触发器优于约束时进行标识。  
  
-   DML 触发器可以将更改通过级联方式传播给数据库中的相关表；不过，使用级联引用完整性约束可以更有效地执行这些更改。 除非 REFERENCES 子句定义了级联引用操作，否则 FOREIGN KEY 约束只能用与另一列中的值完全匹配的值来验证列值。  
  
-   DML 触发器可以防止恶意或错误的 INSERT、UPDATE 以及 DELETE 操作，并强制执行比 CHECK 约束定义的限制更为复杂的其他限制。  
  
     与 CHECK 约束不同，DML 触发器可以引用其他表中的列。 例如，触发器可以使用另一个表中的 SELECT 比较插入或更新的数据，以及执行其他操作，如修改数据或显示用户定义错误信息。  
  
-   DML 触发器可以评估数据修改前后表的状态，并根据该差异采取措施。  
  
-   一个表中的多个同类 DML 触发器（INSERT、UPDATE 或 DELETE）允许采取多个不同的操作来响应同一个修改语句。  
  
-   约束只能通过标准化的系统错误消息来传递错误消息。 如果应用程序需要（或能受益于）使用自定义消息和较为复杂的错误处理，则必须使用触发器。  
  
-   DML 触发器可以禁止或回滚违反引用完整性的更改，从而取消所尝试的数据修改。 当更改外键且新值与其主键不匹配时，这样的触发器将生效。 但是，FOREIGN KEY 约束通常用于此目的。  
  
-   如果触发器表上存在约束，则在 INSTEAD OF 触发器执行后但在 AFTER 触发器执行前检查这些约束。 如果违反了约束，则回滚 INSTEAD OF 触发器操作并且不执行 AFTER 触发器。  
  
## <a name="types-of-dml-triggers"></a>DML 触发器的类型  
 AFTER 触发器  
 在执行 INSERT、UPDATE、MERGE 或 DELETE 语句的操作之后执行 AFTER 触发器。 如果违反了约束，则永远不会执行 AFTER 触发器；因此，这些触发器不能用于任何可能防止违反约束的处理。 对于在 MERGE 语句中指定的每个 INSERT、UPDATE 或 DELETE 操作，将为每个 DML 操作触发相应的触发器。  
  
 INSTEAD OF 触发器  
 INSTEAD OF 触发器替代下列触发语句的标准操作。 因此，触发器可用于对一个或多个列执行错误或值检查，然后在插入、更新或删除行之前执行其他操作。 例如，当在工资表中小时工资列的更新值超过指定值时，可以将触发器定义为产生错误消息并回滚该事务，或在将记录插入工资表中之前将新记录插入到审核记录。 INSTEAD OF 触发器的主要优点是可以使不能更新的视图支持更新。 例如，基于多个基表的视图必须使用 INSTEAD OF 触发器来支持引用多个表中数据的插入、更新和删除操作。 INSTEAD OF 触发器的另一个优点是使您得以编写这样的逻辑代码：在允许批处理的其他部分成功的同时拒绝批处理中的某些部分。  
  
 下表对 AFTER 触发器和 INSTEAD OF 触发器的功能进行了比较。  
  
|函数|AFTER 触发器|INSTEAD OF 触发器|  
|--------------|-------------------|------------------------|  
|适用范围|表|表和视图|  
|每个表或视图包含触发器的数量|每个触发操作（UPDATE、DELETE 和 INSERT）包含多个触发器|每个触发操作（UPDATE、DELETE 和 INSERT）包含一个触发器|  
|级联引用|无任何限制条件|不允许在作为级联引用完整性约束目标的表上使用 INSTEAD OF UPDATE 和 DELETE 触发器。|  
|执行|晚于：<br /><br /> 约束处理<br /><br /> 声明性引用操作<br /><br /> 创建 **插入的** 和 **删除的** 表<br /><br /> 触发操作|之前：约束处理<br /><br /> 代替：触发操作<br /><br /> 之后：创建  **插入的** 和 **删除的** 表|  
|执行顺序|可指定第一个和最后一个执行|不适用|  
|**插入的** 和 **删除的** 表中的 **varchar(max)** 、 **nvarchar(max)** 和 **varbinary(max)** 列引用|允许|允许|  
|**插入的** 和 **删除的** 表中的 **text** 、 **ntext** 和 **image** 列引用。|不允许|允许|  
  
 CLR 触发器  
 CLR 触发器可以是 AFTER 触发器或 INSTEAD OF 触发器。 CLR 触发器还可以是 DDL 触发器。 CLR 触发器将执行在托管代码（在 .NET Framework 中创建并在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中上载的程序集的成员）中编写的方法，而不用执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]存储过程。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务|主题|  
|----------|-----------|  
|说明如何创建 DML 触发器。|[创建 DML 触发器](../../relational-databases/triggers/create-dml-triggers.md)|  
|说明如何创建 CLR 触发器。|[创建 CLR 触发器](../../relational-databases/triggers/create-clr-triggers.md)|  
|说明如何创建 DML 触发器，以处理单行和多行数据修改。|[创建 DML 触发器以处理多行数据](../../relational-databases/triggers/create-dml-triggers-to-handle-multiple-rows-of-data.md)|  
|说明如何嵌套触发器。|[创建嵌套触发器](../../relational-databases/triggers/create-nested-triggers.md)|  
|说明如何指定激发 AFTER 触发器的顺序。|[指定第一个和最后一个触发器](../../relational-databases/triggers/specify-first-and-last-triggers.md)|  
|说明如何在触发器代码中使用特殊的插入和删除表。|[使用插入的和删除的表](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md)|  
|说明如何修改或重命名 DML 触发器。|[修改或重命名 DML 触发器](../../relational-databases/triggers/modify-or-rename-dml-triggers.md)|  
|说明如何查看有关 DML 触发器的信息。|[获取有关 DML 触发器的信息](../../relational-databases/triggers/get-information-about-dml-triggers.md)|  
|说明如何删除或禁用 DML 触发器。|[删除或禁用 DML 触发器](../../relational-databases/triggers/delete-or-disable-dml-triggers.md)|  
|说明如何管理触发器安全性。|[管理触发器安全性](../../relational-databases/triggers/manage-trigger-security.md)|  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [触发器函数 (Transact-SQL)](../../t-sql/functions/trigger-functions-transact-sql.md)  
  
  
