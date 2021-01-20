---
title: SQL Server 迁移助手访问 (AccessToSQL) |Microsoft Docs
description: 了解 SSMA for Access，并按照逐步说明将 Access 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 10/10/2019
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 40c1eb02-26b2-44ba-969d-6c430c61c281
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 25e7694505f7f67702f476983c33120a87539887
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596425"
---
# <a name="sql-server-migration-assistant-for-access-accesstosql"></a>SQL Server 迁移助手访问 (AccessToSQL) 

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]迁移助手 (SSMA) 的访问权限是一种用于迁移数据库 [!INCLUDE[msCoName](../../includes/msconame_md.md)] 的工具[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]在 windows 和 linux 上访问版本 2010 97 到2012、2014、2016、2017、windows 和 linux 上的2019、 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] windows 和 linux 或 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Azure SQL 数据库。 SSMA for Access 将 Access 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE sql 数据库对象，将这些对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库，然后将数据从 Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 azure sql 数据库迁移到 azure sql database。
  
本文档介绍如何 SSMA 访问，并提供有关将 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的分步说明，并提供迁移后可能出现的问题的相关信息。  
  
## <a name="contents"></a>目录  
  
|部分|说明|
|-----------|---------------|
|[SSMA for Access 中的新增功能](./what-s-new-in-ssma-for-access-accesstosql.md)|列出对 SSMA 版本的更改。|  
|[安装访问 SQL Server 迁移助手](installing-sql-server-migration-assistant-for-access-accesstosql.md)|列出了安装 SSMA 的先决条件、安装和许可 SSMA 的过程，以及指向最新版本的链接。|  
|[使用 SQL Server 迁移助手入门访问 &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)|介绍 SSMA 及其用户界面。|  
|[准备要迁移的 Access 数据库](preparing-access-databases-for-migration-accesstosql.md)|描述如何准备要转换为/SQL Azure 的 Access 数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)|概述转换过程，并提供有关过程中每个步骤的详细信息。|  
|[将访问应用程序链接到 SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)|描述如何将现有的 Access 应用程序用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|[用户界面参考](user-interface-reference-accesstosql.md)|包含 SSMA 对话框的文档。|  
|[使用 SSMA for Access 控制台](working-with-ssma-for-access-console-accesstosql.md)|包含有关 SSMA 控制台应用程序的文档|  
|[获取访问的 SSMA 帮助](../sql-server-migration-assistant.md)|提供有关获取其他帮助的信息。|