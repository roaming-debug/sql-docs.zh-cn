---
title: 升级数据库引擎 | Microsoft Docs
description: 本文提供了一些资源链接，这些资源可帮助你将 SQL Server 数据库引擎从 SQL Server 的以前版本升级到 SQL Server 2019。
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], after upgrade
- Database Engine [SQL Server], upgrading
ms.assetid: 3c036813-36cf-4415-a0c9-248d0a433859
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4db158cf3e5e007ce1dade79dc49f3d34dace78f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341825"
---
# <a name="upgrade-database-engine"></a>升级数据库引擎

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
  本部分中的文章可帮助将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎从先前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本升级到 [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]。  
  
1.  [选择数据库引擎升级方法](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)。 在开始升级之前，需要了解各种升级方法。 本文讨论升级方法和每个升级方法所涉及的步骤。  
  
2.  [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)。 查看升级方法后，便可开始为你的环境制定合适的升级方法，然后在升级现有环境之前对该升级方法进行测试。 本文讨论如何制定升级方法并对其进行测试。  
  
3.  [完成数据库引擎升级](../../database-engine/install-windows/complete-the-database-engine-upgrade.md)。 在数据库引擎升级并且数据库处于联机状态后，还需要完成一些步骤，包括制作新备份、升级数据库功能以启用新功能以及重新填充全文目录。 本文讨论了这些步骤。  
  
4.  升级[数据库兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)（适用于：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]）。 当你的数据库在新版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中处于联机状态后，要执行的一个步骤可能是，通过更改数据库兼容性级别来升级数据库功能模式以启用新功能。 这可以手动完成，也可以通过查询优化助手完成。 

    - [更改数据库兼容性模式和使用查询存储](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)。 手动更改数据库兼容性级别后，使用查询存储来监视性能并识别可能的回归。 本文讨论建议的过程，并提供建议的工作流。  

    - [使用查询优化助手更改数据库兼容性模式](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)。 或者，要进行手动更改，请使用查询优化助手 (QTA) 作为指导，以完成更改数据库兼容性级别的建议过程。 本文讨论此过程，并提供有关 QTA 工作流的说明。  

    要详细了解更改数据库兼容性级别后可用的新功能和改进的行为，请参阅[兼容性级别之间的差异](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-stored-procedures)。

5.  [使用新的 SQL Server 功能](https://www.microsoft.com/sql-server/sql-server-2019)。 最后，完成前面的步骤后，便可开始使用特定的新数据库引擎增强功能。 本文推荐了这些增强功能中的几个，并提供了获取详细信息的链接。  
  
  
