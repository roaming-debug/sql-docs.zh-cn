---
title: 向报表历史记录添加快照 - Reporting Services | Microsoft Docs
description: 详细了解如何在 SQL Server Reporting Services (SSRS) 中将快照手动添加到报表历史记录。
ms.prod: reporting-services
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 06/26/2019
ms.openlocfilehash: b11e4f129ea349e44c328b1f665a3935a937506a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021628"
---
# <a name="add-a-snapshot-to-report-history"></a>向报表历史记录添加快照

报表历史记录是随着时间变化而创建的报表快照的集合。 报表快照是包含在特定时间点检索到的布局信息以及查询结果的报表。 与按需运行报表（在选择该报表时可获得最新的查询结果）不同，报表快照按计划进行处理，再保存到报表服务器中。 当您选择报表快照进行查看时，报表服务器将在报表服务器数据库中检索存储的报表，然后显示快照创建时报表的数据和布局。  
  
报表快照不以特定的呈现格式进行保存。 相反，将以用户或应用程序发出请求时的最终查看格式（如 HTML）来呈现报表快照。 延迟呈现会使快照具有可移植性。 报表可以采用适用于请求设备或 Web 浏览器的正确格式呈现。  
  
## <a name="to-manually-add-snapshots-to-report-history"></a>向报表历史记录中手动添加快照
  
::: moniker range="=sql-server-2016"

1. 在报表管理器中，导航到“目录”页，将鼠标悬停在要查看其历史记录的项之上，然后单击下拉箭头。
  
2. 在下拉菜单中，单击 **“查看报表历史记录”** 。  
  
3. 单击 **“新建快照”** 。 将在 **“运行时间”** 列中创建一个新快照。  
    > [!NOTE]
    > 管理员必须将报表历史记录配置为“允许手动创建历史记录”，才能启用快照创建。 有关详细信息，请参阅 [限制报表历史记录（报表管理器）](../reports/limit-report-history-report-manager.md)。

4. 单击“应用”。
  
## <a name="to-automatically-add-all-snapshots-to-report-history"></a>自动向报表历史记录中添加所有快照  
  
1. 对于已配置为作为报表执行快照运行的报表，可以设置其他属性以在每次刷新该快照时将该快照的副本保存到报表历史记录。  
  
2. 在报表管理器中，导航到“目录”页，将鼠标悬停在要查看其历史记录的项之上，然后单击下拉箭头。  
  
3. 在下拉菜单中，单击 **“管理”** 。  
  
4. 单击 **“快照选项”** 。  
  
5. 选中 **“在历史记录中存储所有报表执行快照”** 复选框。  
  
6. 单击“应用”。  
  
## <a name="to-automatically-add-snapshots-to-report-history-based-on-a-schedule"></a>基于计划自动向报表历史记录中添加快照  
  
1. 在报表管理器中，导航到“目录”页，将鼠标悬停在要查看其历史记录的项之上，然后单击下拉箭头。  
  
2. 在下拉菜单中，单击 **“管理”** 。  
  
3. 单击 **“快照选项”** 。  
  
4. 选中 **“使用以下计划将快照添加到报表历史记录中”** 复选框。 执行以下某种方案：  
  
    - 选择“报表特定计划”。 填入计划详细信息，选择计划的开始日期和结束日期，再单击 **“确定”** 。  

    - 选择 **“共享计划”** 。 从列表中选择首选计划。  

5. 单击“应用”。  
  
## <a name="see-also"></a>另请参阅

- [配置报表的执行属性（报表管理器）](../../reporting-services/reports/configure-execution-properties-for-a-report-report-manager.md)
- [限制报表历史记录（报表管理器）](../../reporting-services/reports/limit-report-history-report-manager.md)
- [计划](../../reporting-services/subscriptions/schedules.md)   
- [报表管理器（SSRS 本机模式）](../web-portal-ssrs-native-mode.md)

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="to-manually-add-snapshots-to-report-history"></a>向报表历史记录中手动添加快照
  
