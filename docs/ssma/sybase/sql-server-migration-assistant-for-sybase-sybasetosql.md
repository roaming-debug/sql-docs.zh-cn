---
title: Sybase (SybaseToSQL) 的 SQL Server 迁移助手 |Microsoft Docs
description: 了解用于 Sybase 的 SSMA，并按照逐步说明将 ASE 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 59e63eac-8a7e-4d54-be1c-0633a9bf510d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: d32f59e4f9c81236199203cfa063b7d26152986b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030745"
---
# <a name="sql-server-migration-assistant-for-sybase-sybasetosql"></a>Sybase (SybaseToSQL 的 SQL Server 迁移助手) 

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Sybase 自适应服务器企业 (ASE)  (SSMA) 迁移助手用于将 ASE 数据库迁移到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 和 linux 上的2012、2014、 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016、 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2017、windows 和 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] linux 上的2019或 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Azure SQL 数据库。 SSMA for Sybase 将 ASE 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库中创建这些对象，然后将数据从 ASE 迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 azure sql 数据库。
  
本文档介绍了如何为 Sybase SSMA，并提供了将 ASE 数据库迁移到或 Azure SQL 数据库的分步说明，并提供了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移后可能出现的问题的相关信息。 若要了解详细信息，请参阅以下文章。  
  
## <a name="contents"></a>目录  
  
|部分|说明|
|-----------|---------------|
|[Sybase &#40;SybaseToSQL 的 SSMA 中的新增功能&#41;](../../ssma/sybase/what-s-new-in-ssma-for-sybase-sybasetosql.md)|列出对 SSMA 版本的更改。|  
|[为 Sybase &#40;SybaseToSQL&#41;安装 SSMA ](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)|包含一些文章，这些文章提供了在运行实例的计算机上为 Sybase 客户端和所需组件安装 SSMA 的先决条件和说明 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|[入门用于 Sybase &#40;SybaseToSQL 的 SSMA&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)|介绍用户界面、项目和配置选项。|  
|[将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)|概述转换过程，并提供有关过程中每个步骤的详细信息。|  
|[用户界面参考 &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)|包含用于 Sybase 对话框的 SSMA 文档。|  
|[使用 SSMA for Sybase 控制台](working-with-ssma-for-sybase-console-sybasetosql.md)|包含有关 SSMA 控制台应用程序的文档。|  
|[获取用于 Sybase 帮助的 SSMA](../sql-server-migration-assistant.md)|提供有关获取其他帮助的信息。|