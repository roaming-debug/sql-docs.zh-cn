---
title: 将 Oracle 数据库迁移到 SQL Server (OracleToSQL) |Microsoft Docs
description: 使用此推荐的过程，使用 SQL Server 迁移助手 (SSMA) 将 Oracle 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 04/22/2018
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1d196dd6-4322-4c98-bb72-602c57d96134
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: df5bd55fb9c5490b8063712dbebe5328917e08b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063342"
---
# <a name="migrating-oracle-databases-to-sql-server-oracletosql"></a>将 Oracle 数据库迁移到 SQL Server (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oracle 迁移助手 (SSMA) 是一个全面的环境，可帮助你快速将 Oracle 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] AZURE SQL Database 或 Azure Synapse Analytics。 通过使用 SSMA for Oracle，你可以查看数据库对象和数据、评估要迁移的数据库、将数据库对象迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 、AZURE SQL 数据库或 Azure Synapse analytics，然后将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure sql Database 或 Azure Synapse 分析。 请注意，不能迁移系统和 Oracle 架构。
  
## <a name="recommended-migration-process"></a>建议的迁移过程  
若要成功地将对象和数据从 Oracle 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 、AZURE SQL 数据库或 Azure Synapse Analytics，请使用以下过程：
  
1.  [创建新的 SSMA 项目](working-with-ssma-projects-oracletosql.md)。  
  
    创建项目后，可以设置项目转换、迁移和类型映射选项。 有关项目设置的信息，请参阅 [设置项目选项 &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md)。 有关如何自定义数据类型映射的信息，请参阅 [&#40;OracleToSQL&#41;映射 Oracle 和 SQL Server 数据类型 ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md)。  
  
2.  [连接到 Oracle 数据库服务器](connecting-to-oracle-database-oracletosql.md)。  
  
3.  [连接到 SQL Server 的实例](connecting-to-sql-server-oracletosql.md)。  
  
4.  [将 Oracle 数据库架构映射到 SQL Server 数据库架构](mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md)。  
  
5.  （可选） [创建评估报表](assessing-oracle-schemas-for-conversion-oracletosql.md) 以评估用于转换的数据库对象，并估计转换时间。  
  
6.  [将 Oracle 数据库架构转换为 SQL Server 架构](converting-oracle-schemas-oracletosql.md)。  
  
7.  [将转换后的数据库对象加载到 SQL Server 中](loading-converted-database-objects-into-sql-server-oracletosql.md)。  
  
    可通过以下方式之一执行此操作：  
  
    -   保存并在中运行脚本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
    -   同步数据库对象。  
  
8.  [将数据迁移到 SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md)。  
  
9. 如有必要，请更新数据库应用程序。  
  
## <a name="see-also"></a>另请参阅  
[安装 SSMA for Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md)  
[SSMA for Oracle &#40;OracleToSQL&#41;的入门 ](../../ssma/oracle/getting-started-with-ssma-for-oracle-oracletosql.md)  
  
