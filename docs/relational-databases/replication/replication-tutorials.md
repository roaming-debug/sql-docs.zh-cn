---
title: 复制教程 | Microsoft Docs
description: 使用这些教程帮助你在 SQL Server 中为复制准备服务器，然后了解如何配置事务复制和合并复制。
ms.custom: ''
ms.date: 04/09/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- walkthroughs [SQL Server replication]
- replication [SQL Server], tutorials
ms.assetid: 19fbd10e-5b59-4cd0-a988-52d5d9206242
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 2706b8a72aea53ef6f7cb37399580722161a4ea4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468778"
---
# <a name="replication-tutorials"></a>复制教程
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
复制是一种功能强大的解决方案，可用于在服务器之间移动数据或数据子集。 可以使用事务复制在完全连接的服务器之间复制数据。 此外，还可以使用合并复制在间歇连接的服务器和客户端之间复制数据。 在本文中，你将发现可帮助准备服务器复制的教程，以及介绍如何配置事务复制和合并复制的教程。 
  
在复制教程中，“发布服务器”是指包含将要复制的源数据的服务器。 “订阅服务器”是指目标服务器。 发布服务器和订阅服务器可以共享同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，但这不是必需的。 有关详细信息，请参阅[复制发布模型概述](../../relational-databases/replication/publish/replication-publishing-model-overview.md)。  

上述教程使用 NODE1\SQL2016 作为发布服务器和分发服务器。 使用 NODE2\SQL2016 作为订阅服务器。 
  
> [!NOTE]  
> 这些教程中列举的大多数任务都能够以编程方式执行。 有关详细信息，请参阅[复制开发人员文档](../../relational-databases/replication/concepts/replication-developer-documentation.md)。  
  
## <a name="replication-tutorials"></a>复制教程  
[教程：为复制准备 SQL Server（发布服务器、分发服务器、订阅服务器）](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md) 
 
学习如何准备服务器以便以最少的权限运行复制。 开始其他复制教程之前，必须先完成本教程。  
  
[教程：在两个完全连接的服务器之间配置复制（事务）](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md)

了解如何配置事务复制，以在完全连接的服务器之间复制数据。 本教程还包括一些基本的错误故障排除方法。 

  
[教程：在服务器和移动客户端之间配置复制（合并）](../../relational-databases/replication/tutorial-replicating-data-with-mobile-clients.md)

了解如何配置合并复制，以在服务器与仅偶尔连接的一个或多个客户端之间交换数据。  
  
## <a name="see-also"></a>另请参阅  
[查看和修改复制安全设置](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md) 

[事务复制概述](./transactional/transactional-replication.md) 

[合并复制概述](./merge/merge-replication.md)

