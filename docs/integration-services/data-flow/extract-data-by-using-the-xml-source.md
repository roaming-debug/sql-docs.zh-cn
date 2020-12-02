---
description: 使用 XML 源提取数据
title: 使用 XML 源提取数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- extracting data [Integration Services]
- sources [Integration Services], XML
- XML source [Integration Services]
ms.assetid: 5d5be54c-2b7e-4957-9193-c5ea5c5d6d15
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f3cd306d2cf6097fdb5e178f92e41d45de198783
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127175"
---
# <a name="extract-data-by-using-the-xml-source"></a>使用 XML 源提取数据

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  若要添加和配置 XML 源，包中必须已经包含至少一个数据流任务。  
  
### <a name="to-extract-data-using-an-xml-source"></a>使用 XML 源提取数据  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“数据流”** 选项卡，然后从 **“工具箱”** 把 XML 源拖动到设计图面。  
  
4.  双击此 XML 源。  
  
5.  在 **“XML 源编辑器”** 的 **“连接管理器”** 页上，选择数据访问模式：  
  
    -   对于 **“XML 文件位置”** 访问模式，单击 **“浏览”** 找到包含该 XML 文件的文件夹。  
  
    -   对于“来自变量的 XML 文件”访问模式，选择包含 XML 文件路径的用户定义变量。  
  
    -   对于“来自变量的 XML 数据”访问模式，选择包含 XML 数据的用户定义变量。  
  
    > [!NOTE]  
    >  这些变量必须在包含该 XML 源的数据流任务的作用域内定义，或者在包的作用域内定义；此外，变量的数据类型必须为字符串。  
  
6.  根据需要，也可以选择 **“使用内联架构”** 以指示 XML 文档包含架构信息。  
  
7.  若要为 XML 文件指定外部 XML 架构定义语言 (XSD) 架构，请执行下列操作之一：  
  
    -   单击 **“浏览”** 找到现有的 XSD 文件。  
  
    -   单击 **“生成 XSD”** 从 XML 文件创建 XSD。  
  
8.  若要更新输出列的名称，请单击 **“列”** 并在 **“输出列”** 列表中编辑这些值。  
  
9. 若要配置错误输出，请单击 **“错误输出”** 。 有关详细信息，请参阅 [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md)。  
  
10. 单击“确定”。  
  
11. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [XML 源](../../integration-services/data-flow/xml-source.md)   
 [Integration Services 转换](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Integration Services 路径](../../integration-services/data-flow/integration-services-paths.md)   
 [数据流任务](../../integration-services/control-flow/data-flow-task.md)  
  
  
