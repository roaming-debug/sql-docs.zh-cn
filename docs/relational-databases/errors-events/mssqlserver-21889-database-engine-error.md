---
description: MSSQLSERVER_21889
title: MSSQLSERVER_21889 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21889 (Database Engine error)
ms.assetid: ae199540-7986-4cc2-b782-cd22793236d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1e22046bafea480d385d2aac456ee8d748c61a53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196380"
---
# <a name="mssqlserver_21889"></a>MSSQLSERVER_21889
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|21889|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQLErrorNum21889|  
|消息正文|SQL Server 实例“%s”不是复制发布服务器。 在 SQL Server 实例“%s”(具有分发服务器“%s”)上运行 sp_adddistributor，以便使该实例承载发布数据库“%s”。 确保指定的登录名和密码与用于原始发布服务器的登录名和密码相同。|  
  
## <a name="explanation"></a>说明  
为了承载发布服务器数据库，SQL Server 实例必须为复制发布服务器。 **sp_validate_redirected_publisher** 在远程服务器上调用 **sp_helpdistributor** 来确定该服务器是否为复制发布服务器。 此错误表示 SQL Server 的目标实例不是复制发布服务器。  
  
## <a name="user-action"></a>用户操作  
在承载发布服务器数据库的 SQL Server 实例上执行 sp_adddistributor。 在运行 **sp_adddistributor** 时，指定正确的分发服务器。 为 \@password 参数使用与最初在分发服务器上运行 sp_adddistributor 时使用的值相同的值。  
  
