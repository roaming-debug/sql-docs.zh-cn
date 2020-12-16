---
description: 启动复制监视器
title: 启动复制监视器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, starting
ms.assetid: e037bd27-cc87-4ee9-9e5f-83f6d717cfa4
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d88c974bbf6d8f18271f4ea6f68b95e5c0506203
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432535"
---
# <a name="start-the-replication-monitor"></a>启动复制监视器
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  可以从任何 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 实例上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中启动复制监视器，也可以在命令提示符下启动复制监视器。 启动复制监视器后，将发布服务器添加至监视器。 有关详细信息，请参阅 [从复制监视器中添加和删除发布服务器](../../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md)。  
  
### <a name="to-start-replication-monitor-from-sql-server-management-studio"></a>从 SQL Server Management Studio 启动复制监视器  
  
1.  连接至 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]实例，然后展开服务器节点。  
  
2.  右键单击 **“复制”** 文件夹或它的任一子文件夹，然后单击 **“启动复制监视器”**。  

### <a name="to-start-replication-monitor-from-the-command-prompt"></a>从命令提示符启动复制监视器  
  
1.  在命令提示符下，导航到工具安装目录。 默认路径为 [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]Tools\Binn\。  
  
2.  运行 sqlmonitor.exe。  
  
## <a name="see-also"></a>另请参阅  
 [监视复制](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
