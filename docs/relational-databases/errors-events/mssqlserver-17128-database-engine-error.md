---
description: MSSQLSERVER_17128
title: MSSQLSERVER_17128 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17128 (Database Engine error)
ms.assetid: 7b15a5e6-fd41-47ce-ba87-54f72acea4bb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2d080456f8c28df63749847fb481f7fc28e686cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196791"
---
# <a name="mssqlserver_17128"></a>MSSQLSERVER_17128
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|17128|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|INIT_NOBUFSPACE|  
|消息正文|initdata:没有可用于核心缓冲区的内存。|  
  
## <a name="explanation"></a>说明  
缓冲池的初始内存分配或预留失败，并且 SQL Server 退出。  
  
## <a name="user-action"></a>用户操作  
通常是由在非常小的计算机（远小于最低系统要求）上启动 SQL Server 引起的。  
  
