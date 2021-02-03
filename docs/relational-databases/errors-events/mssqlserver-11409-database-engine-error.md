---
description: MSSQLSERVER_11409
title: MSSQLSERVER_11409 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 11409 (Database Engine error)
ms.assetid: 99b71a1c-a72d-4ca9-9d00-4230c9042ba5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4ad5ea6caf9aec21ffcad0c7b2c0ebc9ec48b76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197211"
---
# <a name="mssqlserver_11409"></a>MSSQLSERVER_11409
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|11409|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|ALTERCOL_COLSET_DROP|  
|消息正文|无法删除列集 '%.*ls' (位于表 '%.\*ls' 中)，因为该表包含的列超过了 1025 列。|  
  
## <a name="explanation"></a>说明  
表中最多可以包含 1024 个未指定为稀疏列或计算列的列。 如果稀疏列导致表中超过 1024 列，则必须为该表定义一个列集。 所引用的表超过了 1024 列，而且您已经尝试删除了列集。  
  
## <a name="user-action"></a>用户操作  
在表中具有当前列数的情况下，您必须保留列集。  
  
若要删除列集，请首先从表中删除某些列，直到列数不超过 1024 列。 随后即可以删除列集。  
  
