---
title: 报表查看器控件的发行说明
description: 与 Reporting Services 相关的 WebForms 和 WinForms 报表查看器控件的发行说明。
ms.date: 11/30/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
ms.assetid: 112e0240-351d-46a9-98c7-2be09f26ac60
ms.reviewer: maggies
author: RhysSchmidtke
ms.author: rhys
ms.openlocfilehash: b6d1c768bb933036d3b23d37be20974515896a3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068152"
---
# <a name="release-notes-for-report-viewer-controls-for-webforms-and-winforms-of-ssrs"></a>适用于 SSRS 的 WebForms 和 WinForms 的报表查看器控件的发行说明

这些是与 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (SSRS) 相关的 WebForms 和 WinForms 报表查看器控件的发行说明。

有关 SSRS 的发行说明，请参阅 [SQL Server Reporting Services (SSRS) 2017 及更高版本的发行说明](../release-notes-reporting-services.md)。

## <a name="15014270"></a>150.1427.0
| 更改描述 | 详细信息 |
| :----------------- | :------ |
| Bug 修复 | 修复了在单击后不访问链接的问题。 |
|           | 修复了影响某些区域设置的日期/时间分析问题。 |
| &nbsp; | &nbsp; |

## <a name="15014040"></a>150.1404.0
| 更改描述 | 详细信息 |
| :----------------- | :------ |
| Bug 修复 | 修复了 WebForms 中工具栏的选项卡排序问题。 |
|           | 改进了表的 HTML 呈现辅助功能。 |
| &nbsp; | &nbsp; |

## <a name="15014000"></a>150.1400.0
| 更改描述 | 详细信息 |
| :----------------- | :------ |
| Bug 修复 | 修复了查看器控件在设计模式下无法加载的问题。 |
| &nbsp; | &nbsp; |

## <a name="15013580"></a>150.1358.0
| 更改描述 | 详细信息 |
| :----------------- | :------ |
| Bug 修复 | 还原了从项目引用中删除了 Microsoft.ReportViewer.Design 程序集的更改。 |
|           | 作为其他更改的一部分，两个程序集从 15.0 版本更改为 15.3。 已还原此更改。 |
| &nbsp; | &nbsp; |

## <a name="15013570"></a>150.1357.0
| 更改描述 | 详细信息 |
| :----------------- | :------ |
| Bug 修复  | 高 DPI 监视器的正确打印预览 |
|            | “打印”对话框将显示在可见区域之外 |
|            | 大量参数导致参数滚动条和下拉列表不能正常工作 |
|            | 修复了包含 Null 和日期时间参数的问题 |
|            | 已将 JQuery 更新至版本 3.3.1 |
|            | 修复了 HTML 呈现中与 tablix 单元重叠的问题 |
|            | 删除了设计时项目引用以消除添加到项目的错误 VS 程序集 |
|            | 工具栏的辅助功能修补程序只说明活动项目 |
| &nbsp; | &nbsp; |

## <a name="150900148"></a>150.900.148

| 更改描述 | 详细信息 |
| :----------------- | :------ |
| 防止不带参数通过 Server.LoadReportDefinition 加载报表的 bug 修复。  | &nbsp; |
| WebForms 报表查看器控件。 | 支持在 RTL 页面（使用 *direction: rtl;* css 属性更改文本流的页面）中进行嵌入。<br/><br/>支持通过 IReportViewerMessages5  本地化接口自定义打印对话框文本。<br/><br/>改进了辅助功能支持。<br/><br/>&bull; &nbsp; &nbsp; [WebForms 报表查看器控件的 NuGet 包](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Webforms/150.900.148) |
| WinForms 报表查看器控件。 | 应用在高 DPI 模式下运行时进行打印的修补程序。<br/><br/>&bull; &nbsp; &nbsp; [WinForms 报表查看器控件的 NuGet 包](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/150.900.148) |
| &nbsp; | &nbsp; |

## <a name="next-steps"></a>后续步骤

报表查看器控件[入门](integrating-reporting-services-using-reportviewer-controls-get-started.md)。

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)。
