---
title: 编写复制脚本 | Microsoft Docs
description: 脚本包含实现 SQL Server 复制组件所需的 Transact-SQL 系统存储过程，例如发布或订阅。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server replication], replication objects
- merge replication scripting [SQL Server replication]
- replication [SQL Server], scripting
- snapshot replication [SQL Server], scripting
- scripts [SQL Server replication]
- transactional replication, scripting
ms.assetid: e50fac44-54c0-470c-a4ea-9c111fa4322b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 160066798b04b16e82ed1903555b6981323a4dcc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468788"
---
# <a name="scripting-replication"></a>编写复制脚本
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  制订灾难恢复计划时，应要求对拓扑中的所有复制组件编写脚本，另外，脚本还可以用来自动处理重复性的任务。 脚本包含为实现要为其编写脚本的复制组件所需的 Transact-SQL 系统存储过程，如发布或订阅。 创建完组件后，可以在向导（如新建发布向导）或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中创建脚本。 您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 **sqlcmd** 查看、修改和运行脚本。 脚本可以与备份文件存储在一起，以便在必须重新配置复制拓扑时使用。  
  
 如果更改了属性，便需要为组件重新编写脚本。 如果对事务复制使用自定义存储过程，则应与脚本一起存储每个过程的副本。如果过程发生更改，应更新相应的副本（通常会由于架构更改或应用程序要求的更改而更新过程）。 有关自定义过程的详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
 对于使用参数化筛选器的合并发布而言，发布脚本中包含了对创建数据分区的存储过程的调用。 该脚本提供了对所创建的分区的引用和一种在必要时重建分区的途径。  
  
## <a name="example-of-automating-a-task-with-scripts"></a>用脚本使任务自动化的示例  
 请考虑 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]，它为把数据分发给远程销售部门而实现合并复制。 销售代表可以用请求订阅下载所有他所负责的区域客户的相关数据。 脱机工作时，销售代表可以更新数据并输入新的客户和订单。 由于 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 在不同地区拥有 50 多个销售代表，所以，用新建订阅向导在每个订阅服务器上创建不同订阅需要耗费大量时间。 而复制管理员可以执行下列步骤：  
  
1.  建立必要的根据销售代表或其所在区域进行分区的合并发布。  
  
2.  为一个订阅服务器创建请求订阅。  
  
3.  根据该请求订阅生成脚本。  
  
4.  修改脚本，将这些值更改为订阅服务器的名称。  
  
5.  在多个订阅服务器上运行脚本，以生成所需的请求订阅。  
  
## <a name="script-replication-objects"></a>脚本复制对象  
 复制向导或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中“复制”文件夹中的脚本复制对象。 如果从向导编写脚本，则可以选择创建对象并为其编写脚本，也可以选择仅编写脚本。  
  
> [!IMPORTANT]  
>  所有密码的脚本被编写为 NULL。 如果可能，请在运行时提示用户输入安全凭据。 如果将凭据存储在脚本文件中，则必须确保文件的安全以防受到未经授权的访问。  
  
 有关使用复制向导的详细信息，请参阅：  
  
-   [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)  
  
-   [创建发布](../../relational-databases/replication/publish/create-a-publication.md)  
  
-   [创建推送订阅](../../relational-databases/replication/create-a-push-subscription.md)  
  
-   [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)  
  
#### <a name="to-script-an-object-from-a-replication-wizard"></a>从复制向导编写对象脚本  
  
1.  在向导的 **“向导操作”** 页上，选中适合该向导的复选框：  
  
    -   **生成包含创建发布的步骤的脚本文件**  
  
    -   **生成包含创建订阅的步骤的脚本文件**  
  
    -   **生成包含配置分发的步骤的脚本文件**  
  
2.  在 **“脚本文件属性”** 页上指定选项。  
  
3.  完成向导。  
  
#### <a name="to-script-an-object-from-management-studio"></a>从 Management Studio 编写对象脚本  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中，连接到分发服务器、发布服务器或订阅服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹或 **“本地订阅”** 文件夹。  
  
3.  右键单击某个发布或订阅，然后单击 **“生成脚本”** 。  
  
4.  在“生成 SQL 脚本 - \<ReplicationObject>”对话框中指定选项。  
  
5.  单击 **“将脚本保存到文件”** 。  
  
6.  在 **“脚本文件位置”** 对话框中输入文件名，然后单击 **“保存”** 。 将显示状态消息。  
  
7.  单击 **“确定”** ，再单击 **“关闭”** 。  
  
#### <a name="to-script-multiple-objects-from-management-studio"></a>从 Management Studio 编写多个对象的脚本  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中，连接到分发服务器、发布服务器或订阅服务器，然后展开服务器节点。  
  
2.  右键单击 **“复制”** 文件夹，然后单击 **“生成脚本”** 。  
  
3.  在 **“生成 SQL 脚本”** 对话框中指定选项。  
  
4.  单击 **“将脚本保存到文件”** 。  
  
5.  在 **“脚本文件位置”** 对话框中输入文件名，然后单击 **“保存”** 。 将显示状态消息。  
  
6.  单击 **“确定”** ，再单击 **“关闭”** 。  
  
  
