---
description: 实现 DDL 触发器
title: 实现 DDL 触发器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- DDL triggers, implementing
ms.assetid: f44e5340-1d18-40e9-828e-0ffcca091ae3
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cfe13855cd03986f8b07cc6b2423346a422fab84
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402978"
---
# <a name="implement-ddl-triggers"></a>实现 DDL 触发器
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题介绍了有助于创建 DDL 触发器，修改 DDL 触发器以及禁用或删除 DDL 触发器的信息。  
  
## <a name="creating-ddl-triggers"></a>创建 DDL 触发器  
 DDL 触发器是使用 DDL 触发器的 [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TRIGGER 语句创建的。  
  
 **创建 DDL 触发器**  
  
-   [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的未来版本中将删除从触发器返回结果集的功能。 返回结果集的触发器可能会导致应用程序出现意外的行为，而这些行为并不符合它们的设计意图。 避免在新的开发工作中从触发器返回结果集，并计划修改当前执行此操作的应用程序。 若要防止触发器在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中返回结果集，请将 [disallow results from triggers 选项](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) 设置为 1。 在后续版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，此选项的默认设置为 1。  
  
## <a name="modifying-ddl-triggers"></a>修改 DDL 触发器  
 如果必须修改 DDL 触发器的定义，只需一个操作即可删除并重新创建触发器，或重新定义现有触发器。  
  
 如果更改了由 DDL 触发器引用的对象的名称，则必须修改触发器，以使其文本反映新的名称。 因此，在重命名对象之前，需要先显示该对象的依赖关系，以确定所建议的更改是否会影响任何触发器。  
  
 也可将修改触发器，对其定义进行加密。  
  
 **修改触发器**  
  
-   [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
 **查看触发器的依赖关系**  
  
-   [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
-   [sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
-   [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
## <a name="disabling-and-dropping-ddl-triggers"></a>禁用和删除 DDL 触发器  
 当不再需要某个 DDL 触发器时，可以禁用或删除该触发器。  
  
 禁用 DDL 触发器不会将其删除。 该触发器仍然作为对象存在于当前数据库中。 但是，当运行编写触发器程序所用的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时，不会激发触发器。 可以重新启用禁用的 DDL 触发器。 在启用 DDL 触发器后，该触发器的激发方式与其最初创建时的激发方式相同。 创建的 DDL 触发器默认处于启用状态。  
  
 删除 DDL 触发器时，该触发器将从当前数据库中删除。 DDL 触发器范围内的任何对象或数据均不受影响。  
  
 **禁用 DDL 触发器**  
  
-   [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)  
  
-   [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
 **启用 DDL 触发器**  
  
-   [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)  
  
-   [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
 **删除 DDL 触发器**  
  
-   [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)  
  
  
