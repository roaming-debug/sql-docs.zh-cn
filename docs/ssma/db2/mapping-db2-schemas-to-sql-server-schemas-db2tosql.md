---
description: '将 DB2 架构映射到 SQL Server 架构 (DB2ToSQL) '
title: 将 DB2 架构映射到 SQL Server 架构 (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 05ff7bd4-e60b-4f48-a893-bc2346aa9a8a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9942d2ee78932c3bb8bed2baac0885b68e40049d
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94869536"
---
# <a name="mapping-db2-schemas-to-sql-server-schemas-db2tosql"></a>将 DB2 架构映射到 SQL Server 架构 (DB2ToSQL) 
在 DB2 中，每个数据库都有一个或多个架构。 默认情况下，SSMA 将 DB2 架构中的所有对象迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 名为的数据库的架构。 但是，你可以自定义 DB2 架构和数据库之间的映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="db2-and-sql-server-schemas"></a>DB2 和 SQL Server 架构  
DB2 数据库包含架构。 的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包含多个数据库，每个数据库都可以有多个架构。  
  
架构的 DB2 概念映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的概念及其架构之一。 例如，DB2 可能有一个名为 **HR** 的架构。 的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能有一个名为 **HR** 的数据库，该数据库中的数据库是架构。 一个架构是 **dbo** (或数据库所有者) 架构。 默认情况下，DB2 架构 **hr** 将映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库和架构 " **hr. dbo**"。 SSMA 是指 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为架构的数据库和架构的组合。  
  
您可以修改 DB2 和架构之间的映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="modifying-the-target-database-and-schema"></a>修改目标数据库和架构  
在 SSMA 中，可以将 DB2 架构映射到任何可用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构。  
  
**修改数据库和架构**  
  
1.  在 DB2 元数据资源管理器中，选择 " **架构**"。  
  
    选择单个数据库、"**架构**" 文件夹或单独的架构时，还可以使用 "**架构映射**" 选项卡。 为所选对象自定义 " **架构映射** " 选项卡中的列表。  
  
2.  在右侧窗格中，单击 " **架构映射** " 选项卡。  
  
    你将看到所有 DB2 架构的列表，后跟目标值。 此目标以两部分表示法表示， (*database. schema*) 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 其中迁移对象和数据的位置。  
  
3.  选择包含要更改的映射的行，然后单击 " **修改**"。  
  
    在 " **选择目标架构** " 对话框中，您可以浏览可用目标数据库和架构，或在两部分表示形式的文本框中输入数据库和架构名称 (database. Schema) ，然后单击 **"确定"**。  
  
4.  " **架构映射** " 选项卡上的目标更改。  
  
**映射模式**  
  
-   映射到 SQL Server  
  
您可以将源数据库映射到任何目标数据库。 默认情况下，源数据库映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 SSMA 连接的目标数据库。 如果要映射的目标数据库在上不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存在，则系统会提示你提供消息 **"目标元数据中不存在数据库和/或架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。它将在同步过程中创建。是否要继续？ "** 单击“是”。 同样，你可以将架构映射到目标数据库下将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 同步过程中创建的非现有架构。  
  
## <a name="reverting-to-the-default-database-and-schema"></a>恢复到默认数据库和架构  
如果自定义 DB2 架构和架构之间的映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则可以将映射恢复为默认值。  
  
**恢复为默认数据库和架构**  
  
1.  在 "架构映射" 选项卡下，选择任意行，然后单击 " **重置为默认值** " 以还原为默认数据库和架构。  
  
## <a name="next-steps"></a>后续步骤  
如果要分析 DB2 对象到对象的转换 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以 [ (SSMA Common) 数据迁移报表 ](../sybase/data-migration-report-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[连接到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
[将 DB2 数据库迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
