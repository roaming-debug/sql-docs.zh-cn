---
description: 连接到 MySQL (MySQLToSQL)
title: 连接到 MySQL (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 94099d01-ab19-4d58-a172-340c86b4a0f3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 33017262982effedd9fa85ea26818fdb7b2ae8ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068950"
---
# <a name="connect-to-mysql-mysqltosql"></a>连接到 MySQL (MySQLToSQL)
使用 " **连接到 mysql** " 对话框连接到要迁移的 MySQL 数据库。  
  
若要访问此对话框，请在 " **文件** " 菜单上选择 " **连接到 MySQL**"。 如果你以前已连接，则该命令将 **重新连接到 MySQL**。  
  
## <a name="options"></a>选项  
**提供程序**  
  
可用的 MySQL 提供程序是 (可信) 的 MySQL ODBC 5.1 驱动程序。  
  
**模式**  
  
默认模式为标准模式。 在 "标准" 模式下，输入或选择 MySQL、服务器名称、服务器端口、用户名和密码的值。  
  
**服务器名称**  
  
输入 MySQL 服务器名称。 这是一个标准模式选项。  
  
**服务器端口**  
  
输入服务器端口。 默认服务器端口为3306。 这是一个标准模式选项。  
  
**用户名**  
  
输入 SSMA 将用于连接到 MySQL 数据库的用户名。  
  
**密码**  
  
输入用户名的密码。  
  
**SSL**  
  
如果希望安全地连接到 MySQL，请通过选中 " **ssl** " 复选框， (ssl) 使用安全套接字层。  
  
**配置**  
  
它提供了一个选项，用于通过安全套接字层 (SSL) 配置与 MySQL 的连接。  
  
> [!NOTE]  
> 若要启用 **配置**，SSL 必须设置为 **True**。  
  
单击 "配置" 按钮时，会显示一个对话框。 若要在连接到 MySQL 数据库时使用加密，必须定义该对话框中提供的以下三个证书文件的路径 [隐私增强邮件证书 (PEM) ]：  
  
-   **SSL 证书颁发机构：** 指定带有可信 SSL Ca 列表的文件的路径。  
  
-   **SSL 证书：** 指定用于建立安全连接的 SSL 证书文件的名称。  
  
-   **SSL 密钥：** 指定用于建立安全连接的 SSL 密钥文件的名称。  
  
> [!NOTE]  
> -   如果提供了所需的信息，则 " **确定** " 按钮处于启用状态。 如果任何文件路径无效，则 "确定" 按钮将保持禁用状态。  
> -   " **取消** " 按钮关闭对话框，并从主连接窗体 **禁用** SSL 选项。  
  
