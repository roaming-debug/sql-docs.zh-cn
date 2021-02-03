---
description: MSSQLSERVER_9532
title: MSSQLSERVER_9532 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9532 (Database Engine error)
ms.assetid: ab95cce8-4f97-4aea-a746-a73eea7c9aab
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1bd1948e2475b792198465ed8befc838ab00db48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187569"
---
# <a name="mssqlserver_9532"></a>MSSQLSERVER_9532
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|9532|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|XMLERR_COLUMNSET_CANNOT_CONVERT_FROM_TO|  
|消息正文|在涉及列集 '%.*ls' 的查询/DML 操作中，将列 '%.\*ls' 从数据类型 '%ls' 转换为数据类型 '%ls' 时转换失败。|  
  
## <a name="explanation"></a>说明  
列集是一种非类型化的 XML 表示形式，它将表的某些列组合成为结构化的输出。 通过 XML 列集插入或更新稀疏列值时，插入到基础稀疏列的值将从 **xml** 数据类型隐式转换为另一种类型。 所提供的值无法转换为列的数据类型。  
  
## <a name="user-action"></a>用户操作  
由于无法对提供的值进行隐式转换，因此该值可能是一个无效项。 更正该错误，然后重试。 如果该值正确，修改该语句以使用单个列而不使用列集。 通过此操作可将该值显式转换为正确的数据类型。  
  
