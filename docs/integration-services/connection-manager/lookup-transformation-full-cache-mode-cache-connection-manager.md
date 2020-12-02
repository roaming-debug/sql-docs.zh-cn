---
description: 查找转换完全缓存模式 - 缓存连接管理器
title: 查找转换完全缓存模式 - 缓存连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: 58bc7611-5fb5-4113-9742-10959e06b94c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2d897301336c7697b658fbf40837b23b0a2e3797
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91719769"
---
# <a name="lookup-transformation-full-cache-mode---cache-connection-manager"></a>查找转换完全缓存模式 - 缓存连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以将查找转换配置为使用完全缓存模式和缓存连接管理器。 在完全缓存模式下，在查找转换运行前，引用数据集会加载到缓存中。  
  
> [!NOTE]  
>  缓存连接管理器不支持二进制大型对象 (BLOB) 数据类型 DT_TEXT、DT_NTEXT 和 DT_IMAGE。 如果引用数据集包含 BLOB 数据类型，则运行包时该组件将失败。 可以使用 **“缓存连接管理器编辑器”** 修改列数据类型。 有关详细信息，请参阅 [Cache Connection Manager Editor](./cache-connection-manager.md)。  
  
 查找转换通过将所连接数据源输入列中的数据和引用数据集中的列进行联接来执行查找。 有关详细信息，请参阅 [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md)。  
  
 使用下列项之一来生成引用数据集：  
  
-   缓存文件 (.caw)  
  
     配置缓存连接管理器以便从现有缓存文件读取数据。  
  
-   数据流中的已连接数据源  
  
     使用“缓存转换”转换将数据流中已连接数据源的数据写入到缓存连接管理器。 这些数据始终存储在内存中。  
  
     必须将查找转换添加到一个单独的数据流中。 这样，在执行查找转换前，缓存转换会填充缓存连接管理器。 数据流可以位于同一个包中，也可以位于两个单独的包中。  
  
     如果数据流位于同一个包中，请使用优先约束来连接数据流。 这样，缓存转换就会在查找转换运行之前运行。  
  
     如果数据流位于单独的包中，则可使用执行包任务从父包调用子包。 在父包中的序列容器任务中添加多个执行包任务，可调用多个子包。  
  
 通过下面的任一方法，可在多个查找转换之间共享缓存中存储的引用数据集：  
  
-   在单个包中将查找转换配置为使用同一缓存连接管理器。  
  
-   在不同的包中将缓存连接管理器配置为使用同一缓存文件。  
  
 有关详细信息，请参阅下列主题：  
  
-   [缓存转换](../../integration-services/data-flow/transformations/cache-transform.md)  
  
-   [缓存连接管理器](../../integration-services/connection-manager/cache-connection-manager.md)  
  
-   [优先约束](../../integration-services/control-flow/precedence-constraints.md)  
  
-   [执行包任务](../../integration-services/control-flow/execute-package-task.md)  
  
-   [序列容器](../../integration-services/control-flow/sequence-container.md)  
  
 有关演示如何使用缓存连接管理器在完全缓存模式下实现查找转换的视频，请参阅 [如何在完全缓存模式下实现查找转换（SQL Server 视频）](/previous-versions/sql/sql-server-2008/cc952929(v=sql.100))。  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-one-package-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>在完全缓存模式下使用缓存连接管理器和数据流中的数据源在单个包中实现查找转换  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开一个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目，再打开一个包。  
  
2.  在 **“控制流”** 选项卡上，添加两个数据流任务，然后使用绿色连接线来连接任务：  
  
3.  在第一个数据流中，添加“缓存转换”转换，然后将该转换连接到数据源。  
  
     根据需要配置数据源。  
  
4.  双击缓存转换，在“缓存转换编辑器”  中的“连接管理器”  页上单击“新建”  ，创建一个新的缓存连接管理器。  
  
