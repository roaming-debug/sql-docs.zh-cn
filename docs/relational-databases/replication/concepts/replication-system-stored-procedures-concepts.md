---
description: Replication System Stored Procedures Concepts
title: 复制系统存储过程概念 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server replication], programming
- programming [SQL Server replication], system stored procedures
- programming interfaces [SQL Server replication]
- system stored procedures [SQL Server replication]
- replication [SQL Server], how-to topics
ms.assetid: 816d2bda-ed72-43ec-aa4d-7ee3dc25fd8a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eb082b29411b136edca7ed14f06effa354a5f0e6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643083"
---
# <a name="replication-system-stored-procedures-concepts"></a>Replication System Stored Procedures Concepts
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，对复制拓扑中所有用户可配置的功能的编程访问是由系统存储过程提供的。 虽然可使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 sqlcmd 命令行实用工具来单独执行存储过程，但编写可执行具有一定逻辑顺序的复制任务的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本文件也有诸多好处。  
  
 编写复制任务脚本具备以下优点：  
  
-   保存用于部署复制拓扑的步骤的永久副本。  
  
-   使用单个脚本配置多个订阅服务器。  
  
-   通过让新数据库管理员评估、了解、更改代码以及解决代码相关问题来快速培训新数据库管理员。  
  
    > [!IMPORTANT]  
    >  脚本可能会带来安全漏洞；它们可以在用户毫不知情或无用户干预的情况下调用系统函数，并且可能包含纯文本形式的安全凭据。 请在使用之前检查脚本是否存在安全问题。  
  
## <a name="creating-replication-scripts"></a>创建复制脚本  
 从复制的角度来看，脚本是单个或一系列 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句，其中每个语句执行一个复制存储过程。 脚本为文本文件，文件扩展名通常为 .sql，可使用 sqlcmd 实用工具来运行。 运行脚本文件时，此实用工具将执行存储在该文件中的 SQL 语句。 相似地，脚本可在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 项目中作为查询对象来存储。  
  
 可以通过下列方式创建复制脚本：  
  
-   手动创建脚本。  
  
-   使用复制向导或  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
-   使用复制管理对象 (RMO) 以编程方式生成脚本，从而创建 RMO 对象。  
  
 手动创建复制脚本时，请注意以下事项：  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本包含一个或多个批处理。 GO 命令表示一个批处理的结束。 如果 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本中没有 GO 命令，则该脚本将作为单个批处理来执行。  
  
-   在单个批处理中执行多个复制存储过程时，批处理中第一个存储过程后的所有存储过程前面都要有 EXECUTE 关键字。  
  
-   在批处理执行前，该批处理中的所有存储过程都必须先编译。 即便如此，编译完批处理并创建了执行计划后，仍有可能出现运行时错误。  
  
-   创建脚本以配置复制时，应使用 Windows 身份验证，以避免在脚本文件中存储安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
## <a name="sample-replication-script"></a>复制脚本示例  
 执行下面的脚本可在服务器上设置发布和分发。  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
 然后，可将此脚本在本地保存为 `instdistpub.sql`，以便在需要时可运行或再次运行此脚本。  
  
 上面的脚本包含 **sqlcmd** 脚本变量，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书中的许多复制代码示例中也使用了这些变量。 脚本变量是使用 `$(MyVariable)` 语法定义的。 可在命令行或在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中将脚本变量的值传递给脚本。 有关详细信息，请参阅本主题中的下一部分“执行复制脚本”。  
  
## <a name="executing-replication-scripts"></a>执行复制脚本  
 创建完复制脚本后，可以通过下列方式之一执行所创建的复制脚本：  
  
