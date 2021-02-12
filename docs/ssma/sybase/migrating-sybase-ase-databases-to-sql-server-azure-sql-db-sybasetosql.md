---
title: 将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库 |Microsoft Docs
description: 使用此推荐的过程，使用 SQL Server 迁移助手 (SSMA) 将 SAP 自适应服务器企业数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.custom: ''
ms.date: 11/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ed7952d4-8331-44d7-bccf-3440e17238b2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 222ce0d6d9a91a0aa97f9dc29dee06fe01754a2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074558"
---
# <a name="migrating-sap-ase-databases-to-sql-server---azure-sql-database-sybasetosql"></a>将 SAP ASE 数据库迁移到 SQL Server-Azure SQL 数据库 (SybaseToSQL) 
SAP 自适应服务器企业 (ASE)  (SSMA) 是一个全面的环境，可帮助你快速将 SAP ASE 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 SQL Server 迁移助手 通过使用 SSMA for SAP ASE，你可以查看数据库对象和数据、评估要迁移的数据库、将数据库对象迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库，以及将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库。  
  
## <a name="recommended-migration-process"></a>建议的迁移过程  
若要成功地将对象和数据从 SAP ASE 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库，请使用以下过程：  
  
1.  [创建新的 SSMA 项目](working-with-ssma-projects-sybasetosql.md)。  
  
    创建项目后，可以设置项目转换、迁移和类型映射选项。 有关项目设置的信息，请参阅 [设置项目选项 &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md)。 有关自定义数据类型映射的信息，请参阅将 [SYBASE ASE 和 SQL Server 数据类型映射 &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md)。  
  
2.  [连接到 SAP ASE 数据库服务器](connecting-to-sybase-ase-sybasetosql.md)。  
  
3.  [连接到实例 SQL Server](connecting-to-sql-server-sybasetosql.md) 或 [连接到 Azure SQL 数据库的实例](connecting-to-azure-sql-db-sybasetosql.md)。  
  
4.  [将 SAP ASE 数据库架构映射到 SQL Server/AZURE SQL 数据库数据库架构](./mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md)。  
  
5.  （可选） [创建评估报表](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md) 以评估用于转换的数据库对象，并估计转换时间。  
  
6.  [将 SAP ASE 数据库架构转换为 SQL Server/AZURE SQL 数据库架构](./converting-sybase-ase-database-objects-sybasetosql.md)。  
  
7.  [将转换后的数据库对象加载到 SQL Server/AZURE SQL 数据库中](./loading-converted-database-objects-into-sql-server-sybasetosql.md)。  
  
    保存脚本并在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库中运行该脚本，或者同步数据库对象。  
  
8.  [将数据迁移到 SQL Server/AZURE SQL 数据库](./migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md)。  
  
9. 如有必要，请更新数据库应用程序。  
  
## <a name="see-also"></a>另请参阅  
[安装 SSMA for SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)  
[SSMA for SAP ASE &#40;SybaseToSQL&#41;的入门 ](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)  