1. 在 Web 门户中，转到要查看历史记录的项，并右键单击此项。  
  
2. 在下拉菜单中，选择“管理”。  
  
3. 选择“历史记录快照”选项卡。  
  
4. 在“历史记录快照”页上，选择“新建历史记录快照”。 此时，系统新建快照并在下面显示它（“创建时间”列中包含当前日期和时间）。  
  
    > [!NOTE]
    > 管理员必须将报表历史记录配置为“允许手动创建历史记录”，才能启用快照创建。 有关详细信息，请参阅[限制报表历史记录（Web 门户）](../../reporting-services/reports/limit-report-history-report-manager.md)。

## <a name="to-add-snapshots-via-a-schedule-to-report-history"></a>通过计划向报表历史记录添加快照的具体步骤

1. 在 Web 门户中，转到要查看历史记录的项，并右键单击此项。  
  
2. 在下拉菜单中，选择“管理”。  
  
3. 选择“历史记录快照”选项卡。  
  
4. 在“历史记录快照”页上，选择“计划和设置”按钮。  
  
5. 在“计划”部分中，选择下面的一个或两个选项（如果至少有一个选项尚未选择的话）：
    - **按计划创建历史记录快照**。  
    - **允许用户手动创建快照**。  
  
6. 在“高级”部分中，选中“保留所有历史记录快照”。  
  
7. （可选）选中“同时将缓存快照保存在报表历史记录中”复选框。  
  
8.  选择“应用”以保存设置。  

    > [!NOTE]  
    > 管理员必须将报表历史记录配置为“允许手动创建历史记录”，才能启用快照创建。 有关详细信息，请参阅[限制报表历史记录（Web 门户）](../../reporting-services/reports/limit-report-history-report-manager.md)。

9.  单击“应用”。

## <a name="to-automatically-add-all-snapshots-to-report-history"></a>自动向报表历史记录中添加所有快照  
  
1. 对于已配置为作为报表执行快照运行的报表，可以设置其他属性以在每次刷新该快照时将该快照的副本保存到报表历史记录。  
  
2. 在 Web 门户中，转到要查看历史记录的项，并右键单击此项。  
  
3. 在下拉菜单中，选择“管理”。  
  
4. 选择“历史记录快照”选项卡。  
  
5. 在“历史记录快照”页上，选择“计划和设置”按钮。  
  
6. 在“计划”部分中，选择下面的一个或两个选项（如果至少有一个选项尚未选择的话）：
    - **按计划创建历史记录快照**。  
    - **允许用户手动创建快照**。  
  
7. 在“高级”部分中，选中“保留所有历史记录快照”。  
  
8. （可选）选中“同时将缓存快照保存在报表历史记录中”复选框。  
  
9. 选择“应用”以保存设置。  
  
## <a name="to-automatically-add-snapshots-to-report-history-based-on-a-schedule"></a>基于计划自动向报表历史记录中添加快照  
  
1. 在 Web 门户中，转到要查看历史记录的项，并右键单击此项。  
  
2. 在下拉菜单中，选择“管理”。  
  
3. 选择“历史记录快照”选项卡。  
  
4. 在“历史记录快照”页上，选择“计划和设置”按钮。  
  
5. 选中 **“使用以下计划将快照添加到报表历史记录中”** 复选框。 执行以下某种方案：  
  
    - 选择“报表特定计划”。 填入计划详细信息，选择计划的开始日期和结束日期，再单击 **“确定”** 。  

    - 选择 **“共享计划”** 。 从列表中选择首选计划。  

5. 单击“应用”。  
  
## <a name="see-also"></a>另请参阅

- [配置报表的执行属性（Web 门户）](../../reporting-services/reports/configure-execution-properties-for-a-report-report-manager.md)
- [限制报表历史记录（Web 门户）](../../reporting-services/reports/limit-report-history-report-manager.md)
- [计划](../../reporting-services/subscriptions/schedules.md)   
- [Web 门户（SSRS 本机模式）](../web-portal-ssrs-native-mode.md)

::: moniker-end