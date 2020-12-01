---
description: 使用 Teradata 连接管理器
title: 使用 Teradata 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e0181cb68a68e7788d59f70d25c9b6935478f248
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425809"
---
# <a name="use-the-teradata-connection-manager"></a>使用 Teradata 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

使用 Teradata 连接管理器，你可以启用一个包从 Teradata 数据库中提取数据，并将数据加载到 Teradata 数据库中。

将 Teradata 连接管理器 `ConnectionManagerType` 属性设置为“TERADATA”。

## <a name="configure-the-teradata-connection-manager"></a>配置 Teradata 连接管理器

连接管理器配置更改在运行时由 Integration Services 解析。 若要添加与 Teradata 数据源的连接，请在“Teradata 连接管理器编辑器”窗格中完成信息。

![“Teradata 连接管理器编辑器”窗格](media/teradata-connection-manager.png)

1. 在“名称”框中，输入连接的名称。 默认名称为“Teradata 连接管理器”。

1. （可选）在“说明”框中，输入连接说明。

1. 在“服务器名称”框中，输入要连接到的 Teradata 服务器的名称。

1. 在“身份验证”下，执行以下任一操作：

   - 若要使用 Windows 身份验证，请选择“使用 Windows 身份验证”。
   - 若要使用 Teradata 数据库身份验证，请选择“使用 Teradata 身份验证”，然后输入以下身份验证类型的凭据：
     - 在“机制”框中，输入要使用的安全检查机制。 有效机制值包括 TD1、TD2、LDAP、KRB5、KRB5C、NTLM 和 NTLMC。
     - 在“参数”框中，输入所输入的安全检查机制所需的参数类型。
     - 在“用户名”框中，输入用于连接到 Teradata 数据库的用户名。  
     - 在“密码”框中，输入用户的 Teradata 数据库密码。

1. （可选）在“默认数据库”下拉列表中，选择要连接到的 Teradata 数据库。 如果此数据库访问权限不正确，则会显示错误，然后可以手动输入数据库名称。

1. （可选）在“帐户”中，输入与用户名对应的帐户的名称。 如果此值为空，则使用数据库的直属所有者帐户。
1. 选择“确定” 。

## <a name="custom-property"></a>自定义属性

自定义属性 `UseUTF8CharSet` 指定是否使用 UTF-8 字符集。 默认值为 *True*。

设置属性：

1. 打开 SQL Server Data Tools (SSDT)。
1. 在“连接管理器”区域中，右键单击“Teradata 连接管理器”，然后选择“属性”。
1. 在“属性”窗格中，对于 `UseUTF8CharSet` 属性，选择“True”或“False”。

## <a name="troubleshoot-the-teradata-connection-manager"></a>Teradata 连接管理器疑难解答

若要将 Teradata 连接管理器调用记录到 Teradata 开放式数据库连接（ODBC）驱动程序，请在 Windows ODBC 数据源管理器中启用 Windows ODBC 跟踪。

## <a name="next-steps"></a>后续步骤

- 配置 [Teradata 源](teradata-source.md)。
- 配置 [Teradata 目标](teradata-destination.md)。
- 如有任何疑问，请访问[技术社区](https://aka.ms/AA5u35j)。
