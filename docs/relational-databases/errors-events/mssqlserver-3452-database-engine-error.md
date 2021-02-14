---
description: MSSQLSERVER_3452
title: MSSQLSERVER_3452 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3452 (Database Engine error)
ms.assetid: bf838f02-7186-4b33-b01e-361b0c02de1f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1fe941e7ffaa594daa11e11e72140b4ace07cd33
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623787"
---
# <a name="mssqlserver_3452"></a>MSSQLSERVER_3452
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3452|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|REC_CHECKIDENTITY|  
|消息正文|数据库 '%.*ls' (%d)的恢复操作检测到表 ID %d 中的标识值可能不一致。 请运行 DBCC CHECKIDENT ('%.\*ls')。|  
  
## <a name="explanation"></a>说明  

在升级或应用服务更新期间，用于恢复数据库中标识值的进程发现了元数据不一致。  
  
## <a name="user-action"></a>用户操作

运行 DBCC CHECKIDENT。  
  
