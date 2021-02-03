---
description: MSSQLSERVER_9692
title: MSSQLSERVER_9692 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9692 (Database Engine error)
ms.assetid: 02399d6e-ab5e-4f30-8a3e-2bb1e8c135b5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f086df43d106810e471d8795dab212dcfc0c1dd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187561"
---
# <a name="mssqlserver_9692"></a>MSSQLSERVER_9692
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|9692|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SB2_CANT_LISTEN_PORT_IN_USE|  
|消息正文|%S_MSG 协议传输无法侦听端口 %d，因为其他进程正在使用此端口。|  
  
## <a name="explanation"></a>说明  
计算机上的其他程序正在使用指示的 TCP 端口。  
  
## <a name="user-action"></a>用户操作  
运行 **netstat -aon** 以确定哪个程序正在使用该端口。 禁用该应用程序，或者为 Service Broker 指定其他端口。  
  
