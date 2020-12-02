---
description: 验证单个订阅
title: 验证单个订阅 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.validateandresynch.f1
helpviewer_keywords:
- Validate Subscription dialog box
ms.assetid: 74bdf5e1-b886-4284-b5fb-332bf79ae083
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 2e80697f75a6234b913e0784b0110873b3c848ac
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470123"
---
# <a name="validate-subscription"></a>验证单个订阅
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  可以使用 **“验证单个订阅”** 对话框，指定在下次运行订阅的合并代理时应对针对合并发布的订阅进行验证。 验证的结果显示在复制监视器中。 有关详细信息，请参阅 [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  
  
 也可以在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中右键单击某个发布，并单击“验证所有订阅”来验证对合并发布的所有订阅。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>选项  
 **上次尝试验证的日期**  
 最后一个包含订阅验证（无论验证是否成功）的合并代理会话的日期。  
  
 **上次成功验证的日期**  
 最后一个包含成功订阅验证的合并代理会话的日期。  
  
 **验证此订阅**  
 选择此选项可验证订阅。  
  
 **选项**  
 单击此项可访问 **“订阅验证选项”** 对话框，使用此对话框可以指定是使用行计数验证还是使用二进制校验和验证。  
  
## <a name="see-also"></a>另请参阅  
 [验证已复制的数据](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
