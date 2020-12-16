---
description: 分发服务器
title: 分发服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.replicationutilities.selectdistributor.f1
ms.assetid: 787f0e9c-09dd-438a-bc04-5b8f99c127b8
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8aff20655f9843313387fd87cefbcee449060cb2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460280"
---
# <a name="distributor"></a>分发服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  **“分发服务器”** 页出现在配置分发向导和新建发布向导中。 分发服务器是包含分发数据库并为所有类型的复制存储元数据和历史记录数据的服务器。 分发服务器还为事务复制存储事务。 分发服务器与发布服务器可以是同一服务器（本地分发服务器），也可以是不同的服务器（远程分发服务器）。 分发服务器的角色根据所实现的复制类型的不同而不同。 通常，对于事务复制，分发服务器角色要远比合并复制和快照复制重要。 合并和快照复制通常使用本地分发服务器，而对于繁忙的系统来说，为事务复制使用远程分发服务器可以提高性能。  
  
 分发服务器在其所在服务器上使用以下附加资源：  
  
-   如果在分发服务器上存储发布的快照文件（通常如此），则需要附加磁盘空间来存储快照文件。  
  
-   存储分发数据库所需的附加磁盘空间。  
  
-   运行于分发服务器上的复制代理执行推送订阅所需的附加处理器资源。  
  
 选作分发服务器的服务器应有足够的磁盘空间和处理器运算能力，以支持该服务器上的复制和任何其他活动。  
  
## <a name="options"></a>选项  
 **“\<ServerName>”将充当自己的分发服务器；SQL Server 将创建分发数据库和日志**  
 选择此选项可将所连接的服务器配置为分发服务器。  
  
 **使用以下服务器作为分发服务器(注意: 您选择的服务器必须已配置为分发服务器)**  
 选择此选项，再单击下面的服务器名称，可将另外一个服务配置为分发服务器。  
  
 **添加**  
 如果未列出要用作分发服务器的服务器，请单击 **“添加”** 以标识服务器，并将其添加到列表中。  
  
> [!NOTE]  
>  若要使用远程服务器作为分发服务器，则远程服务器必须已配置为分发服务器。 对其运行此向导的服务器必须在该分发服务器上启用为发布服务器。  
  
## <a name="see-also"></a>另请参阅  
 [“配置分发”](../../relational-databases/replication/configure-distribution.md)   
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)  
  
  
