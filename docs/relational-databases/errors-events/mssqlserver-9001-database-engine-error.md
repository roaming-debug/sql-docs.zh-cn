---
description: MSSQLSERVER_9001
title: MSSQLSERVER_9001 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9001 (Database Engine error)
ms.assetid: a54de936-90c6-4845-aa96-29d32f154601
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d444024bbe25bdb6d49bc02b3fa82596dd6e8a6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162007"
---
# <a name="mssqlserver_9001"></a>MSSQLSERVER_9001
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|9001|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LOG_NOT_AVAIL|  
|消息正文|数据库 '%.*ls' 的日志不可用。 有关相应错误消息，请查看事件日志。 修复所有错误后重新启动数据库。|  
  
## <a name="explanation"></a>说明  
数据库日志处于脱机状态。 通常，这表示需要重新启动数据库的灾难性故障。  
  
## <a name="user-action"></a>用户操作  
诊断其他错误并重新启动 SQL Server 实例（如果尚未自行重新启动）。  
  
