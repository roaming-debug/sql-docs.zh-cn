---
title: Alias 属性（“别名”选项卡）
description: 使用“属性”对话框的“别名”选项卡来配置别名，以便在连接到 SQL Server 实例时可以使用替代名称。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2d1498e2-129c-4ce7-88e5-408e4037243c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 65558dcdc06b42f3f475fae1b0041909bdff8967
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481708"
---
# <a name="ltaliasgt-properties-alias-tab"></a>&lt;Alias&gt; 属性（“别名”选项卡）

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

别名是可用于进行连接的备用名称。 别名封装了连接字符串所必需的元素，并使用用户所选择的名称显示这些元素。 使用“\<**Alias name**> 属性”对话框中的“别名”页查看或指定别名连接字符串的元素 。

## <a name="options"></a>选项

**别名**

用于引用此连接的名称（别名）。  

**管道名称** / **端口号**  

连接字符串的其他元素。 此框的名称随所选协议的不同而变化。 请参阅下列主题中的示例。  

协议

连接所用的协议。

**Server**

所连接的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。  

## <a name="see-also"></a>另请参阅

- [使用 Shared Memory 协议创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)
- [使用 TCP IP 创建有效的连接字符串](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)
- [使用命名管道创建有效的连接字符串](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))