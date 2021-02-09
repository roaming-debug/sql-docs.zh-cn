---
title: “SQL Server 登录”对话框 (ODBC)
description: 当应用程序在没有指定连接数据库所需的足够信息的情况下建立 ODBC 连接时，可能会出现 SQL Server 登录对话框。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-jizho2
ms.openlocfilehash: 9a2ba6a75457b2c4742603368d153f5636d3b541
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158216"
---
# <a name="sql-server-login-dialog-box-odbc"></a>“SQL Server 登录”对话框 (ODBC)

当调用一个 ODBC 连接而不为驱动程序指定足够的信息来连接到 SQL Server 时，ODBC 驱动程序将显示“SQL Server 登录”对话框。

## <a name="options"></a>选项

### <a name="server"></a>服务器

网络上的 SQL Server 实例的名称。 从列表中选择一个服务器\实例名，或在“服务器”  框中键入服务器\实例名。 或者，可以在客户端计算机上使用“SQL Server 配置管理器”  创建服务器别名，并在“服务器”  框中键入该名称。

当使用与 SQL Server 相同的计算机时可以输入“(local)”。 即使正在运行非联网版的 SQL Server，也可以连接到 SQL Server 的本地实例。

有关不同网络类型的服务器名称的详细信息，请参阅“SQL Server 联机丛书”中的 SQL Server 安装文档。

### <a name="authentication-mode"></a>身份验证模式

请选择以下身份验证模式之一：
- 使用登录 ID 和密码的 SQL Server
- 使用当前登录用户帐户的 Windows 集成身份验证
- 使用登录 ID 和密码的 Active Directory 密码
- 使用当前登录用户帐户的 Active Directory 集成身份验证
- Active Directory 交互式身份验证与登录 ID
- 使用托管标识的托管服务标识身份验证
- 使用 Azure Active Directory 服务主体的 Active Directory 服务主体身份验证

有关身份验证模式的详细信息，请参阅[数据源向导屏幕 2](../../../connect/odbc/windows/dsn-wizard-2.md)。

### <a name="server-spn"></a>服务器 SPN

如果您使用受信任连接，则可为服务器指定服务主体名称 (SPN)。

### <a name="login-id"></a>登录 ID

如果“身份验证模式”设置为“SQL Server”、“Active Directory 密码”、“Active Directory 交互”或“托管服务标识”或“Active Directory 服务主体”，则指定用于连接的 SQL Server 或 Azure Active Directory 登录 ID     。 否则，将禁用“登录 ID”框。

### <a name="password"></a>密码

如果“身份验证模式”设置为“SQL Server”或“Active Directory 密码”，则指定用于连接的 SQL Server 或 Azure Active Directory 登录 ID 的密码。 否则，将禁用“密码”框。

### <a name="options"></a>选项

显示或隐藏“选项”  组。 如果“服务器”具有值，则启用“选项”   按钮。

### <a name="change-password"></a>更改密码

选中此框后，将显示“新密码”和“确认新密码”框。

### <a name="new-password"></a>新密码

指定新密码。

### <a name="confirm-new-password"></a>确认新密码

再次指定新密码以便确认。

### <a name="database"></a>数据库

指定将用于该连接的默认数据库。 此设置将覆盖为该服务器上的登录名指定的默认数据库。 如果未指定数据库，连接将使用为该服务器上的登录名指定的默认数据库。

### <a name="mirror-server"></a>镜像服务器

指定要镜像的数据库的故障转移伙伴的名称。

### <a name="mirror-spn"></a>镜像 SPN

您还可以指定镜像服务器的 SPN。 镜像服务器的 SPN 用于在客户端和服务器之间相互进行身份验证。

### <a name="language"></a>语言

指定用于 SQL Server 系统消息的国家/地区语言。 运行 SQL Server 的计算机必须安装该语言。 此设置将覆盖为该服务器上的登录名指定的默认语言。 如果未指定语言，连接将使用为该服务器上的登录名指定的默认语言。

### <a name="application-name"></a>应用程序名称

（可选）指定将要存储在 sys.sysprocesses 中该连接所在行的 program_name 列的应用程序名称。

### <a name="workstation-id"></a>工作站 ID

（可选）指定将要存储在 sys.sysprocesses 中该连接所在行的 hostname 列的工作站 ID。

### <a name="use-strong-encryption-for-data"></a>对数据使用强加密

如果选择此选项，将对通过连接传递的数据进行加密。 默认情况下，即使清除此复选框，也将对登录名加密。

### <a name="trust-server-certificate"></a>信任服务器证书

仅当启用“对数据使用强加密”时，此选项才适用。 选中后，将不会验证服务器的证书是否有服务器的正确主机名，以及是否由受信任的证书颁发机构颁发。

## <a name="see-also"></a>另请参阅

[Windows 上的 Microsoft ODBC Driver for SQL Server](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)
