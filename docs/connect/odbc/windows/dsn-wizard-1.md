---
description: 了解如何在数据源向导中定义名称和说明，以创建与 SQL Server 的新 ODBC 连接。
title: 数据源向导屏幕 1 (ODBC Driver for SQL Server)
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c7b60adb82915d8fb73138d194a125f478c0a039
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673941"
---
# <a name="data-source-wizard-screen-1"></a>数据源向导屏幕 1

指定数据源的名称和说明，以及该数据源要连接的运行 SQL Server 的服务器名称。

## <a name="options"></a>选项

### <a name="name"></a>名称

ODBC 应用程序请求与数据源连接时使用的数据源名称。 例如，“Personnel”。 该数据源名称显示在“ODBC 数据源管理器”对话框中。

### <a name="description"></a>说明

（可选）数据源的说明。 例如，“所有雇员的雇佣日期、发薪记录和当前审核”。

### <a name="select-or-enter-a-server-name"></a>选择或输入服务器名称

网络上的 SQL Server 实例的名称。 您需要在下一个编辑框中指定一个服务器。

大多数情况下，ODBC 驱动程序可通过使用此框中提供的默认协议顺序和服务器名称进行连接。 如果要创建服务器别名或配置客户端网络库，请使用 SQL Server 配置管理器。

当使用与 SQL Server 相同的计算机时，你可以在服务器框中输入“(local)”。 即使正在运行非联网版的 SQL Server，用户也可以连接到 SQL Server 的本地实例。 在同一台计算机上可以运行 SQL Server 的多个实例。 若要指定 SQL Server 的命名实例，则将服务器名称指定为 _ServerName_\\_InstanceName_。

有关不同网络类型的服务器名称的详细信息，请参阅[登录到 SQL Server](../../../database-engine/configure-windows/logging-in-to-sql-server.md#format-for-specifying-the-name-of-sql-server)。

### <a name="finish"></a>完成

如果此屏幕上指定的信息为连接到 SQL Server 所需的全部信息，则可以选择“完成”。 对于在向导的其他屏幕上指定的所有属性都使用默认值。

### <a name="next"></a>下一步

若要前进到向导的下一个屏幕，请选择“下一步”。

## <a name="next-steps"></a>后续步骤

[数据源向导屏幕 2](dsn-wizard-2.md)
