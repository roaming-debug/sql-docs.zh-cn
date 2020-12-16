---
title: “发布服务器属性”对话框 (SSMS)
description: 介绍 SQL Server Management Studio (SSMS) 中特定发布的“发布服务器属性”对话框。
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configdistwizard.distpubproperties.f1
- sql13.rep.configdistwizard.pubproperties.general.f1
- sql13.rep.configdistwizard.pubproperties.pubdb.f1
- sql13.rep.configdistwizard.pubproperties.subscribers.f1
ms.assetid: 98df1aea-0406-40bf-a917-4bd80464125c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e4de9c722875465bbacfdeda6889955bdc8e063c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479838"
---
# <a name="sql-server-replication-publisher-properties-dialog-box"></a>SQL Server 复制“发布服务器属性”对话框
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

本主题描述在“发布服务器属性”对话框中找到的不同选项。 

## <a name="general"></a>常规
  **“发布服务器属性”** 对话框的 **“常规”** 页显示有关发布服务器使用的分发服务器和分发数据库的只读信息。 若要更改发布服务器的分发服务器或分发数据库，请执行以下操作：  
  
1.  禁用发布服务器上的发布。 有关详细信息，请参阅[禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)。    
2.  重新配置发布和分发。 有关详细信息，请参阅 [Configure Publishing and Distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)。  

## <a name="distributor"></a>分发服务器 
使用 **“发布服务器属性”** 对话框，可以查看和修改与发布服务器和其分发服务器之间的关系相关联的属性。  
  
### <a name="options"></a>选项  
 **到发布服务器的代理连接**  
 指定以下代理从分发服务器连接到发布服务器时所处的上下文：  
  
-   允许排队更新订阅的事务发布的队列读取器代理。    
-   用于 Oracle 发布的快照代理和日志读取器代理。  
  
 选择 **“模拟代理进程帐户”** 以使用运行这些代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户的上下文连接到发布服务器，或指定 **“SQL Server 身份验证”** ，然后为 **“登录名”** 和 **“密码”** 输入值。 建议选择 **“模拟代理进程帐户”** 。 有关代理安全性的详细信息，请参阅[复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
 在新建发布向导中可以指定运行这些代理的 Windows 帐户。 您可以在如下位置更改这些帐户：  
  
-   队列读取器代理的 **“分发服务器属性”** 对话框。    
-   快照代理和日志读取器代理的 **“发布属性”** 对话框。  
  
 **其他**  
 **“发布服务器类型”** 和 **“分发数据库名称”** 都是只读属性。 可以更改 **“默认快照文件夹”** 属性。 有关快照文件夹的详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  

## <a name="publication-databases"></a>发布数据库
  通过使用 **“发布服务器属性”** 对话框的 **“发布数据库”** 页， **sysadmin** 固定服务器角色中的用户可以启用数据库以进行复制。 启用数据库并不会发布相应的数据库；不过，该数据库的 **db_owner** 固定数据库角色中的所有用户都可以对该数据库创建一个或多个发布。  
  
## <a name="options"></a>选项  
 **事务性**  
 选中此复选框后， **db_owner** 固定数据库角色中的用户将可以在数据库中创建快照发布或事务发布。 
  
 **合并**  
 选中此复选框后， **db_owner** 固定数据库角色中的用户将可以在数据库中创建合并发布。  
  

## <a name="subcribers"></a>订阅服务器
  “发布服务器属性”对话框的“订阅服务器”页面用于运行版本低于 **的**  **发布服务器**[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]。 使用此页，可以启用订阅服务器以接收此发布服务器上发布的数据。 启用订阅服务器接收此发布服务器的数据，并不会创建对此发布服务器上的发布的订阅。 若要创建订阅，必须使用新建订阅向导。  
  
### <a name="options"></a>选项  
 **“发布服务器属性”**  
 **“订阅服务器”** 属性网格显示了已启用的从此发布服务器上发布接收数据的订阅服务器。 单击订阅服务器旁边的属性按钮 ( **...** ) 可以查看和设置其他属性。  
  
 **添加**  
 单击 **“添加”** 以添加订阅服务器，然后可单击 **“添加 SQL Server 订阅服务器”** 或 **“添加非 SQL Server 订阅服务器”** 。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [创建发布](../../relational-databases/replication/publish/create-a-publication.md)   


  
