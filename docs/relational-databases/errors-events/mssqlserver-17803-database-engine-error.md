---
description: MSSQLSERVER_17803
title: MSSQLSERVER_17803 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17803 (Database Engine error)
ms.assetid: 28591a19-258d-4891-b78a-4686789bb2d7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 15b5f790ac15b43eb51e14e1410ac8053f0b4a91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196653"
---
# <a name="mssqlserver_17803"></a>MSSQLSERVER_17803
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|17803|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SRV_NOMEMORY|  
|消息正文|在建立连接过程中出现内存分配错误。 减少不必要的内存负载，或增加系统内存。 该连接已关闭。%.*ls|  
  
## <a name="explanation"></a>说明  
服务器内存不足。  
  
## <a name="user-action"></a>用户操作  
确定服务器内存不足的原因。 一般内存故障排除步骤可能会很有用  
  
