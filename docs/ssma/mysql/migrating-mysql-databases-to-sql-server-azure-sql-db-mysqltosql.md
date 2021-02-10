---
title: 将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 |Microsoft Docs
description: 使用此推荐的过程，使用 SQL Server 迁移助手 (SSMA) 将 MySQL 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8006f9a0-394d-4238-8dc5-44255134628b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f4599389fd313447a926cfbc898735fc60f4823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014647"
---
# <a name="migrating-mysql-databases-to-sql-server---azure-sql-database-mysqltosql"></a>将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 (MySQLToSql) 
MySQL SQL Server 迁移助手 (SSMA) 是一个全面的环境，可帮助你快速将 MySQL 数据库迁移到 SQL Server 或 SQL Azure。 通过使用 SSMA for MySQL，你可以查看数据库对象和数据、评估要迁移的数据库、将数据库对象迁移到 SQL Server 或 SQL Azure，然后将数据迁移到 SQL Server 或 SQL Azure。  
  
## <a name="recommended-migration-process"></a>建议的迁移过程  
若要成功地将对象和数据从 MySQL 数据库迁移到 SQL Server 或 SQL Azure，请使用以下过程：  
  
1.  使用[&#40;MySQLToSQL&#41;的 SSMA 项目](../../ssma/mysql/working-with-ssma-projects-mysqltosql.md)。  
  
    创建项目后，可以设置项目转换、迁移和类型映射选项。 有关项目设置的详细信息，请参阅 [设置项目选项 &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md)。 有关如何自定义数据类型映射的信息，请参阅 [&#40;MySQLToSQL 映射 MySQL 和 SQL Server 数据类型&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
2.  [连接到 MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
3.  [连接到 SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
4.  [将 MySQL 数据库映射到 SQL Server 架构 &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
5.  [连接到 Azure SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
  
6.  （可选） [评估用于转换 &#40;MySQLToSQL&#41;的 MySQL 数据库 ](../../ssma/mysql/assessing-mysql-databases-for-conversion-mysqltosql.md) 以评估用于转换的数据库对象并估计转换时间。  
  
7.  [&#40;MySQLToSQL&#41;转换 MySQL 数据库 ](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
8.  [同步](loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
9. 可通过以下方式之一执行此操作：  
  
    -   保存并在 SQL Server 或 SQL Azure 上运行脚本。  
  
    -   同步数据库对象。  
  
10. [将 MySQL 数据迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
11. 如有必要，请更新数据库应用程序。  
  
> [!NOTE]  
> 不能迁移 Information_schema 和 MySQL 架构。  
  
## <a name="see-also"></a>另请参阅  
[安装 SSMA for MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
[与 SSMA for MySQL &#40;MySQLToSQL&#41;入门 ](../../ssma/mysql/getting-started-with-ssma-for-mysql-mysqltosql.md)  
  
