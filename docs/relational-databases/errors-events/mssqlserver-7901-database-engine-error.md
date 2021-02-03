---
description: MSSQLSERVER_7901
title: MSSQLSERVER_7901 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7901 (Database Engine error)
ms.assetid: 2d0d19b9-947b-4474-9ff8-7e03019ab93d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3723349a65a5e5fafc185ec6a342965992041a80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204586"
---
# <a name="mssqlserver_7901"></a>MSSQLSERVER_7901
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|7901|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC2_DATABASE_IN_EMERGENCY_MODE|  
|消息正文|未处理修复语句。 当数据库处于紧急模式下时，不支持此级别的修复。|  
  
## <a name="explanation"></a>说明  
数据库处于紧急模式下，而且指定的修复级别不是 REPAIR_ALLOW_DATA_LOSS。 除非指定 REPAIR_ALLOW_DATA_LOSS，否则无法在紧急模式下进行修复。  
  
## <a name="user-action"></a>用户操作  
返回命令并指定 REPAIR_ALLOW_DATA_LOSS 选项。  
  