### <a name="creating-a-sql-query-file-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中创建一个 SQL 查询文件  
 可以将复制 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本文件创建为 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 项目中的 SQL 查询文件。 编写完脚本后，可以为此查询文件创建一个与数据库的连接，然后即可执行该脚本。 有关如何使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 创建 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本的详细信息，请参阅[查询和文本编辑器 &#40;SQL Server Management Studio&#41;](../../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md)。  
  
 若要使用包含脚本变量的脚本，[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 必须在 **sqlcmd** 模式下运行。 在 **sqlcmd** 模式下，查询编辑器可接受特定于 **sqlcmd** 的附加语法，如可用于变量值的 `:setvar`。 有关 **sqlcmd** 模式的详细信息，请参阅 [使用查询编辑器编辑 SQLCMD 脚本](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md)。 在下面的脚本中，`:setvar` 用于为 `$(DistPubServer)` 变量提供值。  
  
```  
:setvar DistPubServer N'MyPublisherAndDistributor';  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
--  
-- Additional code goes here  
--  
```  
  
### <a name="using-the-sqlcmd-utility-from-the-command-line"></a>从命令行使用 sqlcmd 实用工具  
 下面的示例说明如何在命令行中使用 [sqlcmd 实用工具](../../../tools/sqlcmd-utility.md)来执行 `instdistpub.sql` 脚本文件：  
  
```  
sqlcmd.exe -E -S sqlserverinstance -i C:\instdistpub.sql -o C:\output.log -v DistPubServer="N'MyDistributorAndPublisher'"  
```  
  
 在此示例中，`-E` 开关指示连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时使用 Windows 身份验证。 使用 Windows 身份验证时，不需要在脚本文件中存储用户名和密码。 脚本文件的名称和路径是通过 `-i` 开关指定的，输出文件的名称是通过 `-o` 开关指定的（使用此开关时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的输出将写入此文件，而不是控制台）。 在 `sqlcmd` 实用工具中使用 `-v` 开关，可以在运行时向 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本传递脚本变量。 在此示例中，`sqlcmd` 在执行前将脚本中的每个 `$(DistPubServer)` 实例替换为值 `N'MyDistributorAndPublisher'`。  
  
> [!NOTE]  
>  `-X` 开关可禁用脚本变量。  
  
### <a name="automating-tasks-in-a-batch-file"></a>自动执行批处理文件中的任务  
 通过使用批处理文件，可在同一批处理文件中实现复制管理任务、复制同步任务以及其他任务的自动执行。 下面的批处理文件使用 **sqlcmd** 实用工具删除并重新创建订阅数据库，并添加一个合并请求订阅。 然后，该文件调用合并代理来同步新订阅：  
  
```  
REM ----------------------Script to synchronize merge subscription ----------------------  
REM -- Creates subscription database and   
REM -- synchronizes the subscription to MergeSalesPerson.  
REM -- Current computer acts as both Publisher and Subscriber.  
REM -------------------------------------------------------------------------------------  
  
SET Publisher=%computername%  
SET Subscriber=%computername%  
SET PubDb=AdventureWorks  
SET SubDb=AdventureWorksReplica  
SET PubName=AdvWorksSalesOrdersMerge  
  
REM -- Drop and recreate the subscription database at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE master IF EXISTS (SELECT * FROM sysdatabases WHERE name='%SubDb%' ) DROP DATABASE %SubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE master CREATE DATABASE %SubDb%"  
  
REM -- Add a pull subscription at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb% EXEC sp_addmergepullsubscription @publisher = %Publisher%, @publication = %PubName%, @publisher_db = %PubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb%  EXEC sp_addmergepullsubscription_agent @publisher = %Publisher%, @publisher_db = %PubDb%, @publication = %PubName%, @subscriber = %Subscriber%, @subscriber_db = %SubDb%, @distributor = %Publisher%"  
  
REM -- This batch file starts the merge agent at the Subscriber to   
REM -- synchronize a pull subscription to a merge publication.  
REM -- The following must be supplied on one line.  
"\Program Files\Microsoft SQL Server\130\COM\REPLMERG.EXE"  -Publisher  %Publisher% -Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB  %PubDb% -SubscriberDB %SubDb% -Publication %PubName% -PublisherSecurityMode 1 -OutputVerboseLevel 1  -Output  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1 -Validate 3  
  
```  
  
## <a name="scripting-common-replication-tasks"></a>编写常见复制任务脚本  
 下面是一些可使用系统存储过程来编写脚本的最常见复制任务：  
  
-   配置发布和分发  
  
-   修改发布服务器和分发服务器属性  
  
-   禁用发布和分发  
  
-   创建发布及定义项目  
  
-   删除发布和项目  
  
-   创建请求订阅  
  
-   修改请求订阅  
  
-   删除请求订阅  
  
-   创建推送订阅  
  
-   修改推送订阅  
  
-   删除推送订阅  
  
-   同步请求订阅  
  
## <a name="see-also"></a>另请参阅  
 [复制编程概念](../../../relational-databases/replication/concepts/replication-programming-concepts.md)   
 [复制存储过程 &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [编写复制脚本](../../../relational-databases/replication/scripting-replication.md)  
  
