---
title: 连接到 Sybase (SybaseToSQL) |Microsoft Docs
description: 连接到 SAP ASE 实例，开始使用 SSMA for Sybase (SAP ASE) 进行迁移。 使用 "连接到 Sybase" 对话框。
author: nahk-ivanov
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 524f95ef-10bd-497c-84ca-c06a0ae794fb
ms.author: alexiva
ms.openlocfilehash: 8d43e6a48746fbb1587327b430b67c6959731013
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064092"
---
# <a name="connect-to-sybase-sybasetosql"></a>连接到 Sybase (SybaseToSQL)

使用 " **连接到 sybase** " 对话框连接到要迁移的 Sybase 自适应服务器企业 (ASE) 实例。

若要访问此对话框，请在 " **文件** " 菜单上选择 " **连接到 Sybase**"。 如果以前已连接，则该命令将 **重新连接到 Sybase**。

## <a name="options"></a>选项

**提供程序**  
选择计算机上的任何已安装的提供程序，用于连接到 Sybase 服务器。

**模式**  
选择 "标准" 或 "高级" 连接模式。 在 "标准" 模式下，输入或选择服务器名称、端口、用户名和密码的值。 在高级模式中，提供连接字符串。

**服务器名称**  
输入或选择自适应服务器的名称或 IP 地址。 默认服务器名称与计算机名称相同。 这是一个标准模式选项。

**服务器端口**  
如果要将非默认端口用于连接到 ASE，请输入端口号。 默认端口号为5000。 这是一个标准模式选项。
  
**用户名**  
输入用于连接到 ASE 的用户名。 这是一个标准模式选项。

**密码**  
输入用户名的密码。 这是一个标准模式选项。

**连接字符串**  
输入用于连接到 ASE 的完整连接字符串。

连接字符串包含参数名称和值对。 参数名称因使用的提供程序而异。

**各种提供程序的连接参数如下所示：**

1. **OLE DB 提供程序** 的连接参数

   |设置|Sybase 12.5 参数|Sybase 15 参数|
   |-----------|-------------------------|-----------------------|
   |服务器名称|服务器名称|服务器|
   |Port|服务器端口地址|Port|
   |用户名|用户 ID|用户 ID|
   |密码|密码|密码|
   |提供程序|提供程序|提供程序|

   对于 Sybase ASE 12.5，连接字符串的示例如下所示：

   `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`

   对于 Sybase ASE 15，连接字符串的示例如下所示：

   `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=ASEOLEDB;Port=5000;`

2. **ODBC 提供程序** 的连接参数

   |设置|Sybase 12.5/15 参数|
   |-----------|-----------------------------|
   |驱动程序名称|驱动程序|
   |服务器名称|服务器|
   |用户名|标识号|
   |密码|Pwd|
   |端口号|Port|

   对于 Sybase ASE 12.5 或15，连接字符串的示例如下所示：

   `driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`

3. **ADO.NET 提供程序** 的连接参数

   |设置|Sybase 12.5/15 参数|
   |-----------|-----------------------------|
   |服务器名称|服务器|
   |用户名|标识号|
   |密码|Pwd|
   |端口号|Port|

   ADO.NET 提供程序的连接字符串的示例如下所示：

   `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`

   有关详细信息，请参阅 ASE 文档。

   这是一个高级模式选项。

## <a name="next-steps"></a>后续步骤

迁移过程的下一步是 [连接到 SQL Server](connect-to-sql-server-sybasetosql.md)。