5.  单击 **“缓存连接管理器编辑器”** 对话框的 **“列”** 选项卡，使用 **“索引位置”** 选项指定哪些列是索引列。  
  
     对于非索引列，索引位置是 0。 对于索引列，索引位置是连续的正数。  
  
    > [!NOTE]  
    >  当将查找转换配置为使用缓存连接管理器时，则仅引用数据集中的索引列能够映射到输入列。 此外，还必须对所有索引列进行映射。 有关详细信息，请参阅 [Cache Connection Manager Editor](./cache-connection-manager.md)。  
  
6.  若要将缓存保存到文件，请在 **“缓存连接管理器编辑器”** 中的 **“常规”** 选项卡上，设置以下选项来配置缓存连接管理器：  
  
    -   选择 **“使用文件缓存”** 。  
  
    -   对于 **“文件名”**，请键入文件路径，或者单击 **“浏览”** 选择文件。  
  
         如果为其键入路径的文件不存在，则运行包时系统会创建该文件。  
  
    > [!NOTE]  
    >  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
7.  根据需要配置缓存转换。 有关详细信息，请参阅[缓存转换编辑器（“连接管理器”页）](../data-flow/transformations/cache-transform.md)和[缓存转换编辑器（“映射”页）](../data-flow/transformations/cache-transform.md)。  
  
8.  在第二个数据流中，添加一个查找转换，然后执行以下任务来配置转换：  
  
    1.  将连接线从源或前一转换拖到查找转换，从而将查找转换连接到数据流。  
  
        > [!NOTE]  
        >  如果查找转换连接到包含空白日期字段的平面文件，该转换可能无效。 转换是否有效取决于平面文件的连接管理器是否配置为保留 Null 值。 若要确保查找转换有效，请在 **“连接管理器”** 页上的 **“平面文件源编辑器”** 中选择 **“在数据流中保留源中的 Null 值”** 选项。  
  
    2.  双击源或前一转换以配置组件。  
  
    3.  双击查找转换，在“查找转换编辑器”的“常规”页上，选择“完全缓存”。  
  
    4.  在 **“连接类型”** 区域，选择 **“缓存连接管理器”**。  
  
    5.  在 **“指定如何处理没有匹配项的行”** 列表中，选择一个错误处理选项。  
  
    6.  在 **“连接”** 页的 **“缓存连接管理器”** 列表中，选择缓存连接管理器。  
  
    7.  单击 **“列”** 页，然后将至少一列从 **“可用输入列”** 列表中拖动到 **“可用查找列”** 列表中的列。  
  
        > [!NOTE]  
        >  查找转换自动映射具有相同名称和相同数据类型的列。  
  
        > [!NOTE]  
        >  列必须含有要映射的匹配数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
    8.  在 **“可用查找列”** 列表中，选择列。 在 **“查找操作”** 列表中，指定查找列中的值是替换输入列中的值还是写入到新列。  
  
    9. 若要配置错误输出，请单击 **“错误输出”** 页，并设置错误处理选项。 有关详细信息，请参阅[查找转换编辑器（“错误输出”页）](../data-flow/transformations/lookup-transformation.md)。  
  
    10. 单击 **“确定”** 保存对查找转换的更改。  
  
9. 运行包。  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-two-packages-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>在完全缓存模式下使用缓存连接管理器和数据流中的数据源在两个包中实现查找转换  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开一个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目，再打开两个包。  
  
2.  在每个包中的“控制流”选项卡上，添加数据流任务。  
  
3.  在父包中，向数据流添加“缓存转换”转换，然后将该转换连接到数据源。  
  
     根据需要配置数据源。  
  
4.  双击缓存转换，在“缓存转换编辑器”  中的“连接管理器”  页上单击“新建”  ，创建一个新的缓存连接管理器。  
  
5.  在 **“缓存连接管理器编辑器”** 中的 **“常规”** 选项卡上，设置以下选项来配置缓存连接管理器：  
  
    -   选择 **“使用文件缓存”** 。  
  
    -   对于 **“文件名”**，请键入文件路径，或者单击 **“浏览”** 选择文件。  
  
         如果为其键入路径的文件不存在，则运行包时系统会创建该文件。  
  
    > [!NOTE]  
    >  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
