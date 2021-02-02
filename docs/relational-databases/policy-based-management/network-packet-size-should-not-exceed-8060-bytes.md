---
description: 网络数据包大小不应超过 8060 字节
title: 网络数据包大小不应超过 8060 字节 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: 86db5da1-afe4-4fbb-8bf8-33cedc7e4361
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 737bad4dd5994bebdf23ea8ebab5f4ed6f788266
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076409"
---
# <a name="network-packet-size-should-not-exceed-8060-bytes"></a>网络数据包大小不应超过 8060 字节
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  如果为 sp_configure 'network packet size' 指定的值或者任何登录用户的网络数据包大小大于 8060 字节， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将执行不同的内存分配操作。 这可能会增加不是为缓冲池保留的进程虚拟地址空间。  
  
## <a name="best-practices-recommendations"></a>最佳做法建议  
 网络数据包大小不应超过 8060 字节。  
  
## <a name="for-more-information"></a>有关详细信息  
 [Microsoft 知识库文章 903002](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/903002)  
  
## <a name="see-also"></a>另请参阅  
 [使用基于策略的管理来监视和强制执行最佳做法](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
