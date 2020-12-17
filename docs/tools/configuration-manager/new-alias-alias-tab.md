---
title: 新建别名（“别名”选项卡）
description: 了解如何创建别名，它是一个 SQL Server 实例的替代名称（在与该示例连接时使用）。 查看使用别名的时机的示例。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 785eb6fb-f67e-449d-b1c8-c38dfbb95ef6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 564d97244e19a7520d8f9ae86804af76e480009a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481648"
---
# <a name="new-alias-alias-tab"></a>新建别名（“别名”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  别名是可用于进行连接的备用名称。 别名封装了连接字符串所必需的元素，并使用用户所选择的名称显示这些元素。 使用“别名 - 新建”对话框中的“别名”页可以指定别名连接字符串的元素。  若要更改现有别名的连接字符串，请参阅 [<别名> 属性（别名选项卡）](../../tools/configuration-manager/alias-properties-alias-tab.md)。  
  
 无需在 **“属性”** 的所有网格中都填写值。 有效组合因所选协议的不同而有所变化。 请参阅下面列出的有关有效组合示例的主题。  
  
 **别名**  
 用于引用此连接的名称（别名）。  
  
 **管道名称** / **端口号**  
 连接字符串的其他元素。 此框的名称随所选协议的不同而变化。  
  
 协议  
 连接所用的协议。  
  
 **Server**  
 要连接到的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。  
  
## <a name="when-to-use-an-alias"></a>何时使用别名  
 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] “共享内存” **协议连接到** 的本地实例，使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] “TCP/IP” **或** “命名管道” **连接到其他计算机上的** 实例。 请在以下情况下创建别名：使用 TCP/IP 或命名管道，并且希望提供自定义连接字符串时；希望使用服务器名称之外的其他名称进行连接时。  
  
### <a name="examples"></a>示例  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会侦听默认 TCP/IP 端口 1433，因此你希望提供一个包含另一端口号的连接字符串。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会侦听默认命名管道，因此您希望提供一个包含不同管道名称的连接字符串。  
  
-   希望将应用程序连接到名为 `ACCT`的服务器上的数据库，但该数据库已合并为名为 `ACCT` 的服务器上的 `CENTRAL`实例。 该应用程序不能轻易更改。 创建一个别名 `ACCT`，其中包含指向 `CENTRAL\ACCT`的连接字符串。  
  
## <a name="creating-a-valid-connection-string"></a>创建有效连接字符串  
 有关别名属性的有效组合的说明和示例，请参阅下列主题：  
  
-   [使用 Shared Memory 协议创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
-   [使用 TCP IP 创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
-   [使用命名管道创建有效的连接字符串](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
