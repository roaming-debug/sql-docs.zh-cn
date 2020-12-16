---
title: 安装 SQL Server 复制 | Microsoft Docs
description: 通过使用 SQL Server 安装向导或在命令提示符窗口中安装复制组件。
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server replication]
- command line installations [SQL Server replication]
- installing replication
- replication [SQL Server], installing
- command prompt [SQL Server replication]
ms.assetid: c50ad078-060b-4a8d-ad45-9e31a8d85729
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d258594f464a18d1c2495c2ca868a043d91e415e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460710"
---
# <a name="install-sql-server-replication"></a>安装 SQL Server 复制

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导或通过命令提示符安装复制组件。 请在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]或修改现有实例时安装复制组件。  
  
安装复制组件后，必须配置服务器才可以使用复制。 有关详细信息，请参阅 [联机丛书中的](../../relational-databases/replication/configure-distribution.md) 配置分发 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
>[!IMPORTANT]  
>如果在修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的现有实例时安装复制组件，则在安装完成后必须停止并重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。 此操作可帮助确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理能够识别复制代理子系统，并且可以在作业步骤中调用复制代理。  
  
## <a name="installing-replication-by-using-setup"></a>使用安装程序安装复制  
**在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- 若要安装包括复制管理对象 (RMO) 在内的复制组件，请在安装向导的“功能选择”页上选择“SQL Server 复制”。  
  
## <a name="installing-replication-from-the-command-prompt"></a>从命令提示符安装复制  
 **在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- 请参阅[从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 SQL Server](../../database-engine/install-windows/install-sql-server.md)   
 [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)   
 [SQL Server 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
