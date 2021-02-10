---
title: 映射源和目标数据库 (AccessToSQL) |Microsoft Docs
description: 了解如何指定用于迁移到 SQL Server 或 Azure SQL 数据库的目标数据库，包括多个数据库的多个数据库。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- database schemas
- mapping, databases
- schemas, mapping to
- schemas, SQL Azure
- schemas, SQL Server
- source database
- target database
ms.assetid: 69bee937-7b2c-49ee-8866-7518c683fad4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 966ec1472737f0e4d67615d4e7eb65df01a52cf4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059038"
---
# <a name="mapping-source-and-target-databases-accesstosql"></a>映射源和目标数据库 (AccessToSQL) 
当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 时，需要为迁移指定目标数据库。 如果有多个访问数据库，则可以将它们映射到多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库 (或架构) 或连接的 AZURE SQL 数据库下的多个架构。  
  
## <a name="sql-server-or-azure-sql-database-schemas"></a>SQL Server 或 Azure SQL 数据库架构  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库使用架构的概念将数据库中的对象分为多个逻辑组。 例如，库数据库可以使用三个名为 **书籍**、 **音频** 和 **视频** 的架构来分隔书籍、音频和视频对象。 默认情况下，access 数据库将映射到 **master** 数据库，将中的 **dbo** 架构映射到 SQL Azure 中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接数据库和 **dbo** 架构。  
  
除非自定义每个 Access 数据库和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库和架构之间的映射，否则，SSMA 会将与 Access 数据库相关联的所有架构和数据迁移到映射的默认数据库。  
  
## <a name="modifying-the-target-database-and-schema"></a>修改目标数据库和架构  
SSMA 使你可以将每个访问数据库映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 下面的过程介绍如何自定义每个数据库的映射。  
  
**修改目标数据库和架构**  
  
1.  在 "访问元数据资源管理器" 窗格中，选择 " **访问元数据**"。  
  
    在选择 " **数据库** " 节点或任意数据库节点时，还可以使用架构映射。 已为所选对象自定义架构映射列表。  
  
2.  在右侧窗格中，单击 " **架构映射** " 选项卡。  
  
    你将看到一个表，其中包含访问数据库名称及其对应的 ssNoVersion 或 Sql Azure 架构。 目标架构以两部分表示法表示， (database. schema) 。  
  
3.  选择包含要自定义的映射的行，然后单击 " **修改**"。  
  
4.  在 " **选择目标架构** " 对话框中，您可以浏览可用目标数据库和架构，或在两部分表示形式的文本框中输入数据库和架构名称 (database. Schema) ，然后单击 **"确定"**。  
  
**映射模式**  
  
-   映射到 SQL Server  
  
您可以将源数据库映射到任何目标数据库。 默认情况下，源数据库映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 SSMA 连接的目标数据库。 如果要映射的目标数据库在上不存在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则系统会提示您提供消息 **"目标元数据中不存在数据库和/或架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。它将在同步过程中创建。是否要继续？ "** 单击“是”。 同样，你可以将架构映射到目标数据库下将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 同步过程中创建的非现有架构。  
  
-   映射到 SQL Azure  
  
您可以将源数据库映射到连接目标数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或连接的目标数据库中的任何架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果将源架构映射到已连接目标数据库下的任何非现有架构，则系统会提示您输入消息 **"目标元数据中不存在架构"。它将在同步过程中创建。是否要继续？"** 单击" 是 "。  
  
## <a name="reverting-to-your-initial-database-and-schema"></a>恢复到初始数据库和架构  
如果自定义 Access 数据库和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库之间的映射，则可以将映射还原到连接到或 SQL Azure 时指定的数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
**重置为默认数据库和架构**  
  
1.  在 "架构映射" 选项卡下，选择任意行，然后单击 " **重置为默认值** " 以还原为默认数据库和架构。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [转换数据库对象](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
