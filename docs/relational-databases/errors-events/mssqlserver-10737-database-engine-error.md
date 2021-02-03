---
description: MSSQLSERVER_10737
title: MSSQLSERVER_10737 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10737 (Database Engine error)
ms.assetid: 208af6ed-b271-4ab8-803e-7666025385c8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05bb027048e38f976a8b32de684e59ad5cf68157
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197316"
---
# <a name="mssqlserver_10737"></a>MSSQLSERVER_10737
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|MSSQLSERVER|  
|事件 ID|10737|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|REBUILD_PARTITION_ALL_NOT_SPECIFIED|  
|消息正文|在 ALTER TABLE REBUILD 或 ALTER INDEX REBUILD 语句中，如果在 DATA_COMPRESSION 语句中指定了一个分区，则必须指定 PARTITION=ALL。 PARTITION=ALL 子句用来强调表或索引的所有分区将重新生成，即使仅在 DATA_COMPRESSION 子句中指定了一个子集也是如此。|  
  
## <a name="user-action"></a>用户操作  
将 PARTITION=ALL 子句添加到 ALTER TABLE 或 ALTER INDEX 语句中。 或者，若要重新生成特定分区，请使用 REBUILD PARTITION = \<partition-number-expr> WITH (DATA_COMPRESSION={ON | OFF})。  
  
