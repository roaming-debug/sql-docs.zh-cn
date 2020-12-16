---
description: 快照文件夹
title: 快照文件夹 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.replicationutilities.specifysnapshotfolder.f1
ms.assetid: 3eb1b73f-ddb3-4d09-be6e-811c414698e9
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: cdb92210920bdc6287c690bbdad72ff666da111e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463178"
---
# <a name="snapshot-folder"></a>快照文件夹
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

**“快照文件夹”** 页出现在配置分发向导和新建发布向导中。 为快照文件夹指定的位置将用作此向导中启用的所有发布服务器的默认位置（默认快照文件夹无法应用到以后使用 **“分发服务器属性”** 对话框启用的发布服务器）。 对于配置分发向导的 **“发布服务器”** 页上或 **“分发服务器属性”** 对话框上的任何发布服务器，您均可以覆盖此默认值。  
  
快照文件夹只是指定共享的目录。向此文件夹中执行读写操作的代理必须对其具有足够的访问权限。 有关正确保护文件夹的详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。 在实现复制之前，请测试复制代理是否能够连接到快照文件夹。 以每个代理所使用的帐户登录，再尝试访问快照文件夹。  

对于 Azure SQL 托管实例，快照文件夹必须为 Azure 文件共享。 
  
## <a name="options"></a>选项  
 **快照文件夹**  
 输入用来存储快照文件的文件夹的路径。  
  
> [!NOTE]  
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您将网络共享作为快照文件夹的位置。 其他计算机上的代理无法访问本地路径（以驱动器号开头的路径，如 C:\\）。  
  
## <a name="see-also"></a>另请参阅  
 [修改快照选项](../../relational-databases/replication/snapshot-options.md)   
 [“配置分发”](../../relational-databases/replication/configure-distribution.md)   
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [使用快照初始化订阅](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
  
  
