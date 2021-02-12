---
title: 在 SQL Server 上安装 SSMA 组件 (MySQLToSql) |Microsoft Docs
description: 在运行 SQL Server 的服务器上安装组件以支持 SSMA 的 MySQL 数据库转换，包括 SSMA 扩展包和 MySQL 提供程序。
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA extension pack, Installation
ms.assetid: 6772d0c5-258f-4d7b-afb0-b5f810e71af1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 48c111ab342d7410031851a5635fa73fe143b96b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070192"
---
# <a name="installing-ssma-components-on-sql-server-mysqltosql"></a>在 SQL Server 上安装 SSMA 组件 (MySQLToSql) 

除了安装 SSMA 外，还必须在运行的计算机上安装组件 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这些组件包括支持数据迁移的 SSMA 扩展包以及用于启用服务器到服务器连接的 MySQL 提供程序。

## <a name="ssma-for-mysql-extension-pack"></a>SSMA for MySQL 扩展包

SSMA 扩展包将数据库 **sysdb** 添加到指定的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 此数据库包含迁移数据所需的表和存储过程。

此外，在将数据迁移到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会创建代理作业（当服务器端数据迁移引擎用于迁移数据时）。

### <a name="prerequisites"></a>必备条件

在上安装 SSMA for MySQL server 组件之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请确保计算机满足以下要求：

- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- MySQL 客户端提供程序和与要迁移的 MySQL 数据库的连接。 你可以从 MySQL 产品媒体或 MySQL 网站安装提供程序。
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]必须在安装过程中运行 Browser 服务。 这用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在安装向导中填充实例的列表。 你可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装后禁用 Browser 服务。  

  > [!NOTE]
  > 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务正在运行，但你仍未在安装程序中看到实例列表，则必须解除 UDP 端口1434的阻止。 可以使用 Windows 防火墙暂时取消阻止端口，也可以暂时禁用 Windows 防火墙。 你可能还必须暂时禁用防病毒软件。 请确保在安装后启用防火墙和防病毒软件。

### <a name="installing-the-extension-pack"></a>安装扩展包

在将数据迁移到之前，你可以随时安装扩展包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

> [!IMPORTANT]
> 若要安装扩展包，您必须是实例上 **sysadmin** 服务器角色的成员 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

安装扩展包：

1. 将 **SSMAforMySQLExtensionPack_** 的 SSMA （其中 *n* 是内部版本号）复制到运行的计算机 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
2. 双击 SSMAforMySQLExtensionPack_ " **。**
3. 在 " **欢迎** " 对话框中，单击 " **下一步**"。
4. 在 " **最终用户许可协议** " 对话框中，阅读许可协议。 如果同意，请选择 " **我接受协议"** 选项，然后单击 " **下一步**"。
5. 在 " **选择安装类型** " 对话框中，单击 " **典型**"。
6. 在 " **准备安装** " 对话框中，单击 " **安装**"。
7. 在 " **完成第一步安装"** 对话框中，单击 " **下一** 步"。

   此时将显示一个新对话框，您可以在其中选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于安装扩展包的实例。
  
8. 选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在其中迁移 MySQL 架构的实例，然后单击 " **下一步**"。
  
   默认实例与计算机的名称相同。 命名实例后跟反斜杠和实例名称。

9. 在 "连接" 页上，选择身份验证方法，然后单击 " **下一步**"。
  
    Windows 身份验证将使用您的 Windows 凭据来尝试登录到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果选择 "服务器身份验证"，则必须输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和密码。

10. 下一步要求你设置主密钥的密码，主密钥用于在服务器端数据迁移期间对存储在扩展包数据库中的所有敏感数据进行加密。 提供强密码，并单击 " **下一步**"。

11. 在下一个对话框中，选择 " **安装实用程序数据库 *n* " 并安装扩展包库**，其中 *n* 是版本号，然后单击 " **下一步**"。

    **Sysdb** 数据库是使用 (在此数据库中创建的，这些表和存储过程是使用) 在此数据库中创建的。

12. 若要将实用工具安装到的另一个实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请选择 **"是"**，然后单击 " **下一步**"。 或者，若要退出向导，请单击 " **否**"。

## <a name="see-also"></a>另请参阅

- [安装 SSMA for MySQL 客户端](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)
- [将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
