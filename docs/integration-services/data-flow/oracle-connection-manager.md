---
description: Oracle Connection Manager
title: Oracle Connection Manager | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 428ca450371e452081d548a64e26dba2bd29b3b5
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430739"
---
# <a name="oracle-connection-manager"></a>Oracle Connection Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Oracle Connection Manager 用于启用包，以便从 Oracle Database 中提取数据，并将数据加载到 Oracle Database 中。

Oracle Connection Manager 的“ConnectionManagerType”属性设置为“ORACLE”。

## <a name="configuring-the-oracle-connection-manager"></a>配置 Oracle Connection Manager

Oracle Connection Manager 配置更改在运行时由 Integration Services 解析。 使用“Oracle Connection Manager 编辑器”对话框，可以添加对 Oracle 数据源的连接。

![连接管理器](media/oracle-connection-manager.png)

### <a name="options"></a>选项

#### <a name="connection-manager-information"></a>连接管理器信息

输入 Oracle 连接的相关信息。

**名称**

输入 Oracle 连接的名称。 默认名称为“Oracle Connection Manager”。 

**说明** 

输入连接的说明。 此为可选输入。

**TNS 服务名称**

输入要使用的 Oracle Database 的名称。 TNS 服务名称可以是：

- 在 tnsnames.ora 文件中定义的连接描述符名称，此文件位于 Oracle 客户端的 admin 文件夹中。

- EzConnect 格式：[//]host[:port][/service_name]

有关详细信息，请参阅 Oracle 文档。

#### <a name="connection-manager-logging"></a>连接管理器日志记录

请选择以下选项之一：

- **使用 Windows 身份验证**：选择此选项可以使用 Windows 身份验证。

- **使用 Oracle 身份验证**：选择此选项可以使用 Oracle Database 身份验证。 如果使用此身份验证，请按照以下方式输入 Oracle 凭据：  
    用户名：键入用于连接到 Oracle Database 的用户名。  
    密码：键入“用户名”字段中输入的用户的 Oracle Database 密码。

> [!NOTE]
>
>Oracle Server 18c 不支持 Windows 身份验证。

**测试连接**

单击“测试连接”，以验证所提供的信息是否正确。 如果输入的信息可以连接到 Oracle Database，就会看到消息“测试连接成功”。

### <a name="custom-properties"></a>自定义属性

Oracle Connection Manager 中有以下自定义连接管理器属性：

- **EnableDetailedTracing**：未使用。

- **OracleHome**：指定连接器要使用的 32 位 Oracle 主目录名称或文件夹。 (可选)

- **OracleHome64**：指定在 64 位模式下运行时，连接器要使用的 64 位 Oracle 主目录名称或文件夹。 (可选)

自定义属性未在 Oracle Connection Manager 编辑器中列出。 若要设置 OracleHome 和 OracleHome64 属性，请执行以下操作：

1. 在“连接管理器”区域中，右键单击要使用的 Oracle Connection Manager，再选择“属性”。

2. 在“属性”窗格中，将“OracleHome”或“OracleHome64”属性设置为 Oracle 主目录的完整路径。

## <a name="next-steps"></a>后续步骤

- 配置 [Oracle 源](oracle-source.md)。
- 配置 [Oracle 目标](oracle-destination.md)。
- 若有任何疑问，请访问 [TechCommunity](https://aka.ms/AA5u35j)。
