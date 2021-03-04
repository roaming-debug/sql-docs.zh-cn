---
title: SqlClient 中的 AppContext 开关
description: 介绍如何使用 SqlClient 中提供的 AppContext 开关。
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: aa6de0ff33b0d6a70ef14a42f5def3f3c6a20482
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836000"
---
# <a name="appcontext-switches-in-sqlclient"></a>SqlClient 中的 AppContext 开关

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

AppContext 类允许 SqlClient 提供新的功能，同时继续支持依赖于先前行为的调用方。 用户可通过设置特定的 AppContext 开关来选择退出一种行为更改。

## <a name="enabling-decimal-truncation-behavior"></a>启用小数截断行为

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

从 Microsoft.Data.SqlClient 2.0 开始，默认舍入十进制数据，这与 SQL Server 一样。 若要启用之前的截断行为，可在应用程序启动时将 AppContext 开关“Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal”设置为 `true`：

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

## <a name="enabling-managed-networking-on-windows"></a>在 Windows 上启用托管网络

[!INCLUDE [appliesto-xxxx-netcore-netst-md](../../includes/appliesto-xxxx-netcore-netst-md.md)]

在 Windows 上，SqlClient 默认使用 SNI 网络接口的本机实现。 若要支持使用托管 SNI 实现，可在应用程序启动时将 AppContext 开关“Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows”设置为 `true`：

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

此开关将切换驱动程序的行为，以便在 Windows 上的 .NET Core 2.1+ 和 .NET Standard 2.0+ 项目中使用托管的网络实现，从而消除对 Microsoft.Data.SqlClient 库的本机库的所有依赖项。 它仅用于测试和调试目的。

> [!NOTE]
> 与本机实现相比，两者存在一些已知的差异。 例如，托管实现不支持非域 Windows 身份验证。

## <a name="disabling-transparent-network-ip-resolution"></a>禁用透明网络 IP 解析

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

透明网络 IP 解析 (TNIR) 是对现有 MultiSubnetFailover 功能的修订。 如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，TNIR 就会影响驱动程序的连接序列。 TNIR 与 MultiSubnetFailover 交互，以提供下列三个连接序列：<br />
* 0：先尝试一个 IP，再并行尝试所有 IP
* 1：并行尝试所有 IP
* 2:逐一尝试所有 IP

|TransparentNetworkIPResolution|MultiSubnetFailover|行为|
|--------|--------|--------|
|True|True|1|
|正确|False|0|
|False|True|1|
|False|False|2|

TransparentNetworkIPResolution 默认处于启用状态。 MultiSubnetFailover 默认处于禁用状态。 若要禁用 TNIR，可在应用程序启动时将 AppContext 开关“Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString”设置为 `true`：

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString", true);
```

有关设置这些属性的详细信息，请参阅 [SqlConnection.ConnectionString 属性](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring)文档。 

## <a name="enable-a-minimum-timeout-during-login"></a>在登录期间启用最小超时

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

若要防止登录尝试无限期等待，可在应用程序启动时将 AppContext 开关“Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin”设置为 `true`：

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin", false);
```

## <a name="disable-blocking-behavior-of-readasync"></a>禁用 ReadAsync 的阻止行为

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

默认情况下，ReadAsync 以同步方式运行，并阻止 .NET Framework 上的调用线程。 若要禁用此阻止行为，可在应用程序启动时将 AppContext 开关“Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking”设置为 `false`：

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking", false);
```

## <a name="see-also"></a>另请参阅

[AppContext 类](/dotnet/api/system.appcontext?view=netcore-3.1&preserve-view=true)