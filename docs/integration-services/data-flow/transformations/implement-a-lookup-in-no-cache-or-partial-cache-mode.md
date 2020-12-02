---
description: 在不缓存模式或部分缓存模式下实现查找
title: 在不缓存模式或部分缓存模式下实现查找 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation
- match exactly [Integration Services]
- lookups [Integration Services]
- exact matches [Integration Services]
ms.assetid: 01b7fbca-5181-4d47-9f75-7f25af6b40d2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f971d0788b33420495f1ae7ceeaca56599c8e239
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194161"
---
# <a name="implement-a-lookup-in-no-cache-or-partial-cache-mode"></a>在不缓存模式或部分缓存模式下实现查找

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  可以将查找转换配置为使用部分缓存模式或不缓存模式：  
  
-   部分缓存  
  
     将在引用数据集中有匹配项的行存储在缓存中，也可以将在数据集中没有匹配项的行存储在缓存中。 超出缓存的内存大小后，查找转换会自动从缓存中删除最不常使用的行。  
  
-   不缓存  
  
     不向缓存中加载任何数据。  
  
 不论选择部分缓存还是不缓存，均需要使用 OLE DB 连接管理器连接到引用数据集。 引用数据集是在执行查找转换过程中使用表、视图或 SQL 查询生成的。  
  
### <a name="to-implement-a-lookup-transformation-in-no-cache-or-partial-cache-mode"></a>在不缓存模式或部分缓存模式下实现查找转换  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目，再打开该包。  
  
2.  在 **“数据流”** 选项卡上，添加查找转换。  
  
3.  将连接线从源或前一转换拖到查找转换，从而将查找转换连接到数据流。  
  
    > [!NOTE]  
    >  如果配置为使用无缓存模式的查找转换连接到包含空白日期字段的平面文件，该转换可能无效。 转换是否有效取决于平面文件的连接管理器是否配置为保留 Null 值。 若要确保查找转换有效，请在 **“连接管理器”** 页上的 **“平面文件源编辑器”** 中选择 **“在数据流中保留源中的 Null 值”** 选项。  
  
4.  双击源或前一转换以配置组件。  
  
5.  双击查找转换，然后在“查找转换编辑器”中的“常规”页上，选择“部分缓存”或“不缓存”。  
  
6.  请从 **“指定如何处理没有匹配项的行”** 列表中选择一个错误处理选项。  
  
7.  在 **“连接”** 页上，从 **“OLE DB 连接管理器”** 列表中选择一个连接管理器，或单击 **“新建”** 创建一个新的连接管理器。 有关详细信息，请参阅 [OLE DB Connection Manager](../../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
8.  执行以下步骤中的一个：  
  
    -   单击 **“使用表或视图”**，然后选择一个表或视图，或单击 **“新建”** 创建表或视图。  
  
    -   单击 **“使用 SQL 查询的结果”**，然后在 **“SQL 命令”** 窗口中生成查询。  
  
         \- 或 -  
  
         单击 **“生成查询”** ，使用 **“查询生成器”** 提供的图形工具生成一个查询。  
  
         \- 或 -  
  
         单击 **“浏览”** 从文件中导入 SQL 语句。  
  
     若要验证 SQL 查询，请单击 **“分析查询”**。  
  
     若要查看数据的示例，请单击 **“预览”**。  
  
9. 单击 **“列”** 页，然后将至少一列从 **“可用输入列”** 列表中拖动到 **“可用查找列”** 列表中的列。  
  
    > [!NOTE]  
    >  查找转换自动映射具有相同名称和相同数据类型的列。  
  
    > [!NOTE]  
    >  列必须含有要映射的匹配数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)。  
  
10. 执行以下步骤在输出中包括查找列：  
  
    1.  从 **“可用查找列”** 列表中，选择列。  
  
    2.  在 **“查找操作”** 列表中，指定查找列中的值是替换输入列中的值还是写入到新列。  
  
11. 如果在步骤 5 中选择了 **“部分缓存”** ，则在 **“高级”** 页上，设置以下缓存选项：  
  
    -   从“缓存大小(32 位)”列表中，为 32 位环境选择缓存大小。  
  
    -   从“缓存大小(64 位)”列表中，为 64 位环境选择缓存大小。  
  
    -   若要缓存在引用中有匹配项的行，请选择 **“为没有匹配项的行启用缓存”**。  
  
    -   在 **“从缓存分配”** 列表中，选择用于存储没有匹配项的行的缓存百分比。  
  
12. 若要修改生成引用数据集的 SQL 语句，请选择 **“修改 SQL 语句”**，然后更改文本框中显示的 SQL 语句。  
  
     如果语句中包含参数，请单击 **“参数”** 以将这些参数映射到输入列。  
  
    > [!NOTE]  
    >  在此页上指定的可选 SQL 语句将覆盖并替换在 **“查找转换编辑器”** 的 **“高级”** 页上指定的表名。  
  
13. 若要配置错误输出，请单击 **“错误输出”** 页，并设置错误处理选项。 有关详细信息，请参阅[查找转换编辑器（“错误输出”页）](./lookup-transformation.md)。  
  
14. 单击 **“确定”** 以保存对查找转换的更改，然后运行包。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
