---
title: 使用命令行工具开发面向项目的数据库
description: 查看 SQL Server Data Tools 提供的用于处理 .dacpac 文件的命令行工具的可用资源（例如 SQLPackage.exe 和 dbSqlPackage）。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9a26def9-8fbd-43e4-9e57-414840b73ed8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: 1439673e40faf8da8cf65da673f0145374b48abd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081888"
---
# <a name="project-oriented-database-development-using-command-line-tools"></a>使用命令行工具开发面向项目的数据库

SQL Server Data Tools 经证明是支持多种面向项目的数据库开发方案的命令行工具。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|描述|  
|-|-|  
|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)|本主题介绍了用于以下任务的 SQLPackage.exe 实用工具：<br /><br />-   从活动的 SQL Server 数据库中提取 .dacpac 文件。<br />-   将 .dacpac 文件发布到活动的 SQL Server 数据库，以便增量更新活动数据库架构以匹配 .dacpac。<br />-   将 .dacpac 文件与活动的 SQL Server 数据库进行比较，并且在不更新活动数据库的情况下生成增量升级 Transact\-SQL 脚本。<br />-   将两个 .dacpac 文件进行比较以便生成增量升级 Transact\-SQL 脚本。<br />-   生成一个 XML 报表，该报表汇总了在增量升级数据库时将发生的增量升级更改。|  
|[将 MSDeploy 用于 dbSqlPackage 提供程序](../ssdt/using-msdeploy-with-dbsqlpackage-provider.md)|本主题介绍名为 dbSqlPackage 的 [Web 部署工具](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd568996(v=ws.10))提供程序，它随 SSDT 一起提供，与 Microsoft Internet Information Services (IIS) Web 部署工具 (MSDeploy.exe) 结合使用可执行以下任务：<br /><br />-   从远程/本地 SQL Server 或 Azure SQL 数据库中提取 .dacpac 文件。<br />-   将 .dacpac 发布到远程/本地 SQL Server 或 Azure SQL 数据库以对其进行增量升级。<br />-   从本地 SQL Server 数据库发布到远程 SQL Server 或 Azure SQL 数据库，以便增量升级远程数据库。<br />-   将 .dacpac 与远程/本地 SQL Server 或  Azure SQL  数据库进行比较，以便在不更新活动数据库的情况下生成增量升级 Transact\-SQL 脚本。<br />-   生成一个 XML 报表，该报表汇总了在增量升级数据库时将发生的增量升级更改。|  
  
## <a name="related-sections"></a>相关章节  
[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)  
