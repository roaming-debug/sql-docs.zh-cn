---
title: Named Pipes 属性
description: 在使用 Named Pipes 协议时，可以使用“Named Pipes 属性”对话框的“协议”页查看或更改 SQL Server 侦听的命名管道。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- pipes [SQL Server]
- listening [SQL Server], pipes
- pipes [SQL Server], listening on pipes
- Named Pipes [SQL Server], listening on pipes
ms.assetid: a5fd5b8e-f889-485b-89e3-d4010ec4c6ec
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c4929816503430c151c80735078d6a71b0221e71
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463850"
---
# <a name="named-pipes-properties"></a>Named Pipes 属性
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用“命名管道属性”对话框中的“协议”页，可以在使用命名管道协议时查看或更改 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听的命名管道。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 才能启用或禁用该协议，或更改命名管道。  
  
## <a name="options"></a>选项  
 **已启用**  
 可能的值为“是”和“否”。  
  
 **管道名称**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听的命名管道。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听： `\\.\pipe\sql\query` （对于默认实例）和 `\\.\pipe\MSSQL$<instancename>\sql\query` （对于命名实例）。 此字段最多允许 2047 个字符。  
  
## <a name="creating-an-alternate-named-pipe"></a>创建备用命名管道  
 若要更改命名管道，可以在 **“管道名称”** 框中键入新的管道名称，然后停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，再将其重新启动。 由于 **sql\query** 众所周知是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用的命名管道，更改管道可以有效地降低恶意程序攻击的风险。  
  
### <a name="example"></a>示例  
 键入 **\\\\.\pipe\unit\app** 以侦听 **unit\app** 管道。  
  
 键入 **\\\\.\pipe\acct** 以侦听 **acct** 管道。  
  
## <a name="see-also"></a>另请参阅  
 [启用或禁用服务器网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [选择网络协议](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [使用命名管道创建有效的连接字符串](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
