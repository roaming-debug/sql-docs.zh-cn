---
title: 配置报表的执行属性 - Reporting Services | Microsoft Docs
description: 了解如何设置报表处理选项以指定何时检索报表数据，以避免每次请求报表时检索相同数据的开销。
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- report execution properties [Reporting Services]
- reports [Reporting Services], properties
- reports [Reporting Services], execution options
ms.assetid: 73cc8dcc-ef80-40d7-9739-d33bba0eb28a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9b1cecda6c4ec085efe7b18392eb06f06866352b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466558"
---
# <a name="configure-execution-properties-for-a-report"></a>配置报表的执行属性
  可以设置报表处理选项以指定何时为报表检索数据。 如果外部数据源按照特定的时间刷新（例如，每天或每周刷新数据仓库），并且您想要避免每当请求报表时对同一数据进行检索的开销，则为报表计划数据处理非常有用。 如果您想要控制外部数据库服务器上的处理负载，或者如果想要为必须使用相同数据集的多个用户提供一致的结果，则计划数据处理也很有用。 使用可变数据的按需运行报表每分钟都会生成不同的结果。 相反，报表快照则可用来针对包含同一时间点数据的其他报表或分析工具进行有效比较。  
  
 报表快照是一个报表，其中包含在特定时间点检索到的布局说明以及查询结果。 与按需运行报表（在选择该报表时可获得最新的查询结果）不同，报表快照按计划进行处理，再保存到报表服务器中。 当您选择报表快照进行查看时，报表服务器将在报表服务器数据库中检索存储的报表，然后显示快照创建时报表的数据和布局。  
  
 报表快照不以特定的呈现格式进行保存。 相反，将以用户或应用程序发出请求时的最终查看格式（如 HTML）来呈现报表快照。 延迟呈现会使快照具有可移植性。 报表可以采用适用于请求设备或 Web 浏览器的正确格式呈现。  

::: moniker range="=sql-server-2016"
  
## <a name="to-configure-report-processing-options"></a>配置报表处理选项  
  
1.  启动 [报表管理器（SSRS 本机模式）](../web-portal-ssrs-native-mode.md)。  
  
2.  导航到并打开要设置处理选项的报表。  
  
 悬停在该报表之上，然后单击下拉箭头。  
  
1.  在下拉菜单中，单击“管理”，然后选择“处理选项”选项卡   。  
  
2.  单击 **“通过执行快照呈现此报表”** ，然后选择以下选项之一：  
  
    -   如果要创建快照，则选择“使用下列计划创建报表执行快照”，再定义特定于报表的计划，或从“共享计划”列表中进行选择   。  
  
    -   如果希望立即创建快照，请选择 **“在此页上单击‘应用’按钮后创建报表快照”** 。  
  
3.  单击“应用”  。  
  
## <a name="see-also"></a>另请参阅  
 [设置报表处理属性](../../reporting-services/report-server/set-report-processing-properties.md)   
 [“内容”页（报表管理器）](/previous-versions/sql/sql-server-2016/ms186470(v=sql.130))   
 [报表服务器内容管理（SSRS 本机模式）](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [“处理选项”属性页（报表管理器）](/previous-versions/sql/sql-server-2016/ms178821(v=sql.130))  
  
::: moniker-end

::: moniker range=">=sql-server-2017"
  
## <a name="to-configure-report-execution-properties"></a>配置报表执行属性  
  
通过[报表服务器的 Web 门户（SSRS 本机模式）](../../reporting-services/web-portal-ssrs-native-mode.md)：  
  
1. 导航到要配置执行属性的报表。  
  
2. 右键单击报表，然后从下拉菜单中选择“管理”  。

3. 选择“历史记录快照”  选项卡以显示“历史记录快照”  页。  
  
4. 选择“计划和设置”  按钮，并选中“按计划创建历史记录快照”  （如果尚未选中）。
  
5. 根据需要选择“共享计划”  或“报表特定计划”  。  
  
6. 在“高级”  部分中，为历史记录快照选择所需的“保留”  策略。  
  
7. 选择“应用”。   
  
   >[!NOTE]
   >如果希望立即创建快照，请选择“新建历史记录快照”  按钮，而不是“计划和设置”  按钮，此时将立即创建报表快照。  
  
## <a name="see-also"></a>另请参阅  
 [设置报表处理属性](../../reporting-services/report-server/set-report-processing-properties.md)   
 [报表服务器内容管理（SSRS 本机模式）](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [设置报表处理属性](../../reporting-services/report-server/set-report-processing-properties.md)   

::: moniker-end