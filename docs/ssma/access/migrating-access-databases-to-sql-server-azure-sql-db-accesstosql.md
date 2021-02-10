---
title: 将 Access 数据库迁移到 SQL Server-Azure SQL 数据库 |Microsoft Docs
description: 使用此推荐的过程，使用 SQL Server 迁移助手 (SSMA) 将 Access 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instructions, migration
- migrating databases, overview
- overview, migration process
- procedure, migration
- recommended migration process
ms.assetid: 76a3abcf-2998-4712-9490-fe8d872c89ca
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 683c17a23b1eda96c040dabcf62d58b24fbb53ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066692"
---
# <a name="migrating-access-databases-to-sql-server---azure-sql-database-accesstosql"></a>将 Access 数据库迁移到 SQL Server-Azure SQL 数据库 (AccessToSQL) 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手 (SSMA) 是一种提供综合性环境的工具，可帮助你快速将 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 通过使用 SSMA，你可以查看 Access 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库对象、评估用于迁移的 access 数据库、转换 access 数据库对象、将其加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，以及迁移数据。  
  
## <a name="recommended-migration-process"></a>建议的迁移过程  
若要成功地将对象和数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的访问，请使用以下过程：  
  
1.  [创建新的 SSMA 项目](creating-and-managing-projects-accesstosql.md)。 创建项目后，可以 [设置项目选项](setting-conversion-and-migration-options-accesstosql.md)，其中包括转换选项、迁移选项和数据类型映射。  
  
2.  [将 Access 数据库文件添加](adding-and-removing-access-database-files-accesstosql.md) 到项目。  
  
    您可以添加单个文件，包括您在计算机或网络中找到的文件。  
  
3.  [连接到 SQL Server 的目标实例](connecting-to-sql-server-accesstosql.md) 或 [连接到 SQL Azure 的目标实例](connecting-to-azure-sql-db-accesstosql.md)。  
  
    可以连接到 SQL Server 或 SQL Azure。  
  
4.  若要自定义一个或多个 Access 数据库之间的映射， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 架构，请  [映射源和目标数据库](mapping-source-and-target-databases-accesstosql.md)。  
  
5.  或者，您可以 [创建一个评估报表](assessing-access-database-objects-for-conversion-accesstosql.md) 来确定 Access 数据库对象是否可以成功转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。  
  
6.  [将 Access 数据库对象转换](converting-access-database-objects-accesstosql.md) 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 对象定义。  
  
7.  [将转换后的数据库对象加载到 SQL Server 中](loading-converted-database-objects-into-sql-server-accesstosql.md)。  
  
    您可以使用 SSMA 将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 中，也可以保存 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。  
  
8.  将[Access 数据迁移到 SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md)。  
  
    > [!NOTE]  
    > 您可以通过一个步骤转换、加载和迁移架构和数据。 若要执行一键式迁移，请单击 " **转换、加载和迁移** " 按钮。  
  
9. 如果希望 Access 应用程序使用或 SQL Azure 中的数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请使用将 [Access 表链接到 SQL Server 表](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)。  
  
你还可以使用迁移向导来指导你完成此过程。 有关详细信息，请参阅 [迁移向导](migration-wizard-accesstosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[使用 SQL Server 迁移助手进行访问的入门](getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[准备要迁移的 Access 数据库](preparing-access-databases-for-migration-accesstosql.md)
