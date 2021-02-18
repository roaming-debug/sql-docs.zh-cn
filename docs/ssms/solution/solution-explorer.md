---
description: 解决方案资源管理器
title: 解决方案资源管理器
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], solutions
- projects [SQL Server Management Studio], about projects
- SQL Server Management Studio [SQL Server], projects
- solutions [SQL Server Management Studio], about solutions
- SQL Server Management Studio [SQL Server], items
- items [SQL Server]
ms.assetid: 0df09843-0d4f-4925-bc6c-99265035a0c1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 1ebf2ca3be5b92474c1f62052d1845cb3fb5b277
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349008"
---
# <a name="solution-explorer"></a>解决方案资源管理器

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“解决方案资源管理器”窗格提供了称为“项目”的容器，用于管理数据库脚本、查询、数据连接和文件等项。 一个或多个彼此相关联的项目可以组合在一个容器中（称为解决方案）。  
  
“解决方案”包含一个或多个项目，以及定义整个解决方案所需的文件和元数据。 “项目”是一组文件和相关的元数据（如连接信息）。 解决方案和项目所包含的“项”表示创建数据库解决方案所需的脚本、查询、连接信息和文件。  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="benefits-of-using-solutions"></a>使用解决方案的好处  
使用这些容器可以执行以下操作：  
  
-   对查询和脚本实施源代码管理。  
  
-   管理整个解决方案的设置或管理各个项目的设置。  
  
-   在您集中精力管理构成数据库解决方案的项的同时，处理文件管理的详细信息。  
  
-   向解决方案中的多个项目或向解决方案添加有用项，而不必在每个项目中都引用该项。  
  
-   对独立于解决方案或项目的杂项文件进行操作。  
  
项目中包含的项取决于项目类型以及您是否使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
## <a name="related-tasks"></a>Related Tasks  
使用以下主题开始使用 SQL Server 解决方案：  
  
|说明|主题|  
|-|-|    
|介绍如何在解决方案中收集一个或多个项目。|[解决方案 (SQL Server Management Studio)](../../ssms/solution/solutions-sql-server-management-studio.md)|  
|介绍如何创建项目并添加项（如脚本和连接）。|[项目 (SQL Server Management Studio)](../../ssms/solution/projects-sql-server-management-studio.md)|  
|提供有关 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 管理解决方案和文件所使用的文件的信息。|[用于管理解决方案和项目的文件](../../ssms/solution/files-that-manage-solutions-and-projects.md)|  
  
