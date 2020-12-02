---
description: 验证多个订阅
title: 验证多个订阅 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.subscriptions.f1
helpviewer_keywords:
- Validate Subscriptions dialog box
ms.assetid: 0ca39a35-f22c-46c5-82a4-342e34bf5d1b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: cf4b901c4d830bf333e3078b836eb775ceac6d3a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470107"
---
# <a name="validate-subscriptions"></a>验证多个订阅
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  可以使用 **“验证多个订阅”** 对话框，指定在下次运行各个订阅的分发代理时应验证对事务发布的订阅。 验证的结果显示在复制监视器中。 有关详细信息，请参阅 [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>选项  
 **验证所有 SQL Server 订阅**  
 选择此选项将验证此发布的所有 SQL Server 订阅的数据。  
  
 **验证下列订阅**  
 如果不希望验证所有订阅，请选中该选项。 从列表中选择要验证的订阅。  
  
 **验证选项**  
 单击此项可访问 **“订阅验证选项”** 对话框，使用此对话框可以指定是使用行计数验证还是使用二进制校验和验证。  
  
## <a name="see-also"></a>另请参阅  
 [验证已复制的数据](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
