---
title: SqlClient 驱动程序支持生命周期
description: 包含产品支持生命周期信息的页面。
ms.date: 11/19/2020
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 30155a584de4e22692601a1dcf9551a67d4f580f
ms.sourcegitcommit: 4c3949f620d09529658a2172d00bfe37aeb1a387
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2020
ms.locfileid: "95011789"
---
# <a name="sqlclient-driver-support-lifecycle"></a>SqlClient 驱动程序支持生命周期

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Microsoft.Data.SqlClient 库遵循适用于所有版本的最新 .NET Core 支持策略。

[查看 .NET Core 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

## <a name="microsoftdatasqlclient-release-cadence"></a>Microsoft.Data.SqlClient 发行说明

从 1.2 版开始，将每六个月发布一次新的稳定 (GA) 版本，中间会有 2 到 3 个预览版。 利益干系人和维护人员将根据一些资格和客户响应来选择长期支持 (LTS) 版本。

### <a name="release-life-cycles"></a>发布生命周期

| 版本 | 正式发布日期 | 最新修补程序版本 | 修补程序发布日期 | 支持级别  | 支持结束日期 |
| -- | -- | -- | -- | -- | -- |
| 2.1 | 2020 年 11 月 19 日 | 2.1.0 | 2020 年 11 月 19 日 | 当前 | |
| 2.0 | 2020 年 6 月 16 日 | 2.0.1 | 2020 年 8 月 25 日 | 当前 | |
| 1.1 | 2019 年 11 月 20 日 | 1.1.3 | 2020 年 5 月 15 日 | LTS | 2022 年 11 月 21 日 |
| 1.0 | 2019 年 8 月 28 日 | 1.0.19269.1 | 2019 年 9 月 26 日 | 当前 | 2020 年 2 月 20 日 |

### <a name="long-term-support-lts-releases"></a>长期支持 (LTS) 版本

LTS 版本在首次发布后的三年内受支持。

### <a name="current-releases"></a>当前版本

当前版本在后续的当前版本或 LTS 版本发布后的三个月内受支持。

## <a name="sql-version-compatibility-with-microsoftdatasqlclient"></a>SQL 与 Microsoft.Data.SqlClient 的版本兼容性

|数据库版本&nbsp;&#8594;<br />&#8595; 驱动程序版本|Azure SQL Database|Azure Synapse Analytics|Azure SQL 托管实例|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|
|---|---|---|---|---|---|---|---|---|
|2.1|是|是|是|是|是|是|是|是|
|2.0|是|是|是|是|是|是|是|是|
|1.1|是|是|是|是|是|是|是|是|
|1.0|是|是|是|是|是|是|是|是|
