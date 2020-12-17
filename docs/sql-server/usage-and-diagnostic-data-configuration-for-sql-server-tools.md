---
title: 配置 SQL Server 工具使用情况和诊断数据收集 (CEIP) | Microsoft Docs
description: 了解 CEIP 从用户处收集的用于改善产品的信息。 查看如何在 SQL Server Data Tools (SSDT) 中选择加入或退出该程序。
ms.custom: ''
ms.date: 10/21/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: baf3a205-a6bb-4564-8b64-3a0475bb9273
author: stevestein
ms.author: sstein
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 8f12acd87a21cca6047621abfb985906295b58c1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461438"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-tools-ceip"></a>配置 SQL Server 工具使用情况和诊断数据收集 (CEIP)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

了解客户体验改善计划 (CEIP) 如何帮助 Microsoft 确定改善软件的方法。  可以配置工具以随时选择加入或退出。  
  
> [!NOTE]  
> 有关 Microsoft SQL Server 版本的用户数据集收集和使用方式的说明，请参考此[隐私声明](https://go.microsoft.com/fwlink/?LinkID=868444)。  
  
## <a name="opting-in-and-out-of-ceip-for-sql-server-data-tools"></a>选择加入和退出 SQL Server Data Tools 的 CEIP  

 客户体验改善计划是为了帮助 Microsoft 随着时间的推移改进其产品的计划。 此计划将收集有关计算机硬件和人们如何使用产品的信息，同时不会打断用户在计算机上的任务。 收集的信息可帮助 Microsoft 确定要改善的功能。 在本文档中，我们将介绍如何选择启用或禁用适用于 Visual Studio 2017、Visual Studio 2015 和 Visual Studio 2013 的 SQL Server Data Tools (SSDT) CEIP。  有关选择退出 SQL Server 的 CEIP 的信息，请参阅[禁用 SQL Server 的本地审核](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off)。

### <a name="choice-and-control-over--ceip-and-sql-server-data-tools-for-visual-studio-2017"></a>选择和控制适用于 Visual Studio 2017 的 CEIP 和 SQL Server Data Tools

 适用于 Visual Studio 2017 的 SSDT 是 SQL Server 2017 随附的数据建模工具。 它使用 Visual Studio 2017 中内置的 CEIP 选项。 可以参阅这篇 [Visual Studio 帮助文档](https://www.visualstudio.com/docs/work/connect/give-feedback)，详细了解如何在 Visual Studio 2017 中通过 CEIP 提交反馈。  
  
 对于 SQL Server 2017 预览版本，CEIP 默认启用。 可以按照下面的说明将其关闭，或重新打开。  
  
 **在 Visual Studio 中（适用于 Visual Studio 2017 的完整语言安装）**  
  
 如果在已有 Visual Studio 的计算机上运行 SSDT 安装程序，将仅添加 SQL Server 和 Business Intelligence 项目模板。 对于此方案，Visual Studio 提供的客户反馈选项可用于选择加入或退出 CEIP。  
  
1.  启动 Visual Studio。  
  
2.  在“帮助”菜单中，选择“发送反馈” > “设置”。  
  
3.  若要禁用 CEIP，请单击“否，我不想参加”，然后单击“确定”。  
  
     若要启用 CEIP，请单击“是，我愿意参加”，然后单击“确定”。  
  

  
 **使用基于注册表的策略或组策略**  
  
 如果在没有安装 Visual Studio 2017 的计算机上运行 SSDT 安装程序，只会安装 Visual Studio Shell。 Shell 中不提供客户反馈选项。 在这种情况下，注册表更新是唯一的选项来配置 CEIP  
  
 企业客户可以设置适用于 SQL Server 2017 的注册表策略，从而构造用于选择启用或禁用的组策略。  
  
 相关注册表项和设置如下所示：  
  
- 64 位 OS，项 = HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\VSCommon\15.0\SQM
- 32 位 OS，项 = HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\VSCommon\15.0\SQM

启用组策略时，项 = HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM 

条目 = OptIn

值 = (DWORD)
- 0 表示选择退出（关闭 VSCEIP）
- 1 表示选择加入（开启 VSCEIP）

  
> [!CAUTION]  
>  不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。 如果在应用手动更改之后遇到问题，也可以使用“最近一次的正确配置”启动选项。  
  
 有关 CEIP 收集、处理或传送的信息的详细信息，请参阅[隐私声明](https://go.microsoft.com/fwlink/?LinkID=868444)。  
 
### <a name="choice-and-control-over-ceip-and-sql-server-data-tools-for-visual-studio-2015"></a>对适用于 Visual Studio 2015 的 SQL Server Data Tools 和 CEIP 的选择与控制  
 适用于 Visual Studio 2015 的 SSDT 是附带 SQL Server 2016 的数据建模工具。 它使用 Visual Studio 2015 中内置的 CEIP 选项。 可以参阅这篇 [Visual Studio 帮助文档](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)，详细了解如何在 Visual Studio 2015 中通过 CEIP 提交反馈。  
  
 对于 SQL Server 2016 的预览版本，默认启用 CEIP。 可以按照下面的说明将其关闭，或重新打开。  
  
 **在 Visual Studio 中（适用于 Visual Studio 2015 的完整语言安装）**  
  
 如果在已有 Visual Studio 的计算机上运行 SSDT 安装程序，将仅添加 SQL Server 和 Business Intelligence 项目模板。 对于此方案，Visual Studio 提供的客户反馈选项可用于选择加入或退出 CEIP。  
  
1.  启动 Visual Studio。  
  
2.  在“帮助”菜单中，选择“发送反馈” > “设置”。  
  
3.  若要禁用 CEIP，请单击“否，我不想参加”，然后单击“确定”。  
  
     若要启用 CEIP，请单击“是，我愿意参加”，然后单击“确定”。  
  

  
 **使用基于注册表的策略或组策略**  
  
 如果在没有 Visual Studio 2015 的计算机上运行 SSDT 安装程序，将仅安装 Visual Studio Shell。 Shell 中不提供客户反馈选项。 在这种情况下，注册表更新是唯一的选项来配置 CEIP  
  
 企业客户可以通过设置适用于 SQL Server 2016 的基于注册表的策略构造组策略以选择加入或退出。  
  
 相关注册表项和设置如下所示：  
  
 Key = HKEY_CURRENT_USER\Software\Microsoft\VSCommon\14.0\SQM  
  
 RegEntry name = OptIn  
  
 注册表项类型 DWORD：  
  
-   0 表示选择不加入  
  
-   1 表示选择加入  
  
> [!CAUTION]  
>  不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。 如果在应用手动更改之后遇到问题，也可以使用“最近一次的正确配置”启动选项。  
  
 有关 CEIP 收集、处理或传送的信息的详细信息，请参阅[隐私声明](https://go.microsoft.com/fwlink/?LinkID=868444)。  
  
### <a name="choice-and-control-for-ceip-and-sql-server-data-tools---bi-ssdt-bi"></a>对 CEIP 和 SQL Server Data Tools - BI (SSDT-BI) 的选择和控制  
 如果你在使用 SSDT-BI，安装期间将有机会参与 CEIP。 稍后，可以通过客户端工具或编辑注册表设置来更改 SSDT-BI 的 CEIP 配置。  
  
 **在适用于 Visual Studio 2013 的 SSDT 和 SSDT-BI 中**  
  
1.  启动工具并打开 Analysis Services 或 Integration Services 的新的或现有项目。  
  
2.  从“帮助”菜单中，选择“Microsoft SQL Server 客户反馈选项” 。  
  
3.  要关闭 CEIP，单击“否，我不想参加” 。  
  
     要启用 CEIP，请单击“是，我愿意参加” 。  
  
4.  单击“确定”。  
  
 **使用基于注册表的策略或组策略**  
  
 企业客户可以通过设置适用于 SQL Server 2014 的基于注册表的策略构造组策略以选择加入或退出。  
  
 相关注册表项和设置如下所示：  
  
 注册表项 = HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\120  
  
 注册表项名称 = CustomerFeedback  
  
 注册表项类型 DWORD：  
  
-   0 表示选择不加入  
  
-   1 表示选择加入  
  
  
