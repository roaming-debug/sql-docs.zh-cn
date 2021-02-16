---
description: 缓存连接管理器
title: 缓存连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.cacheconnection.f1
helpviewer_keywords:
- Cache connection manager
ms.assetid: bdc92038-3720-4795-8a5c-79b963f2c952
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fba6ab0ff6c58228f702983b6338c2bd9965e5e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338442"
---
# <a name="cache-connection-manager"></a>缓存连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  缓存连接管理器从缓存转换或从缓存文件 (.caw) 中读取数据，并可将数据保存到缓存文件。 无论是否将缓存连接管理器配置为使用缓存文件，数据都会始终存储在内存中。  
  
 “缓存转换”转换可以将数据流中已连接数据源的数据写入缓存连接管理器。 包中的查找转换会对数据执行查找。  
  
> [!NOTE]  
>  缓存连接管理器不支持二进制大型对象 (BLOB) 数据类型 DT_TEXT、DT_NTEXT 和 DT_IMAGE。 如果引用数据集包含 BLOB 数据类型，则运行包时该组件将失败。 可以使用 **“缓存连接管理器编辑器”** 修改列数据类型。 有关详细信息，请参阅 [Cache Connection Manager Editor]()。  
  
> [!NOTE]  
>  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
## <a name="configuration-of-the-cache-connection-manager"></a>缓存连接管理器的配置  
 可以用下列方式配置缓存连接管理器：  
  
-   指示是否使用缓存文件。  
  
     如果将缓存连接管理器配置为使用缓存文件，则连接管理器将执行下列操作之一：  
  
    -   若将“缓存转换”转换配置为将数据从数据流中的某个数据源写入缓存连接管理器，则将数据保存到该文件。  
  
    -   从缓存文件读取数据。  
  
     有关详细信息，请参阅 [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md)。  
  
-   更改缓存中存储的列的元数据。  
  
-   使用表达式在运行时更新缓存文件名，以设置 ConnectionString 属性。 有关详细信息，请参阅 [在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)。  
  
 可以通过 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 设计器或以编程方式来设置属性。  
  
 有关如何以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和[以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)。  
  
## <a name="cache-connection-manager-editor"></a>缓存连接管理器编辑器
  缓存连接管理器从缓存转换或缓存文件 (.caw) 中读取引用数据集，并且可以将数据保存到缓存文件中。 这些数据始终存储在内存中。  
  
> [!NOTE]  
>  缓存连接管理器不支持二进制大型对象 (BLOB) 数据类型 DT_TEXT、DT_NTEXT 和 DT_IMAGE。 如果引用数据集包含 BLOB 数据类型，则运行包时该组件将失败。 可以使用 **“缓存连接管理器编辑器”** 修改列数据类型。  
  
 查找转换在引用数据集上执行查找。  
  
 “缓存连接管理器编辑器”对话框包含以下选项卡：  
  
###  <a name="general-tab"></a><a name="generaltab"></a> “常规”选项卡  
 “缓存连接管理器编辑器”对话框的“常规”选项卡用于指示是从文件读取缓存还是将缓存保存到文件。  
  
#### <a name="options"></a>选项  
 **连接管理器名称**  
 为工作流中的缓存连接提供唯一的名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
 **说明**  
 描述此连接。 最好根据连接的用途对其进行说明，以使包的说明一目了然，且更便于维护。  
  
 **使用文件缓存**  
 指示是否使用缓存文件。  
  
> [!NOTE]  
>  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
 如果将缓存连接管理器配置为使用缓存文件，则连接管理器将执行下列操作之一：  
  
-   若将“缓存转换”转换配置为将数据从数据流中的某个数据源写入缓存连接管理器，则将数据保存到该文件。 有关详细信息，请参阅 [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md)。  
  
-   从缓存文件读取数据。  
  
 **文件名**  
 键入缓存文件的路径和文件名。  
  
 **“浏览”**  
 定位缓存文件。  
  
 **刷新元数据**  
 删除缓存连接管理器中的列元数据，然后用所选缓存文件中的列元数据重新填充缓存连接管理器。  
  
###  <a name="columns-tab"></a><a name="columnstab"></a> “列”选项卡  
 **“缓存连接管理器编辑器”** 对话框的 **“列”** 选项卡用于配置缓存中各列的属性。  
  
#### <a name="options"></a>选项  
 **列**  
 指定列名。  
  
 **索引位置**  
 通过指定各列的索引位置指定哪些列是索引列。 索引是一列或多个列的集合。  
  
 对于非索引列，索引位置是 0。  
  
 对于索引列，索引位置是连续的正数。 此数字指示查找转换将引用数据集中的行与输入数据源中的行进行比较的顺序。 具有最多唯一值的列应当具有最低的索引位置。  
  
> [!NOTE]  
>  当将查找转换配置为使用缓存连接管理器时，则仅引用数据集中的索引列能够映射到输入列。 此外，还必须对所有索引列进行映射。  
  
 **类型**  
 指定列的数据类型。  
  
 **长度**  
 指定列数据类型。 如果适用于该数据类型，则可更新 **Length**。  
  
 **精度**  
 指定特定列数据类型的精度。 精度指数字的位数。 如果适用于该数据类型，则可更新 **Precision**。  
  
 **缩放**  
 指定特定列数据类型的小数位数。 小数位数指小数点后的数字位数。 如果适用于该数据类型，则可更新 **Scale**。  
  
 **代码页**  
 指定列类型的代码页。 如果适用于该数据类型，则可更新 **Code Page**。  
  
## <a name="related-tasks"></a>Related Tasks  
 [在完全缓存模式下使用缓存连接管理器实现查找转换](lookup-transformation-full-cache-mode-cache-connection-manager.md)  
  
