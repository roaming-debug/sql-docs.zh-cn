---
title: 包含的数据库 | Microsoft Docs
description: 了解包含的数据库、部分包含的数据库的优势和局限性，以及 SQL Server 2019 隔离数据库的方式。
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database
- database_uncontained_usage event
- partially contained database
- contained database, understanding
ms.assetid: 36af59d7-ce96-4a02-8598-ffdd78cdc948
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cd8b60ec5b302183fdb1a0713e71568821dae2fe
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813100"
---
# <a name="contained-databases"></a>包含的数据库
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  “包含数据库”  是独立于其他数据库以及承载数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的一种数据库。  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 以 4 种方法帮助用户使其数据库独立于实例。  
  
-   很多用于描述数据库的元数据都在该数据库中维护。 （除此之外或代替在 master 数据库中维护元数据。）  
  
-   使用相同的排序规则定义所有元数据。  
  
-   数据库可执行用户身份验证，因此减少了对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的登录名的数据库依赖关系。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境（DMV、XEvent 等）报告并可以执行包含信息。  
  
 部分包含的数据库的某些功能（例如将元数据存储在数据库中）适用于所有 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 数据库。 部分包含的数据库的某些优点（例如数据库级别身份验证和目录排序规则）必须在可用后才能实现。 使用 **CREATE DATABASE** 和 **ALTER DATABASE** 语句或者通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]可启用部分包含。 有关如何启用部分数据库包含的详细信息，请参阅 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)。  
  
