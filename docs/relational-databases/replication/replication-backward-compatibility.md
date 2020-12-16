---
title: 复制的向后兼容性 | Microsoft Docs
description: 在升级之前，或者复制拓扑中有多个版本的 SQL Server 时，请查看这些资源以确保复制中的向后兼容性。
ms.custom: ''
ms.date: 03/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, backward compatibility
- backward compatibility [SQL Server replication]
- merge replication backward compatibility [SQL Server replication]
- replication [SQL Server], backward compatibility
- backward compatibility [SQL Server], replication
- snapshot replication [SQL Server], backward compatibility
- compatibility [SQL Server replication]
ms.assetid: 091c51dc-8b32-4b4f-847e-b317456c8394
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a48ca05acd680cfd89f4438a64cac83c5d00dd0f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468818"
---
# <a name="replication-backward-compatibility"></a>复制的向后兼容性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

如果要升级或者在复制拓扑中有 SQL Server 的多个版本，则了解向后兼容性是非常重要的。 

一般规则是： 

-   分发服务器的版本可以是高于或等于发布服务器版本的任何版本（在许多情况下，分发服务器与发布服务器是同一个实例）。    
-   发布服务器的版本可以是低于或等于分发服务器版本的任何版本。    
-   订阅服务器版本取决于发布的类型：    
    - 用于事务发布的订阅服务器版本可以是两个发布服务器版本中的任何一个版本。 例如：SQL Server 2012 (11.x) 发布服务器可以包含 SQL Server 2014 (12.x) 和 SQL Server 2016 (13.x) 订阅服务器；SQL Server 2016 (13.x) 发布服务器可以包含 SQL Server 2014 (12.x) 和 SQL Server 2012 (11.x) 订阅服务器。     
    - 合并发布的订阅服务器可以是等于或低于发布服务器版本的所有版本，而发布服务器版本是生命周期支持周期支持的版本。  


## <a name="replication-matrix"></a>复制矩阵
[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]


## <a name="additional-resources"></a>其他资源
 [SQL Server 复制中不推荐使用的功能](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中为实现后向兼容性而保留，但在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的将来版本中将删除的复制功能。  
  
 [SQL Server 复制中的重大更改](../../relational-databases/replication/breaking-changes-in-sql-server-replication.md)  
 可能需要更改应用程序的复制功能更改。 

 [升级复制的数据库](../../database-engine/install-windows/upgrade-replicated-databases.md)  
 升级参与复制拓扑的 SQL Server 的步骤和注意事项。 
  
  
