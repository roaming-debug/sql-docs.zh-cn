---
description: 验证所有订阅
title: 验证所有订阅 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.allsubscriptions.f1
helpviewer_keywords:
- Validate All Subscriptions dialog box
ms.assetid: 32e31469-36e4-42d9-a57a-12388bfd229d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: a050ee7d8f018277f4bb55726cd1e674c0ee7e68
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475628"
---
# <a name="validate-all-subscriptions"></a>验证所有订阅
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  可以使用 **“验证所有订阅”** 对话框，指定下次运行各个订阅的合并代理时应验证对合并发布的所有订阅。 验证的结果显示在复制监视器中。 有关详细信息，请参阅 [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  
  
 右键单击 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的订阅，再单击 **“验证单个订阅”**，也可以验证单个订阅。  
  
## <a name="options"></a>选项  
 **仅验证行计数**  
 选择此选项可以验证订阅服务器上的表是否与发布服务器上的表具有相同的行数。 此方法不会验证行的内容是否匹配。 行计数验证是一种简易验证方法，可帮助您了解数据是否存在问题。  
  
 **验证行计数并比较校验和以验证行数据**  
 除了可在发布服务器和订阅服务器上对行进行计数之外，还可使用二进制校验和算法来计算所有数据的校验和。 如果行计数失败，则不计算校验和。 此选项对 [!INCLUDE[ssEW](../../includes/ssew-md.md)]无效。  
  
## <a name="see-also"></a>另请参阅  
 [验证已复制的数据](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