##  <a name="partially-contained-database-concepts"></a><a name="Concepts"></a> 部分包含的数据库概念  
 完全包含的数据库包括定义数据库所需的所有数据库设置和元数据，它与安装数据库的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例没有配置依赖关系。 在以前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本中，将一个数据库与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例隔离开来可能会很耗时，并且要求该数据库与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例之间的关系的详细知识。 通过部分包含的数据库，可以轻松地将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的数据库与其他数据库隔离开来。  
  
 包含数据库将根据包含情况来考虑功能。 任何只依赖于数据库内部功能的用户定义实体均被视为处于完全包含状态。 任何依赖于数据库外部功能的用户定义实体均被视为处于非包含状态。 （有关详细信息，请参阅本主题后面的 [包含](#containment) 部分。）  
  
 以下术语适用于包含数据库模型。  
  
 数据库边界  
 数据库和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例之间的边界。 数据库和其他数据库之间的边界。  
  
 包含  
 完全在数据库边界中存在的元素。  
  
 非包含  
 跨数据库边界的元素。  
  
 非包含数据库  
 具有设置为 **NONE** 的包含的数据库。 版本早于 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 的所有数据库均属于非包含数据库。 默认情况下，所有 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本数据库的包含都设置为 **NONE**。  
  
 部分包含数据库  
 部分包含数据库是一种包含数据库，可允许存在跨越数据库边界的某些功能。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包括确定何时跨越包含边界的功能。  
  
 包含的用户  
 包含数据库有两种用户类型。  
  
-   **具有密码的包含数据库用户**  
  
     具有密码的包含数据库用户由数据库进行身份验证。 有关详细信息，请参阅 [包含的数据库用户 - 使你的数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
-   **Windows 主体**  
  
     授权的 Windows 用户和授权的 Windows 组的成员可直接连接到数据库，而无需登录 **master** 数据库。 数据库信任 Windows身份验证。  
  
 基于 **master** 数据库中登录名的用户可被授予对包含数据库的访问权限，但将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上创建依赖关系。 因此，基于登录创建用户需要部分包含。
  
> [!IMPORTANT]  
>  启用部分包含数据库会将对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的访问控制委托给该数据库的所有者。 有关详细信息，请参阅 [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md)。  
  
 数据库边界  
 由于部分包含数据库会区分数据库功能与实例功能，因此在这两个元素之间存在一条明确定义的边界，称为“数据库边界” 。  
  
 数据库边界之内是“数据库模型” ，在这里开发和管理数据库。 位于数据库模型内部的实体示例包括：系统表（如 **sys.tables**）、具有密码的包含数据库用户以及当前数据库中由特定名称（包含两部分）引用的用户表。  
  
 数据库边界之外是“管理模型” ，这与实例级别的功能和管理有关。 位于数据库边界之外的实体示例包括：系统表（如 **sys.endpoints**）、映射到登录名的用户，以及另一个数据库中由特定名称（包含三部分）引用的用户表。  
  
##  <a name="containment"></a><a name="containment"></a> 包含  
 完全位于数据库内部的用户实体被视为“包含” 实体。 任何位于数据库之外的实体或任何需要与数据库之外的功能进行交互的实体均被视为“非包含” 实体。  
  
 一般而言，用户实体分为以下几种包含类别：  
  
-   完全包含的用户实体（从不跨越数据库边界的用户实体），例如 sys.indexes。 任何使用这些功能的代码或任何只引用这些实体的对象也都是完全包含实体。  
  
-   非包含用户实体（跨越数据库边界的用户实体），例如 sys.server_principals 或服务器主体（登录名）本身。 任何使用这些实体的代码或任何引用这些实体的功能都是非包含实体。  
  
###  <a name="partially-contained-database"></a><a name="partial"></a> Partially Contained Database  
 包含数据库的功能目前只在部分包含状态下可用。 部分包含数据库是一种允许使用非包含功能的包含数据库。  
  
 使用 [sys.dm_db_uncontained_entities](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) 和 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) 视图可返回有关非包含对象或功能的信息。 通过确定数据库元素的包含状态，可以发现必须替换或更改哪些对象或功能才能提升包含。  
  
> [!IMPORTANT]  
>  由于某些对象的默认包含项目设置的是 **NONE**，此视图可能返回误报的结果。  
  
 部分包含数据库与非包含数据库之间在行为方面的最大区别在于排序规则。 有关排序规则问题的详细信息，请参阅 [Contained Database Collations](../../relational-databases/databases/contained-database-collations.md)。  
  
##  <a name="benefits-of-using-partially-contained-databases"></a><a name="benefits"></a> 使用部分包含数据库的好处  
 与非包含数据库关联的某些问题和复杂性可通过使用部分包含数据库加以解决。  
  
### <a name="database-movement"></a>数据库移动  
 在移动数据库时会发生的问题之一是：在数据库从一个实例移到另一个实例时，某些重要信息可能无法使用。 例如，登录信息存储在实例中，而不是存储于数据库中。 如果将非包含数据库从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的一个实例移到另一个实例，则这些信息会留在原地。 您必须标识缺失的信息，并将其与数据库一起移到新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中。 此过程不仅十分困难，而且耗时。  
  
 部分包含数据库可以在数据库中存储重要信息，因此，在移动信息后数据库仍具有信息。  
  
> [!NOTE]  
>  部分包含数据库可提供文档，说明无法独立于实例的数据库所使用的那些功能。 这包括其他相关数据库的列表、数据库要求但无法被包含的系统设置等。  
  
### <a name="benefit-of-contained-database-users-with-always-on"></a>使用 AlwaysOn 的包含的数据库用户的好处  
 通过减少与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的关联，当您使用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]时，在故障转移过程中部分包含数据库可能会很有用。  
  
 通过创建包含用户，可使用户直接连接到包含数据库。 这在高可用性和灾难恢复方案中（例如在 AlwaysOn 解决方案中）是非常重要的功能。 如果用户是包含用户，则在故障转移时，用户无需在承载辅助副本的实例上创建登录名，就能够连接到辅助副本。 这会给用户带来直接的好处。 有关详细信息，请参阅 [AlwaysOn 可用性组的概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 和 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
### <a name="initial-database-development"></a>初始数据库开发  
 由于开发人员可能不知道新数据库的部署环境，因此减少部署环境对数据库的影响可使开发人员的工作变得更加轻松。 在非包含模型中，开发人员必须相应考虑环境对新数据库和程序的可能影响。 但是，通过使用部分包含数据库，开发人员可检测到数据库在实例级别所受的影响，从而减轻了开发人员的心理负担。  
  
### <a name="database-administration"></a>数据库管理  
 通过在数据库中维护数据库设置，而非在 master 数据库中进行维护，使每个数据库所有者都可以更好地控制其数据库，而不必向数据库所有者授予 **sysadmin** 权限。  
  
##  <a name="limitations"></a><a name="Limitations"></a> 限制  
 部分包含数据库不允许以下功能。  
  
-   复制、变更数据捕获或更改跟踪。  
  
-   编号过程  
  
-   绑定到架构的对象，且依赖于可更改排序规则的内置功能  
  
-   绑定因排序规则更改而导致的变化，包括对对象、列、符号或类型的引用。
  
> [!WARNING]  
>  目前允许临时存储过程。 因为临时存储过程违反包含，所以，预计在将来的包含数据库版本中将不再支持临时存储过程。  
  
##  <a name="identifying-database-containment"></a><a name="Identifying"></a> 标识数据库包含关系  
 可以借助两个工具来帮助识别数据库的包含状态。 [sys.dm_db_uncontained_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) 是可以显示数据库中所有潜在的非包含实体的视图。 在运行时识别出任何实际的非包含实体时将 database_uncontained_usage 事件将发生。  
  
### <a name="sysdm_db_uncontained_entities"></a>sys.dm_db_uncontained_entities  
 此视图显示数据库中有可能成为非包含实体的所有实体（如跨数据库边界的那些实体）。 这包括可能在数据库模型外部使用对象的那些用户实体。 但是，由于直到运行时才能确定某些实体（例如那些使用动态 SQL 的实体）的包含关系，所以此视图中显示的某些实体实际上可能并不是非包含实体。 有关详细信息，请参阅 [sys.dm_db_uncontained_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)。  
  
### <a name="database_uncontained_usage-event"></a>database_uncontained_usage 事件  
 在运行时标识出非包含实体时将发生此 XEvent。 这包括源自客户端代码的实体。 此 XEvent 仅针对实际的非包含实体发生。 但是，该事件仅在运行时发生。 因此，此 XEvent 将不能识别您尚未运行的任何非包含用户实体。  
  
## <a name="see-also"></a>另请参阅  
 [经过修改的功能（包含的数据库）](../../relational-databases/databases/modified-features-contained-database.md)   
 [包含数据库的排序规则](../../relational-databases/databases/contained-database-collations.md)   
 [针对包含数据库的安全性最佳方法](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [包含的数据库用户 - 使数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)  
  
  
