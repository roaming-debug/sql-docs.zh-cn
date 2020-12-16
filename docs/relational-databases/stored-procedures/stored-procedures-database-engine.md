---
title: 存储过程（数据库引擎）| Microsoft Docs
description: 了解以下内容：SQL Server 中的存储过程是由一个或多个 Transact-SQL 语句或对 .NET Framework 公共语言运行时方法的引用构成的一个组。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- storing programs as stored procedures
- stored procedures [SQL Server], about stored procedures
ms.assetid: cc6daf62-9663-4c3e-950a-ab42e2830427
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: be8431723e70b85f006f24aa81f8ba51bec8360f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467048"
---
# <a name="stored-procedures-database-engine"></a>存储过程（数据库引擎）
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的存储过程是由一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或对 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 方法的引用所构成的一个组。 过程与其他编程语言中的构造相似，这是因为它们都可以：  
  
-   接受输入参数并以输出参数的格式向调用程序返回多个值。  
  
-   包含用于在数据库中执行操作的编程语句。 这包括调用其他过程。  
  
-   向调用程序返回状态值，以指明成功或失败（以及失败的原因）。  
  
## <a name="benefits-of-using-stored-procedures"></a>使用存储过程的好处  
 下表介绍了使用过程的一些好处。  
  
 减少了服务器/客户端网络流量  
 过程中的命令作为代码的单个批处理执行。 这可以显著减少服务器和客户端之间的网络流量，因为只有对执行过程的调用才会跨网络发送。 如果没有过程提供的代码封装，每个单独的代码行都不得不跨网络发送。  
  
 更强的安全性  
 多个用户和客户端程序可以通过过程对基础数据库对象执行操作，即使用户和程序对这些基础对象没有直接权限。 过程控制执行哪些进程和活动，并且保护基础数据库对象。 这消除在了单独的对象级别授予权限的要求，并且简化了安全层。  
  
 可在 CREATE PROCEDURE 语句中指定 [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) 子句以便实现对其他用户的模拟，或者使用户或应用程序无需针对基础对象和命令的直接权限，即可执行某些数据库活动。 例如，某些操作（如 TRUNCATE TABLE）没有可授予的权限。 若要执行 TRUNCATE TABLE，用户必须对指定表具有 ALTER 权限。 授予用户对表的 ALTER 权限可能不是最佳方法，因为用户将拥有超出截断表的能力的权限。 通过将 TRUNCATE TABLE 语句纳入模块中并指定该模块作为一个有权修改表的用户执行，您可以将截断表的权限扩展至授予其对模块的 EXECUTE 权限的用户。  
  
 在通过网络调用过程时，只有对执行过程的调用是可见的。 因此，恶意用户无法看到表和数据库对象名称、嵌入自己的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或搜索关键数据。  
  
 使用过程参数有助于避免 SQL 注入攻击。 因为参数输入被视作文字值而非可执行代码，所以，攻击者将命令插入过程内的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句并损害安全性将更为困难。  
  
 可以对过程进行加密，这有助于对源代码进行模糊处理。 有关详细信息，请参阅 [SQL Server Encryption](../../relational-databases/security/encryption/sql-server-encryption.md)。  
  
 代码的重复使用  
 任何重复的数据库操作的代码都非常适合于在过程中进行封装。 这消除了不必要地重复编写相同的代码、降低了代码不一致性，并且允许拥有所需权限的任何用户或应用程序访问和执行代码。  
  
 更容易维护  
 在客户端应用程序调用过程并且将数据库操作保持在数据层中时，对于基础数据库中的任何更改，只有过程是必须更新的。 应用程序层保持独立，并且不必知道对数据库布局、关系或进程的任何更改的情况。  
  
 提高了性能  
 默认情况下，在首次执行过程时将编译过程，并且创建一个执行计划，供以后的执行重复使用。 因为查询处理器不必创建新计划，所以，它通常用更少的时间来处理过程。  
  
 如果过程引用的表或数据有显著变化，则预编译的计划可能实际上会导致过程的执行速度减慢。 在此情况下，重新编译过程和强制新的执行计划可提高性能。  
  
## <a name="types-of-stored-procedures"></a>存储过程的类型  

 **用户定义**  
 用户定义的过程可在用户定义的数据库中创建，或者在除了 **Resource** 数据库之外的所有系统数据库中创建。 该过程可在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中开发，或者作为对 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 方法的引用开发。  
  
 **临时**  
 临时过程是用户定义过程的一种形式。 临时过程与永久过程相似，只是临时过程存储于 **tempdb** 中。 临时过程有两种类型：本地过程和全局过程。 它们在名称、可见性以及可用性上有区别。 本地临时过程的名称以单个数字符号 (#) 开头；它们仅对当前的用户连接是可见的；当用户关闭连接时被删除。 全局临时过程的名称以两个数字符号 (##) 开头，创建后对任何用户都是可见的，并且在使用该过程的最后一个会话结束时被删除。  
  
 **系统**  
 系统过程是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]随附的。 它们物理上存储在内部隐藏的 **Resource** 数据库中，但逻辑上出现在每个系统定义数据库和用户定义数据库的 **sys** 架构中。 此外， **msdb** 数据库还在 **dbo** 架构中包含用于计划警报和作业的系统存储过程。 因为系统过程以前缀 **sp_** 开头，所以，我们建议你在命名用户定义过程时不要使用此前缀。 有关系统过程的完整列表，请参阅[系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和外部程序之间提供一个接口以实现各种维护活动的系统过程。 这些扩展过程使用 xp_ 前缀。 有关扩展过程的完整列表，请参阅[常规扩展存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)。  
  
 **扩展的用户定义**  
 扩展过程允许你使用编程语言（例如 C）创建外部例程。这些过程是指 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例可以动态加载和运行的 DLL。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的未来版本中将删除扩展存储过程。 请不要在新的开发工作中使用该功能，并尽快修改当前还在使用该功能的应用程序。 请改为创建 CLR 过程。 此方法提供了更为可靠和安全的替代方法来编写扩展过程。  
  
## <a name="related-tasks"></a>Related Tasks  
  
| 任务说明 | 主题 |
| ---------------- | ----- |
|介绍如何创建存储过程。|[创建存储过程](../../relational-databases/stored-procedures/create-a-stored-procedure.md)|  
|介绍如何修改存储过程。|[修改存储过程](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)|  
|介绍如何删除存储过程。|[删除存储过程](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)|  
|介绍如何执行存储过程。|[执行存储过程](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)|  
|介绍如何授予对存储过程的权限。|[授予对存储过程的权限](../../relational-databases/stored-procedures/grant-permissions-on-a-stored-procedure.md)|  
|介绍如何将来自存储过程的数据返回到应用程序。|[从存储过程中返回数据](../../relational-databases/stored-procedures/return-data-from-a-stored-procedure.md)|  
|介绍如何重新编译存储过程。|[重新编译存储过程](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md)|  
|介绍如何重命名存储过程。|[重命名存储过程](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)|  
|介绍如何查看存储过程的定义。|[查看存储过程的定义](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)|  
|介绍如何查看存储过程的依赖关系。|[查看存储过程的依赖关系](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)|  
|介绍如何在存储过程中使用参数。|[参数](../../relational-databases/stored-procedures/parameters.md)|  
  
## <a name="related-content"></a>相关内容  
 [CLR 存储过程](/dotnet/framework/data/adonet/sql/clr-stored-procedures)  
 [延迟名称解析](../../t-sql/statements/create-trigger-transact-sql.md#deferred-name-resolution)
