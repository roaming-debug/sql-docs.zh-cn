---
title: 连接到 Oracle (OracleToSQL) |Microsoft Docs
description: 了解如何使用 SSMA for Oracle 连接到 Oracle 数据库以开始迁移。 使用 "连接到 Oracle" 对话框。
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 23a48cb6-ff30-49bb-b4a7-603ebcab336f
ms.author: alexiva
ms.openlocfilehash: 2eb44fa569048fe9e668a210ab97e66d1c8a8b55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058592"
---
# <a name="connect-to-oracle-oracletosql"></a>连接到 Oracle (OracleToSQL) 

使用 " **连接到 oracle** " 对话框连接到要迁移的 Oracle 数据库。

若要访问此对话框，请在 " **文件** " 菜单上选择 " **连接到 Oracle**"。 如果你以前已连接，则该命令将 **重新连接到 Oracle**。

## <a name="options"></a>选项

**提供程序**  
选择用于连接到 Oracle 数据库的数据访问接口。 可用的提供程序是 Oracle 客户端提供程序和 OLE DB 提供程序。 默认值为 Oracle 客户端提供程序。

**模式**  
选择 "标准"、"TNSNAME" 或 "连接字符串" 模式。

- 在 "标准" 模式下，为提供程序、服务器名称、服务器端口、Oracle SID、用户名和密码输入或选择值。
- 在 TNSNAME 模式下，可以输入 Oracle 数据库的连接标识符 (TNS alias) ，用户名和密码。
- 在连接字符串模式下，你将提供一个连接字符串。

  > [!IMPORTANT]
  > 建议您不要使用连接字符串模式，因为文本可能包含密码，并以明文形式发送。

默认值为 "标准模式"。

**服务器名称**  
输入 Oracle 服务器名称。 默认服务器名称与计算机名称相同。 这是一个标准模式选项。

**服务器端口**  
如果你使用的端口号不是 1521 (默认值为到 Oracle 的连接) ，请输入端口号。 这是一个标准模式选项。

**连接标识符**  
输入 Oracle connect 标识符。 这是在本地 tnsnames.ora. tnsnames.ora 文件中定义的数据库的别名。

这是一个 TNSNAME 模式选项。

**Oracle SID**  
输入数据库的 SID。 SID 是用于区分计算机上的 Oracle 数据库的标识符。 数据库的默认 SID 是数据库名称的前八个字符。

这是一个标准模式选项。

**用户名**  
输入 SSMA 将用于连接到 Oracle 数据库的用户名。

**密码**  
输入用户名的密码。

**连接字符串**  
> [!IMPORTANT]
> 建议您不要使用连接字符串模式，因为文本可能包含密码，并以明文形式发送。

如果使用连接字符串模式，请输入与 Oracle 的连接的完整连接字符串。

连接字符串包含参数名称和值对。

- 有关 OLE DB 连接字符串的信息，请参阅 MSDN Library 上的 [Oracle 的 Microsoft OLE DB 提供程序](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) 文章。

对于 SSMA 连接字符串，请始终包含提供程序参数。 此外，请确保在连接到 Oracle 时包含 Port 参数。

## <a name="next-steps"></a>后续步骤

迁移过程的下一步是 [连接到 SQL Server](connect-to-sql-server-oracletosql.md)。