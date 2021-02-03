---
description: MSSQLSERVER_3431
title: MSSQLSERVER_3431 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3431 (Database Engine error)
ms.assetid: 9541217f-e5c6-4a12-a19a-006058f1d3f3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6c66c72500e19689debc69fabd6ab4b6f5879193
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185232"
---
# <a name="mssqlserver_3431"></a>MSSQLSERVER_3431
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3431|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|UNRESOLVED_XACT|  
|消息正文|无法恢复数据库 '%.*ls' (数据库 ID 为 %d)，因为事务结果尚未解析。 Microsoft 分布式事务处理协调器 (MS DTC) 事务已准备好，但 MS DTC 无法确定解析方法。 若要进行解析，请修复 MS DTC，从完整备份进行还原，或者修复数据库。|  
  
## <a name="explanation"></a>说明  
在数据库关闭时，使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 的一项或多项分布式事务尚未完成。 此数据库的恢复失败，因为没有来自 MS DTC 的详细信息，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法完成事务或回退事务。  
  
## <a name="user-action"></a>用户操作  
若要恢复该数据库，必须首先解决 MS DTC 存在的问题。 若要调查 MS DTC 存在的问题，请检查 Windows 事件日志。 如果无法解决 MS DTC 问题和恢复数据库，请还原数据库备份。  
  
