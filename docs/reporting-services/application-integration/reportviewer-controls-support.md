---
title: 对报表查看器控件版本的支持
description: Microsoft Report Viewer 控件与遵循新式支持生命周期策略的 SQL Server Reporting Services 和 Power BI 报表服务器兼容。
author: maggiesMSFT
ms.custom: seo-lt-2019
ms.author: maggies
ms.reviewer: jonhp
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
ms.date: 12/01/2020
ms.openlocfilehash: fac7ebfc88432ef3bd8abde6c0e4e62115311520
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015637"
---
# <a name="support-for-report-viewer-current-branch-versions"></a>Report Viewer 当前分支版本支持

**适用范围：  Microsoft Report Viewer 版本 150.900.148 及更高版本**

Microsoft Report Viewer 控件与遵循 Microsoft 现代[支持生命周期策略](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy)的 SQL Server Reporting Services 和 Power BI 报表服务器兼容。 此信息适用于通过 [NuGet](https://www.nuget.org/) 分发的“ASP.NET”和“WinForms”版本。 所有已发布的版本都可通过 [NuGet](https://www.nuget.org/) 获得。 修补程序、功能或其他更新将回滚转发到最新版本。 必须应用最新版本才能接收更改。 Report Viewer 会继续接收“安全更新和关键更新”，并且至少提前一年通知任何支持的策略更改  。

有关 Report Viewer 控件的版本历史记录，请参阅以下链接：

- [Windows 窗体](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/)
- [ASP.NET Web 窗体](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/)

## <a name="application-server-and-report-server-combinations"></a>应用程序服务器和报表服务器组合

报表查看器控件的某些功能依赖于操作系统的默认行为。 因此，它们可能需要为客户端（运行报表查看器控件的应用程序服务器）和服务器（运行 Reporting Services）运行相同的版本。 支持以下应用程序服务器和报表服务器组合：

| 应用程序服务器 | 报表服务器 |
| :----------------- | :------ |
| Windows Server 2012 | Windows Server 2012 |
| Windows Server 2012 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 |
| Windows Server 2016 和更高版本 | Windows Server 2016 和更高版本 |

## <a name="next-steps"></a>后续步骤

有关报表查看器控件的详细信息，请参阅[开始使用报表查看器控件来集成 Reporting Services](integrating-reporting-services-using-reportviewer-controls-get-started.md)。
