---
description: 使用数据迁移助手标识 Stretch Database 的数据库和表
title: 标识数据库和表
ms.date: 10/30/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, identifying databases
- Stretch Database, identifying tables
- identifying databases for Stretch Database
- identifying tables for Stretch Database
ms.assetid: 81bd93d8-eef8-4572-88d7-5c37ab5ac2bf
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0535dd0dbc410444f0dd9423da6e1bf40a4026bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079868"
---
# <a name="identify-databases-and-tables-for-stretch-database-with-data-migration-assistant"></a>使用数据迁移助手标识 Stretch Database 的数据库和表
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  若要标识作为 Stretch Database 候选项的数据库和表，以及潜在的障碍，请下载并运行 Microsoft 数据迁移助手。
  
## <a name="get-data-migration-assistant"></a>获取数据迁移助手
 从 [此处](https://www.microsoft.com/download/details.aspx?id=53595)下载并安装数据迁移助手。 此工具不包含在 SQL Server 安装介质中。  
  
## <a name="run-data-migration-assistant"></a>运行数据迁移助手  
  
1.  运行 Microsoft 数据迁移助手。  

2.  创建类型为“评估”的新项目并为其命名。

3.  选择“SQL Server”作为“源服务器类型”和“目标服务器类型”。

4.  选择“创建”。 

5. 在“选项”页面（步骤 1）上，选择“新功能建议”。 （可选）清除所选的“兼容性问题”。

6.  在“选择源”页面（步骤 2）上，连接到服务器，选择数据库，然后选择“添加”。

7.  选择“开始评估”。

## <a name="review-the-results"></a>查看结果  
  
1.  分析完成后，在“查看结果”页面（步骤 3）上，选择“功能建议”选项，然后选择“存储”选项卡。

2.  查看与 Stretch Database 相关的建议。 每项建议都列出了 Stretch Database 可能适合的表以及任何潜在的障碍。

## <a name="historical-note"></a>历史注释
Stretch Database 顾问之前是 SQL Server 2016 升级顾问的一个组件。 那时，必须作为单独的操作选择并运行 Stretch Database 顾问。

随着数据迁移助手的发布，它替换并扩展了升级顾问，并将 Stretch Database 顾问的功能纳入到此新工具中。 无需选择任何选项即可获取与 Stretch Database 相关的建议。 当在数据迁移助手中运行评估时，与 Stretch Database 相关的结果将显示在“功能建议”的“存储”选项卡上。
  
## <a name="next-step"></a>下一步  
 启用 Stretch Database。  
  
-   要在 **数据库** 上启用 Stretch Database，请参阅 [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)。  
  
-   当 Stretch 已在数据库上启用时，要在另一个 **表** 上启用 Stretch Database，请参阅 [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)。 
  
## <a name="see-also"></a>另请参阅  
 [Stretch Database 的限制](../../sql-server/stretch-database/limitations-for-stretch-database.md)   
 [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [为表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
