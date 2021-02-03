---
description: MSSQLSERVER_8966
title: MSSQLSERVER_8966 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8966 (Database Engine error)
ms.assetid: 6b1150fd-9dfd-4df9-8f08-8eca237667db
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6b3a7c18f37d0bb774b8a0af06cdd31a377425f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194037"
---
# <a name="mssqlserver_8966"></a>MSSQLSERVER_8966
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|8966|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC3_FAILED_TO_READ_AND_LATCH_PAGE|  
|消息正文|无法使用闩锁类型 TYPE 读取并闩锁页 P_ID。 操作失败。|  
  
## <a name="explanation"></a>说明  
页读取失败或无法针对 PFS 或 GAM 页进行闩锁。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中可能会有闩锁超时或其他附带的消息。  
  
## <a name="user-action"></a>用户操作  
查阅 SQL 错误日志，检查附带的消息并解决其中的错误。  
  
