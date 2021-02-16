---
description: 查找转换完全缓存模式 - OLE DB 连接管理器
title: 查找转换完全缓存模式 - OLE DB 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: c4150e1b-bdff-4f7a-af4c-3401c34def83
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7abcc50e3400e37219c73d8fbee0746557cc33da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350356"
---
# <a name="lookup-transformation-full-cache-mode---ole-db-connection-manager"></a>查找转换完全缓存模式 - OLE DB 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以将查找转换配置为使用完全缓存模式和 OLE DB 连接管理器。 在完全缓存模式下，在查找转换运行前，引用数据集会加载到缓存中。  
  
 查找转换通过将所连接数据源输入列中的数据和引用数据集中的列进行联接来执行查找。 有关详细信息，请参阅 [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md)。  
  
 在配置查找转换以使用 OLE DB 连接管理器时，可以选择表、视图或 SQL 查询以生成引用数据集。  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-ole-db-connection-manager"></a>在完全缓存模式下使用 OLE DB 连接管理器实现查找转换  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目，然后在解决方案资源管理器中双击该包。  
  
2.  在 **“数据流”** 选项卡上，从 **“工具箱”** 中将查找转换拖至设计图面。  
  
3.  将连接线从源或前一转换拖到查找转换，从而将查找转换连接到数据流。  
  
    > [!NOTE]  
    >  如果查找转换连接到包含空白日期字段的平面文件，该转换可能无效。 转换是否有效取决于平面文件的连接管理器是否配置为保留 Null 值。 若要确保查找转换有效，请在 **“连接管理器”** 页上的 **“平面文件源编辑器”** 中选择 **“在数据流中保留源中的 Null 值”** 选项。  
  
4.  双击源或前一转换以配置组件。  
  
5.  双击查找转换，在“查找转换编辑器”的“常规”页上，选择“完全缓存”。  
  
6.  在 **“连接类型”** 区域，选择 **“OLE DB 连接管理器”**。  
  
7.  在 **“指定如何处理无匹配项的行”** 列表中，为没有匹配项的行选择一个错误处理选项。  
  
8.  在 “连接” 页上，从 **“OLE DB 连接管理器”** 列表中选择一个连接管理器，或单击 **“新建”** 创建一个新的连接管理器。 有关详细信息，请参阅 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
9. 执行下列任务之一：  
  
    -   单击 **“使用表或视图”**，然后选择一个表或视图，或单击 **“新建”** 创建表或视图。  
  
         - 或 -  
  
    -   单击 **“使用 SQL 查询的结果”**，然后在 **“SQL 命令”** 窗口中生成查询，或者单击 **“生成查询”** ，使用 **查询生成器** 提供的图形工具生成查询。  
  
         - 或 -  
  
    -   或者，单击 **“浏览”** ，从文件中导入 SQL 语句。  
  
     若要验证 SQL 查询，请单击 **“分析查询”**。  
  
     若要查看数据的示例，请单击 **“预览”**。  
  
10. 单击 **“列”** 页，然后将至少一列从 **“可用输入列”** 列表中拖动到 **“可用查找列”** 列表中的列。  
  
    > [!NOTE]  
    >  查找转换自动映射具有相同名称和相同数据类型的列。  
  
    > [!NOTE]  
    >  列必须含有要映射的匹配数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
11. 执行以下任务在输出中包括查找列：  
  
    1.  在 **“可用查找列”** 列表中。 选择列。  
  
    2.  在 **“查找操作”** 列表中，指定查找列中的值是替换输入列中的值还是写入到新列。  
  
12. 若要配置错误输出，请单击 **“错误输出”** 页，并设置错误处理选项。 有关详细信息，请参阅[查找转换编辑器（“错误输出”页）](../data-flow/transformations/lookup-transformation.md)。  
  
13. 单击 **“确定”** 以保存对查找转换的更改，然后运行包。  
  
## <a name="see-also"></a>另请参阅  
 [在完全缓存模式下使用缓存连接管理器实现查找转换](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-cache-connection-manager.md)   
 [在不缓存模式或部分缓存模式下实现查找](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Integration Services 转换](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
