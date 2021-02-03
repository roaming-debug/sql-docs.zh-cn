---
description: MSSQLSERVER_7915
title: MSSQLSERVER_7915 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7915 (Database Engine error)
ms.assetid: 63338587-7dd3-40e6-b70e-d8ae18fff47b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4022199bcf7ce81c0d3e23b20840e28103500f8c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185901"
---
# <a name="mssqlserver_7915"></a>MSSQLSERVER_7915
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|7915|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC2_REPAIR_IAM_CHAIN_REBUILT|  
|消息正文|修复:对象 ID O_ID，索引 ID I_ID，分区 ID PN_ID，分配单元 ID A_ID（类型为 TYPE）的 IAM 链已在页 P_ID 前截断，将重新生成该链。|  
  
## <a name="explanation"></a>说明  
这是 REPAIR 发送的信息性消息，指示修补了指定的索引分配映射 (IAM) 链，以便可以重新生成它。 此修补可能需要分配新的 IAM 链头或者删除错误页。  
  
## <a name="user-action"></a>用户操作  
无  
  
