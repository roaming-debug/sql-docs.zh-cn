---
title: 客户端协议属性（“顺序”选项卡）
description: 了解如何启用或禁用客户端协议。 了解当客户端尝试连接到 SQL Server 时如何重新安排协议的使用顺序。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- client protocols [SQL Server]
ms.assetid: 64fd7135-1756-4885-bed9-9ab8997ecc6c
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 18c8dfab9d2fef64ccd7e441eeab50eca3fdce97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349852"
---
# <a name="client-protocols-properties-order-tab"></a>客户端协议属性（“顺序”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用“客户端协议属性”对话框中的“顺序”页可以查看和启用客户端协议 。  
  
 单击某个协议，再单击 **“启用”** 或 **“禁用”** ，可以将所选协议移到 **“启用的协议”** 列表或 **“禁用的协议”** 列表中。  
  
 将按协议列出的顺序尝试协议，首先尝试使用第一个列出的协议进行连接，然后使用第二个列出的协议，以此类推。通过单击向上键和向下键按钮，可以在“启用的协议”列表中向上或向下移动协议。 从该计算机上的客户端连接到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，将始终首先尝试使用“Shared Memory” 协议（如果已启用）。  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET SqlClient 不会使用这些设置。 .NET SqlClient 的协议顺序依次为：TCP、命名管道。此顺序不能改变。  
  
## <a name="options"></a>选项  
 **“启用的协议”**  
 列出已安装但当前未使用的协议。  
  
 **“禁用的协议”**  
 列出此计算机上的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端可以使用的协议。  
  
 **>**  
 启用“禁用的协议”框中当前突出显示的协议，会将其移到“启用的协议”框中。  
  
 **\<**  
 禁用“启用的协议”框中当前突出显示的协议，会将其移到“禁用的协议”框中。  
  
 向上键  
 在列表中向上移动突出显示的协议。 这样就可以使网络库尝试使用所选协议进行连接的优先级提高。  
  
 向下键  
 在列表中向下移动突出显示的协议。 这样就可以使网络库尝试使用所选协议进行连接的优先级降低。  
  
 **启用 Shared Memory 协议**  
 从该计算机上的客户端连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，启用 Shared Memory 协议。在启用了该协议的情况下，始终首先尝试使用该协议。  
  
> [!NOTE]  
>  如果通过前缀或连接字符串的一部分指定了协议，则仅尝试使用指定的协议。  
  
## <a name="see-also"></a>另请参阅  
 [选择网络协议](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
