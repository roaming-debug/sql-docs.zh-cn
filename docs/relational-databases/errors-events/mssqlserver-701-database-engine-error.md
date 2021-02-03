---
description: MSSQLSERVER_701
title: MSSQLSERVER_701 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 701 (Database Engine error)
ms.assetid: 3b975000-63a1-43c2-a40f-89d0a8a36bef
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5df51550a44455b728222aa02ea2f507a0b7d4fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194127"
---
# <a name="mssqlserver_701"></a>MSSQLSERVER_701
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|701|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|NOSYSMEM|  
|消息正文|系统内存不足，无法运行此查询。|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法分配足够的内存来运行查询。 这可能是由多种原因造成的，包括操作系统设置，物理内存可用性或当前工作负载的内存限制。 在大多数情况下，失败的事务不是此错误的原因。  
  
由于服务器没有足够的内存，诊断查询（如 DBCC 语句）可能失败。  
  
等待资源池“default”中的内存资源来执行该查询时发生超时。  
  
## <a name="user-action"></a>用户操作  
如果您未在使用资源调控器，我们建议您对整个服务器状态或负荷进行验证，或者检查资源池或工作负荷组设置。  
  
下面的列表概述了有助于解决内存错误的一般步骤：  
  
1.  验证其他应用程序或服务是否占用此服务器上的内存。 重新配置不太重要的应用程序或服务，使其占用更少的内存。  
  
2.  开始收集以下内容的性能监视器计数器：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **:Buffer Manager**、 **SQL Server:Memory Manager**。  
  
3.  检查以下 SQL Server 内存配置参数：  
  
    -   **max server memory**  
  
    -   **min server memory**  
  
    -   **min memory per query**  
  
    注意不正常的设置。 根据需要更正它们。 满足更高内存要求。 SQL Server 联机丛书的“设置服务器配置选项”中列出了默认设置。  
  
4.  在您看到这些错误消息时，观察 DBCC MEMORYSTATUS 输出及其变化情况。  
  
5.  检查工作负荷（例如，并发会话数，当前执行的查询）。  

以下操作可能会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供更多内存：  
  
-   如果除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 外的应用程序正在占用资源，请尝试停止运行这些应用程序，或者考虑在单独的服务器上运行它们。 这样做将消除外部内存压力。  
  
-   如果已配置 **max server memory**，请增大其设置。  
  
运行以下 DBCC 命令以释放一些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存缓存。  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
如果问题仍存在，则您将需要进一步调查，可能需要减小工作负荷。  
  