6.  单击 **“列”** 选项卡，然后使用 **“索引位置”** 选项来指定哪些列是索引列。  
  
     对于非索引列，索引位置是 0。 对于索引列，索引位置是连续的正数。  
  
    > [!NOTE]  
    >  当将查找转换配置为使用缓存连接管理器时，则仅引用数据集中的索引列能够映射到输入列。 此外，还必须对所有索引列进行映射。 有关详细信息，请参阅 [Cache Connection Manager Editor](./cache-connection-manager.md)。  
  
7.  根据需要配置缓存转换。 有关详细信息，请参阅[缓存转换编辑器（“连接管理器”页）](../data-flow/transformations/cache-transform.md)和[缓存转换编辑器（“映射”页）](../data-flow/transformations/cache-transform.md)。  
  
8.  执行下列操作之一以填充第二个包中使用的缓存连接管理器：  
  
    -   运行父包。  
  
    -   双击在步骤 4 中创建的缓存连接管理器，单击“列”，选择行，然后按 Ctrl+C 复制列元数据。  
  
9. 在子包中，右键单击“连接管理器”区域，单击“新建连接”，在“添加 SSIS 连接管理器”对话框中选择“缓存”，单击“添加”，即可创建缓存连接管理器。  
  
     **“连接管理器”** 区域显示在 **设计器的**“控制流” **、**“数据流” **和** “事件处理程序” [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 选项卡的底部。  
  
10. 在 **“缓存连接管理器编辑器”** 中的 **“常规”** 选项卡上，设置以下选项对缓存连接管理器进行配置，以便从所选的缓存文件中读取数据：  
  
    -   选择 **“使用文件缓存”** 。  
  
    -   对于 **“文件名”**，请键入文件路径，或者单击 **“浏览”** 选择文件。  
  
    > [!NOTE]  
    >  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
11. 如果在步骤 8 中复制了列元数据，请单击“列”，选择空行，然后按 Ctrl+V 粘贴列元数据。  
  
12. 添加查找转换，然后执行以下操作来配置转换：  
  
    1.  将连接线从源或前一转换拖到查找转换，从而将查找转换连接到数据流。  
  
        > [!NOTE]  
        >  如果查找转换连接到包含空白日期字段的平面文件，该转换可能无效。 转换是否有效取决于平面文件的连接管理器是否配置为保留 Null 值。 若要确保查找转换有效，请在 **“连接管理器”** 页上的 **“平面文件源编辑器”** 中选择 **“在数据流中保留源中的 Null 值”** 选项。  
  
    2.  双击源或前一转换以配置组件。  
  
    3.  双击查找转换，在“查找转换编辑器”的“常规”页上选择“完全缓存”。  
  
    4.  在 **“连接类型”** 区域，选择 **“缓存连接管理器”** 。  
  
    5.  在 **“指定如何处理没有匹配项的行”** 列表中，为没有匹配项的行选择一个错误处理选项。  
  
    6.  在 **“连接”** 页的 **“缓存连接管理器”** 列表中，选择您添加的缓存连接管理器。  
  
    7.  单击 **“列”** 页，然后将至少一列从 **“可用输入列”** 列表中拖动到 **“可用查找列”** 列表中的列。  
  
        > [!NOTE]  
        >  查找转换自动映射具有相同名称和相同数据类型的列。  
  
        > [!NOTE]  
        >  列必须含有要映射的匹配数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
    8.  在 **“可用查找列”** 列表中，选择列。 在 **“查找操作”** 列表中，指定查找列中的值是替换输入列中的值还是写入到新列。  
  
    9. 若要配置错误输出，请单击 **“错误输出”** 页，并设置错误处理选项。 有关详细信息，请参阅[查找转换编辑器（“错误输出”页）](../data-flow/transformations/lookup-transformation.md)。  
  
    10. 单击 **“确定”** 保存对查找转换的更改。  
  
13. 打开父包，向控制流添加一个执行包任务，然后配置任务以调用子包。 有关详细信息，请参阅 [Execute Package Task](../../integration-services/control-flow/execute-package-task.md)。  
  
14. 运行包。  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-cache-connection-manager-and-an-existing-cache-file"></a>在完全缓存模式下使用缓存连接管理器和现有缓存文件实现查找转换  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开一个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目，再打开一个包。  
  
2.  右键单击“连接管理器”区域，然后单击“新建连接”。  
  
     **“连接管理器”** 区域显示在 **设计器的**“控制流” **、**“数据流” **和** “事件处理程序” [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 选项卡的底部。  
  
3.  在 **“添加 SSIS 连接管理器”** 对话框中，选择 **CACHE**，单击 **“添加”** 添加一个缓存连接管理器。  
  
4.  双击缓存连接管理器以打开“缓存连接管理器编辑器”。  
  
5.  在 **“缓存连接管理器编辑器”** 中的 **“常规”** 选项卡上，设置以下选项来配置缓存连接管理器：  
  
    -   选择 **“使用文件缓存”** 。  
  
    -   对于 **“文件名”**，请键入文件路径，或者单击 **“浏览”** 选择文件。  
  
    > [!NOTE]  
    >  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
6.  单击 **“列”** 选项卡，然后使用 **“索引位置”** 选项来指定哪些列是索引列。  
  
     对于非索引列，索引位置是 0。 对于索引列，索引位置是连续的正数。  
  
    > [!NOTE]  
    >  当将查找转换配置为使用缓存连接管理器时，则仅引用数据集中的索引列能够映射到输入列。 此外，还必须对所有索引列进行映射。 有关详细信息，请参阅 [Cache Connection Manager Editor](./cache-connection-manager.md)。  
  
7.  在 **“控制流”** 选项卡上，向包添加一个数据流任务，然后向数据流添加一个查找转换。  
  
8.  执行以下操作来配置查找转换：  
  
    1.  将连接线从源或前一转换拖到查找转换，从而将查找转换连接到数据流。  
  
        > [!NOTE]  
        >  如果查找转换连接到包含空白日期字段的平面文件，该转换可能无效。 转换是否有效取决于平面文件的连接管理器是否配置为保留 Null 值。 若要确保查找转换有效，请在 **“连接管理器”** 页上的 **“平面文件源编辑器”** 中选择 **“在数据流中保留源中的 Null 值”** 选项。  
  
    2.  双击源或前一转换以配置组件。  
  
    3.  双击查找转换，在“查找转换编辑器”的“常规”页上，选择“完全缓存”。  
  
    4.  在 **“连接类型”** 区域，选择 **“缓存连接管理器”** 。  
  
    5.  在 **“指定如何处理没有匹配项的行”** 列表中，为没有匹配项的行选择一个错误处理选项。  
  
    6.  在 **“连接”** 页的 **“缓存连接管理器”** 列表中，选择您添加的缓存连接管理器。  
  
    7.  单击 **“列”** 页，然后将至少一列从 **“可用输入列”** 列表中拖动到 **“可用查找列”** 列表中的列。  
  
        > [!NOTE]  
        >  查找转换自动映射具有相同名称和相同数据类型的列。  
  
        > [!NOTE]  
        >  列必须含有要映射的匹配数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
    8.  在 **“可用查找列”** 列表中，选择列。 在 **“查找操作”** 列表中，指定查找列中的值是替换输入列中的值还是写入到新列。  
  
    9. 若要配置错误输出，请单击 **“错误输出”** 页，并设置错误处理选项。 有关详细信息，请参阅[查找转换编辑器（“错误输出”页）](../data-flow/transformations/lookup-transformation.md)。  
  
    10. 单击 **“确定”** 保存对查找转换的更改。  
  
9. 运行包。  
  
## <a name="see-also"></a>另请参阅  
 [在完全缓存模式下使用 OLE DB 连接管理器实现查找转换](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-ole-db-connection-manager.md)   
 [在不缓存模式或部分缓存模式下实现查找](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Integration Services 转换](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
