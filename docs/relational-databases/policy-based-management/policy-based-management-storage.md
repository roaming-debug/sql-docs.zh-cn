---
description: 基于策略的管理存储
title: 基于策略的管理存储 | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, storage
ms.assetid: d0cbf214-fc2e-4917-8d31-1d71c9ffa61d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abd24579c34b11332e235d29a5f7b89a6dc183e3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88475629"
---
# <a name="policy-based-management-storage"></a>基于策略的管理存储
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  策略存储在 msdb 数据库中。 在更改策略或条件后，应对 msdb 进行备份。 有关详细信息，请参阅[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)。  
  
## <a name="storing-policies"></a>存储策略  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 包括可用于监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的策略。 默认情况下， [!INCLUDE[ssDE](../../includes/ssde-md.md)]中未安装这些策略；不过，可以从默认安装位置 C:\Program Files (x86)\Microsoft SQL Server\140\Tools\Policies\DatabaseEngine\1033 导入这些策略。  
  
 可以使用“文件/新建”菜单直接创建策略，然后将这些策略保存到文件中。 这样，在未连接到[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的情况下也可以创建策略。  
  
 在当前 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中评估的策略的策略历史记录保存在 msdb 系统表中。 不会保留应用于 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的其他实例、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的策略的策略历史记录。  
  
  
