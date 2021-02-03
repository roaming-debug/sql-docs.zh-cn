---
description: MSSQLSERVER_7911
title: MSSQLSERVER_7911 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7911 (Database Engine error)
ms.assetid: dd8390f3-0f77-4fb2-ba94-631a56e42bc6
author: MashaMSFT
ms.author: mathoma
robots: noindex,nofollow
ms.openlocfilehash: 5fe814cb9eb29e59e2ed10d5f820bc148997b5ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210535"
---
# <a name="mssqlserver_7911"></a>MSSQLSERVER_7911
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|7911|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC2_REPAIR_PAGE_DEALLOCATED|  
|消息正文|修复:页 P_ID 已从对象 ID O_ID，索引 ID I_ID，分区 ID PN_ID，分配单元 ID A_ID（类型为 TYPE）中释放。|  
  
## <a name="explanation"></a>说明  
这是来自 REPAIR 的信息性消息，指出页已从索引分配映射 (IAM) 页的单页槽数组释放。  
  
## <a name="user-action"></a>用户操作  
无  
  
