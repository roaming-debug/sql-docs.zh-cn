---
description: 通过使用 OLE DB 目标来加载数据
title: 通过使用 OLE DB 目标来加载数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- loading data
- OLE DB destination [Integration Services]
- destinations [Integration Services], OLE DB
ms.assetid: 78899498-725e-4300-a7af-f983f4ea384b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3165371fc1153636ae869b650468342e3614b024
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495819"
---
# <a name="load-data-by-using-the-ole-db-destination"></a>通过使用 OLE DB 目标来加载数据

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  若要添加和配置 OLE DB 目标，包中必须已包含至少一个数据流任务和一个源。  
  
### <a name="to-load-data-using-an-ole-db-destination"></a>使用 OLE DB 目标加载数据  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“数据流”** 选项卡，然后将 OLE DB 目标从 **“工具箱”** 拖动到设计图面。  
  
4.  将连接线从数据源或前一转换拖到目标，从而将 OLE DB 目标连接到数据流。  
  
5.  双击 OLE DB 目标。  
  
6.  在 **“OLE DB 目标编辑器”** 对话框中的 **“连接管理器”** 页上，选择现有的 OLE DB 连接管理器，或单击 **“新建”** 创建新的连接管理器。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
7.  选择数据访问方法：  
  
    -   **“表或视图”** 在包含该数据的数据库中选择一个表或视图。  
  
    -   **表或视图 - 快速加载** 选择数据库中包含该数据的表或视图，然后设置快速加载选项：“保留标识”、“保留空值”、“表锁”、“检查约束”、“每批行数”或“最大插入提交大小”。  
  
    -   **表名变量或视图名变量** 在数据库中选择包含表名称或视图名称的用户定义变量。  
  
    -   **表名变量或视图名变量快速加载** 在包含该数据的数据库中选择包含表名称或视图名称的用户定义变量，然后设置快速加载选项。  
  
    -   **“SQL 命令”** 键入 SQL 命令，或单击 **“生成查询”** 以使用 **查询生成器** 编写 SQL 命令。  
  
8.  单击 **“映射”** ，然后将列从一个列表拖动到另一个列表，从而将 **“可用输入列”** 列表中的列映射到 **“可用目标列”** 列表中的列。  
  
    > [!NOTE]  
    >  OLE DB 目标会自动映射同名的列。  
  
9. 若要配置错误输出，请单击 **“错误输出”** 。 有关详细信息，请参阅 [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md)。  
  
10. 单击“确定”。  
  
11. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [OLE DB 目标](../../integration-services/data-flow/ole-db-destination.md)   
 [Integration Services 转换](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Integration Services 路径](../../integration-services/data-flow/integration-services-paths.md)   
 [数据流任务](../../integration-services/control-flow/data-flow-task.md)  
  
  
