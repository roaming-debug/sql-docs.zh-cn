---
title: 完成数据库引擎升级 | Microsoft Docs
description: 本文介绍在完成 SQL Server 数据库引擎升级后可能需要执行的其他一些步骤。
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 3f08087e-e532-416c-8caa-e0ec88c57596
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 5e4d727666ad2e3c013aacdabee93517389059bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347835"
---
# <a name="complete-the-database-engine-upgrade"></a>完成数据库引擎升级

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

升级 SQL Server 完成后，还有一些可能需要完成的额外步骤。 其中包括：  
  
升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)]后，请完成以下任务：  
  
- **备份数据库：** 为每个数据库执行完整备份。  

- **启用新功能：** 在 SQL Server 2016、2017 和 2019 版中，只有在数据库的 DATABASE_COMPATIBILITY 级别更改为 130 或更高级别后，才会启用某些更改。  有关详细信息和建议工作流，请参阅 [更改数据库兼容性模式和使用 Query Store](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)。 如果数据库在 SQL Server 2014 中创建了内存优化表，请查看[内存优化表的统计信息](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md)。
  
- **Integration Services：**  
  
     将 Integration Services 包迁移到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 格式。 有关详细信息，请参阅 [升级 Integration Services 包](../../integration-services/install-windows/upgrade-integration-services-packages.md)。  
  
- **Reporting Services：** 对于新安装升级，请还原 Reporting Services 加密密钥。 有关详细信息，请参阅 [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)。  
  
- **Master Data Services：** 升级 MDS 数据库架构并创建 SQL Server 2019 Web 应用程序。 有关详细信息，请参阅 [Upgrade Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md)。  
  
- **Data Quality Services：** 升级 DQS 数据库架构并验证 DQS 数据库架构升级。 有关详细信息，请参阅 [Upgrade Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)。  
  
- **全文搜索：** 重新填充全文目录以便确保查询结果中的语义一致性。 有关详细信息，请参阅 [填充全文索引](../../relational-databases/search/populate-full-text-indexes.md)。  
  
