---
description: MSSQLSERVER_3417
title: MSSQLSERVER_3417 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3417 (Database Engine error)
ms.assetid: 005829c8-cf57-4828-818a-bbe8ee2e00f0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bf4dac403156a5fff0b0b5cdb59a98d4a58abd0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201606"
---
# <a name="mssqlserver_3417"></a>MSSQLSERVER_3417
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|3417|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|REC_BADMASTER|  
|消息正文|无法恢复 master 数据库。 SQL Server 无法运行。 请利用完整备份还原 master 数据库，修复它，或者重新生成它。 有关如何重新生成 master 数据库的详细信息，请参阅 SQL Server 联机丛书。|  
  
## <a name="explanation"></a>说明  
SQL Server 无法启动 **master** 数据库。 如果无法使 **master** 或 **tempdb** 联机，则 SQL Server 无法运行。 其他错误通常在此错误之前出现。 检查错误日志以查找根本原因。  
  
## <a name="user-action"></a>用户操作  
还原数据库备份或修复数据库。  
  
