---
title: “分发服务器属性”对话框
description: 介绍 SQL Server Management Studio (SSMS) 中“分发服务器属性”对话框的不同页面。
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configdistwizard.distdbproperties.f1
- sql13.rep.configdistwizard.distproperties.general.f1
- sql13.rep.configdistwizard.distproperties.publishers.f1
ms.assetid: f643c7c3-f238-4835-b81e-2c2b3b53b23f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 51e617c26aad4261d0d337ff2fd2a3dcd19c79b8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484859"
---
# <a name="sql-server-replication-distributor-properties-dialog-box"></a>SQL Server 复制“分发服务器属性”对话框 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

此页介绍在“分发服务器属性”对话框中找到的页面。 

## <a name="general"></a>常规
使用 **“分发服务器属性”** 对话框的 **“常规”** 页，可以添加和删除分发数据库，以及设置分发数据库属性。  
  
 分发数据库用于存储所有类型复制的元数据和历史记录数据，并存储事务复制的事务。 在许多情况下，单个分发数据库就能够满足需要。 不过，如果多个发布服务器使用单个分发服务器，则应考虑为每个发布服务器都创建一个分发数据库。 这样可确保通过每个分发数据库的数据流是不同的。  

 **数据库**  
 **“数据库”** 属性网格显示了分发服务器上分发数据库的名称和保持期属性。 **“事务保持期”** 是指为事务复制存储事务的时间长度（事务保持期也称为分发保持期）。 **“历史记录保持期”** 是指为所有类型的复制存储历史记录元数据的时间长度。 有关分发保持期的详细信息，请参阅[订阅过期和停用](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
 单击 **“数据库”** 属性网格中的属性按钮 ( **...** ) 可启动 **“分发数据库属性”** 对话框。  
  
 **新建**  
 单击此项可创建一个新的分发数据库。  
  
 **删除**  
 选择 **“数据库”** 属性网格中的一个现有分发数据库，再单击 **“删除”** ，即可删除该数据库。 如果只有一个这样的分发数据库，则无法删除；每个分发服务器必须至少有一个分发数据库。 若要删除所有分发数据库，则必须在该计算机上禁用分发功能。 有关详细信息，请参阅[禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)。  
  
 **默认配置文件**  
 单击此项可访问 **“代理配置文件”** 对话框中的复制代理配置文件。 有关配置文件的详细信息，请参阅 [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md)。  

## <a name="publishers"></a>发布者
可以使用 **“分发服务器属性”** 对话框的 **“发布服务器”** 页，允许发布服务器使用此分发服务器。 还可以设置与这些发布服务器关联的属性。 请注意，允许发布服务器将此服务器用作其远程分发服务器的同时，并不会使该服务器成为发布服务器。 必须连接到发布服务器，对其进行配置以用于发布，并选择此服务器作为分发服务器。 您可以通过新建发布向导配置发布服务器并选择分发服务器。  
  
 **发布服务器**  
 选择允许使用此分发服务器的服务器。 单击发布服务器旁边的属性按钮 **(...)** 可以查看和设置其他属性。  
  
 **添加**  
 如果希望允许的服务器没有列出，请单击“添加”向可用发布服务器列表中添加   发布服务器或 Oracle 发布服务器[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 如果添加的服务器是使用此分发服务器作为远程分发服务器的第一个服务器，则系统将会提示您提供管理链接密码。  
  
 **管理链接密码**  
 对于使用 **distributor_admin** 登录名在发布服务器和远程分发服务器之间进行的连接复制，使用此选项可以为其指定或更新密码：  
  
-   如果分发服务器仅用作本地分发服务器，则会随机生成并自动配置此密码。   
-   如果分发服务器已具有远程发布服务器，则密码已在此页上或配置分发向导的 **“分发服务器密码”** 页上提供。    
-   如果为此分发服务器启用第一个远程发布服务器，则系统将会提示您输入密码。  有关分发服务器的安全性的详细信息，请参阅[保护分发服务器](../../relational-databases/replication/security/secure-the-distributor.md)。  

## <a name="distribution-database"></a>分发数据库 
 可以使用 **“分发数据库属性”** 对话框查看数据库的多个属性，以及为数据库设置事务保持期和历史记录保持期。  
  
 **名称**  
 分发数据库的名称，默认值为“distribution”（只读）。  
  
 **文件位置**  
 数据库文件和日志文件的位置（只读）。  
  
 **事务保持期**  
 也称为分发保持期。 为事务复制存储的事务时间长度。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
 **历史记录保持期**  
 为所有类型的复制存储的历史记录元数据的时间长度。  
  
 **队列读取器代理安全性**  
 队列读取器代理由带有排队更新订阅的事务复制使用。 如果在新建发布向导的 **“发布类型”** 页上选择 **“带有更新订阅的事务发布”** ，将自动创建队列读取器代理。 单击 **“安全设置…”** 可以更改运行代理并连接到分发服务器时所使用的帐户。  
  
 还可以在此页上选择 **“创建队列读取器代理”** （如果已创建该代理，此选项将被禁用），来创建队列读取器代理。  
  
 下面两点说明了队列读取器代理的其他连接信息：    
-   使用 **“发布服务器属性”** 对话框中指定的凭据，该代理可以连接到发布服务器。该对话框可以在 **“分发服务器属性”** 对话框的 **“发布服务器”** 页中找到。    
-   使用在新建订阅向导中为分发代理指定的凭据，该代理可以连接到订阅服务器。  有关详细信息，请参阅  [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md)。 
  
## <a name="see-also"></a>另请参阅  
 [“配置分发”](../../relational-databases/replication/configure-distribution.md)   
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
  
  
