---
title: 客户端协议 - Named Pipes 属性（“协议”选项卡）
description: 了解如何在 SQL Server 配置管理器中查看或修改默认管道的说明。 了解如何连接到不同的管道。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- pipes [SQL Server], connecting to
- Named Pipes [SQL Server], default pipe
- client protocols [SQL Server]
ms.assetid: 30fbae62-2f2e-4d36-9c6e-3444fff68781
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6e637bb0dd5979e5149ef70cba59ed8a4ac7f583
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463882"
---
# <a name="client-protocols---named-pipes-properties-protocol-tab"></a>客户端协议 - Named Pipes 属性（“协议”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器中，使用“Named Pipes 属性”对话框中的“协议”选项卡可以查看或修改默认管道的说明。 若要连接到其他管道，请在 **“默认管道”** 框中键入该管道。 有关连接字符串的详细信息，请参阅 [Creating a Valid Connection String Using Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))。  
  
## <a name="options"></a>选项  
 **“默认管道”**  
 指定 Named Pipes 网络库用来尝试连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]目标实例的默认管道。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 侦听： `\\.\pipe\sql\query`  
  
 若要连接到默认管道，请输入 `sql\query`  
  
 **已启用**  
 可能的值为“是”和“否”。  
  
## <a name="see-also"></a>另请参阅  
 [选择网络协议](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
