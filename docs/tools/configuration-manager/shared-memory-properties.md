---
title: shared memory 属性
description: 了解如何启用或禁用共享内存协议，客户端可使用该协议连接到在同一台计算机上运行的 SQL Server 实例。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- shared memory [SQL Server]
ms.assetid: dc1704da-eacd-4d26-b529-c996f958ca4b
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8f5d913a9517ef441b50ea4bc3e8cbac5791ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349638"
---
# <a name="shared-memory-properties"></a>shared memory 属性
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用“共享内存属性”对话框的“协议”页可以启用或禁用共享内存协议。  Shared Memory 是可供使用的最简单协议，没有可配置的设置。 由于使用 Shared Memory 协议的客户端仅可以连接到在同一台计算机运行的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，因此它对于大多数数据库活动而言是没有用的。 如果怀疑其他协议配置有误，请使用 Shared Memory 协议进行故障排除。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 才能启用或禁用该协议。  
  
## <a name="options"></a>选项  
 **已启用**  
 可能的值为“是”和“否”。 默认情况下，Shared Memory 协议是启用的。  
  
## <a name="see-also"></a>另请参阅  
 [选择网络协议](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [使用 Shared Memory 协议创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
