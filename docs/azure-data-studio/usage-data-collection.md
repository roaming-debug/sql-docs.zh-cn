---
title: 启用或禁用使用情况数据收集和崩溃报告
description: 本文介绍如何控制是否收集使用情况和崩溃报告数据并将其发送给 Microsoft。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/24/2018
ms.openlocfilehash: ee687a6c6ab63386ad0e96e97ea86397703b53d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040517"
---
# <a name="enable-or-disable-usage-data-collection-for-azure-data-studio"></a>为 Azure Data Studio 启用或禁用使用情况数据收集

## <a name="how-to-disable-telemetry-reporting"></a>如何禁用遥测报告

Azure Data Studio 收集使用情况数据并将其发送给 Microsoft，以帮助改善我们的产品和服务。 若要了解更多，请阅读 [隐私声明](https://go.microsoft.com/fwlink/?LinkID=528096&clcid=0x409)。

如果不想将使用情况数据发送给 Microsoft，可以将“telemetry.enableTelemetry”设置设为“false” 。

若要以无提示的方式接收来自 Azure Data Studio 的所有遥测事件，可通过“文件” > “首选项” > “设置”，添加以下选项  ：

```json
    "telemetry.enableTelemetry": false
```

**重要通知**：此选项需要重启 Azure Data Studio 才能生效。 

## <a name="how-to-disable-crash-reporting"></a>如何禁用崩溃报告

若要禁用崩溃报告，可通过“文件” > 首选项” > 设置”，添加以下选项  ：

```json
    "telemetry.enableCrashReporter": false
```

**重要通知**：此选项需要重启 Azure Data Studio 才能生效。

## <a name="additional-resources"></a>其他资源
- [工作区和用户设置](settings.md)
