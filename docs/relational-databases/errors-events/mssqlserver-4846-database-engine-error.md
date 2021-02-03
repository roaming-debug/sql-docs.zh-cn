---
description: MSSQLSERVER_4846
title: MSSQLSERVER_4846 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4846 (Database Engine error)
ms.assetid: a455e809-1883-4c7d-b3e3-835ee5bfe258
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 789b6220c0b3b61fba1567c88ca5405d7b1e8895
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185579"
---
# <a name="mssqlserver_4846"></a>MSSQLSERVER_4846
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|4846|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|BULKPROV_MEMORY|  
|消息正文|大容量数据提供程序分配内存失败。|  
  
## <a name="explanation"></a>说明  
内存分配失败。  
  
## <a name="user-action"></a>用户操作  
执行以下一般步骤以解决内存错误：  
  
1.  验证其他应用程序或服务是否占用此服务器上的内存。 重新配置不太重要的应用程序或服务，使其占用更少的内存。  
  
2.  开始收集以下内容的性能监视器计数器：**SQL Server:Buffer Manager**、**SQL Server:Memory Manager**。  
  
3.  检查以下 SQL Server 内存配置参数：  
  
    -   **max server memory**  
  
    -   **min server memory**  
  
    -   **min memory per query**  
  
    注意任何不寻常的设置。 根据需要更正它们。 满足 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的内存要求。 SQL Server 联机丛书的“设置服务器配置选项”中列出了默认设置。  
  
4.  在您看到这些错误消息时，观察 DBCC MEMORYSTATUS 输出及其变化情况。  
  
5.  检查工作负荷（例如，并发会话数，当前执行的查询）。  
  
以下操作可能会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供更多内存：  
  
-   如果除 SQL Server 外的应用程序正在占用资源，请尝试停止运行这些应用程序，或者考虑在单独的服务器上运行它们。 这样做将消除外部内存压力。  
  
-   如果已配置 **max server memory**，请增大其设置。  
  
运行以下 DBCC 命令以释放一些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存缓存。  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
如果问题仍存在，则您将需要进一步调查，可能需要减小工作负荷。  
  
