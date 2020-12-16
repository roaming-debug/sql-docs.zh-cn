---
description: 发布服务器
title: 发布服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configuredistributionwizard.enablepublishers.f1
ms.assetid: 116cd6a5-32ac-4273-81a2-d184408e0f07
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6aa802cc14b494a3892fea71d2ddbc99f42c8749
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479828"
---
# <a name="publishers"></a>发布服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  您可以为其他发布服务器授予使用分发服务器的权限。 请注意，允许发布服务器将此服务器用作其远程分发服务器的同时，并不会使该服务器成为发布服务器。 必须连接到发布服务器，对其进行配置以用于发布，并选择此服务器作为分发服务器。 您可以通过新建发布向导配置发布服务器并选择分发服务器。  
  
 被选作发布服务器的服务器将使用此向导的 **“分发数据库”** 页上指定的分发数据库。 若要使用其他分发数据库，此时请不要启用发布服务器。 相反，在完成配置分发向导后，请使用 **“分发服务器属性”** 对话框来添加发布服务器。  
  
## <a name="options"></a>选项  
 **发布服务器**  
 选择允许使用此分发服务器的服务器。 请单击发布服务器旁边的属性按钮 ( **...** ) 以查看和设置其他属性。  
  
 **添加**  
 如果希望允许的服务器没有列出，请单击“添加”向可用发布服务器列表中添加 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器或 Oracle 发布服务器。  
  
## <a name="see-also"></a>另请参阅  
 [“配置分发”](../../relational-databases/replication/configure-distribution.md)   
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [创建发布](../../relational-databases/replication/publish/create-a-publication.md)  
  
  
