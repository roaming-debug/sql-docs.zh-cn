---
description: MSSQLSERVER_802 - 数据库引擎错误
title: MSSQLSERVER_802 - 数据库引擎错误 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 802 (Database Engine error)
ms.assetid: 5892ed24-4dcb-4bf9-a8a4-a7ca898832d5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 69492a99e8fe6c524543ea364fba78cb3b4cba69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180331"
---
# <a name="mssqlserver_802---database-engine-error"></a>MSSQLSERVER_802 - 数据库引擎错误
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|802|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|NO_BUFS|  
|消息正文|缓冲池中的可用内存不足。|  
  
## <a name="explanation"></a>说明  
当缓冲池已满且缓冲池无法再增大时，会导致此错误。  
  
## <a name="user-action"></a>用户操作  
下面的列表概述了有助于解决内存错误的一般步骤：  
  
1.  验证其他应用程序或服务是否占用此服务器上的内存。 重新配置不太重要的应用程序或服务，使其占用更少的内存。  
  
2.  开始收集以下内容的性能监视器计数器：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **:Buffer Manager**、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **:Memory Manager**。  
  
3.  检查下面的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存配置参数：  
  
    -   **max server memory**  
  
    -   **min server memory**  
  
    -   **min memory per query**  
  
    注意任何不寻常的设置，并根据需要更正它们。 满足 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的更高内存要求。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书的“设置服务器配置选项”中列出了默认设置。  
  
4.  在您看到这些错误消息时，观察 DBCC MEMORYSTATUS 输出及其变化情况。  
  
5.  检查工作负荷（并发会话数、当前执行的查询）。  
  
以下操作可能会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供更多内存：  
  
-   如果除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 外的应用程序正在占用资源，请尝试停止这些应用程序，或者在单独的服务器上运行它们。  
  
-   如果已配置 **max server memory**，请增大该设置。  
  
运行以下 DBCC 命令以释放一些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存缓存。  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
如果问题仍存在，则您将需要进一步调查，可能需要减小工作负荷。  
  
