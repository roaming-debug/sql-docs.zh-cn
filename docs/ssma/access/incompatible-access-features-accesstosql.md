---
title: AccessToSQL)  (不兼容的访问功能 |Microsoft Docs
description: 了解与 SQL Server 不兼容的 Access 数据库功能的可能迁移问题以及如何解决这些问题。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, features incompatible with SQL Azure
- Access databases, features incompatible with SQL Server
- dates
- default expressions
- foreign keys
- hyperlink columns
- incompatible Access features
- indexes
- indexes, length of
- keywords
- primary keys
- reference, incompatible features
- replication columns
- special characters
- unique indexes
- validation rules
ms.assetid: 99d45b9c-e3b9-4d56-8c25-b594b887ace1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 492add4bd73d81c8c25256e35ff1b3de37fc13de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044527"
---
# <a name="incompatible-access-features-accesstosql"></a>AccessToSQL)  (不兼容的访问功能
并非所有 Access 数据库功能都与兼容 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Access 具有不同的保留关键字集。 此类问题可能会阻止成功迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 使用下表来了解可能的迁移问题以及你可以执行的操作。  
  
## <a name="database-settings-or-features-that-might-affect-migration"></a>可能影响迁移的数据库设置或功能  
  
|Access 数据库设置或功能|迁移问题|  
|--------------------------------------|-------------------|  
|访问表没有唯一索引。|如果没有唯一索引的表迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则迁移后将无法修改该表。 这可能导致应用程序兼容性问题。<br /><br />转换 Access 数据库对象时，"输出" 窗口将列出不具有唯一索引的所有访问表。<br /><br />您可以配置访问权限，以便在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 转换过程中对表添加主键。 有关详细信息，请参阅 [项目设置 (转换) ](./project-settings-conversion-accesstosql.md)。|  
|Access 表具有复制列。|如果将包含复制系统列的访问表迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则迁移后 Jet 复制功能将中断。<br /><br />迁移之后，请考虑使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制来维护数据库的同步副本。|  
|具有唯一索引的访问表包含多个 null 值。|不能将具有多个 null 值的唯一索引的 Access 表传输到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，因为在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，唯一索引不允许使用多个 null 值。 对于这些表，迁移将失败。<br /><br />SSMA 会在评估报表中标记此问题。 若要创建评估报表，请参阅 [评估 Access 数据库对象的转换](assessing-access-database-objects-for-conversion-accesstosql.md)。<br /><br />如果存在此问题，则必须确保主键不具有重复的 null 值。 或者，必须删除包含多个 null 值的主键或唯一索引。|  
|访问表包含的日期值超出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 范围。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Datetime** 类型仅接受介于 1 Jan 1753 至 31 12 9999 月12日的日期。 访问接受范围为1月 1 100 日至31年 12 9999 月1日的日期。<br /><br />SSMA 会在评估报表中标记此问题。 若要创建评估报表，请参阅 [评估 Access 数据库对象的转换](assessing-access-database-objects-for-conversion-accesstosql.md)。<br /><br />可以配置 SSMA 解析超出范围的日期的方式 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅 [项目设置 (迁移) ](./project-settings-migration-accesstosql.md)。|  
|Access 中的索引长度超过900个字节。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 索引的索引键列的总大小具有900字节的限制。 如果访问表使用较大的索引，SSMA 将显示警告。<br /><br />如果继续进行数据迁移，迁移可能会失败。|  
|Access 对象名称为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关键字或包含特殊字符。|访问并 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 具有不同的保留关键字和特殊字符集。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果使用括在括号中的标识符或带引号的标识符（如 "select" 或 [select]），将接受使用关键字命名或包含特殊字符的对象。 有关详细信息，请参阅联机丛书中的 "带分隔符的标识符 (数据库引擎) " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />**注意：** 若要使用引号分隔标识符，请设置 QUOTED_IDENTIFIER 必须为 ON。<br /><br />例如， `CREATE TABLE [schema](c1 [FOR])` 是一个有效的语句，即使 **架构** 和是保留关键字。 同时，也 `CREATE TABLE [xxx*yyy](c1 x&y)` 是有效的语句，即使表和列名包含特殊字符 **\& #42;** 和 **&amp;** 。<br /><br />引用这些对象的所有查询都必须使用括号或引号引起来的名称。 例如，查询 `SELECT * FROM schema` 将失败。 正确的查询是： `SELECT * FROM [schema]` 。<br /><br />转换 Access 数据库对象时，"输出" 窗格将列出使用关键字或特殊字符的所有访问表。 您可以修改 Access 中的表，然后再次删除并添加数据库;或者，您可以修改引用这些对象的查询，以便查询使用括号或引号来分隔标识符。 如果不修改查询，则访问应用程序可能会返回错误或遇到其他问题。|  
|字段大小在主键/外键关系中有所不同。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持使用具有外键约束的不同数据类型或大小的链接列的 Jet 功能。<br /><br />转换 Access 数据库对象时，"输出" 窗口将列出所有不会转换为的主键/外键约束 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以更改 Access 列上的数据类型和大小，以便它们匹配，然后删除并重新添加 Access 数据库。 或者，你可以迁移数据，但是不会在中创建这些约束 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|访问关系中的引用表既没有主键也没有唯一索引。|Access 接受所引用的表没有主键或唯一索引的表之间的关系。 但是，这不受支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />转换 Access 数据库对象时，"输出" 窗口将列出具有关系但没有主键或唯一索引的任何表。 您可以更改表以添加主键或唯一索引，然后删除并重新添加 Access 数据库。 或者，您也可以迁移数据，但是表之间的关系将会断开。|  
|Access 表包含 hyperlink 列。|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持 **超链接** 列。 相反，这些列的处理方式与访问备注列相同。 默认情况下，这些列将转换为 **nvarchar (max)** 中的列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以自定义映射。 有关详细信息，请参阅 [映射源和目标数据类型](mapping-source-and-target-data-types-accesstosql.md)。|  
|默认或验证规则表达式包含无法转换为或 SQL Azure 的访问函数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|Access 默认表达式或验证规则可能包括不映射到或 SQL Azure 的访问系统函数或用户定义函数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 使用不映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的函数会阻止你将默认表达式或验证规则加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 中。|  
  
## <a name="see-also"></a>另请参阅  
[准备要迁移的 Access 数据库](preparing-access-databases-for-migration-accesstosql.md)  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
