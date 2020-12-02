---
description: 缓存转换
title: 缓存转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.cachetrans.f1
- sql13.dts.designer.cachetranscon.f1
- sql13.dts.designer.cachetransmap.f1
helpviewer_keywords:
- Cache transform
ms.assetid: a5683fc8-9c32-4634-819e-e9815627e4f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21f4d8059ee7e0e2f9d466289f4b40f9f51ff950
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123438"
---
# <a name="cache-transform"></a>缓存转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  “缓存转换”转换通过将所连接数据源中的数据写入流入缓存连接管理器的数据流，可以为查找转换生成一个引用数据集。 查找转换通过将所连接数据源输入列中的数据与引用数据库中的列进行联接来执行查找。  
  
 若想将查找转换配置为在完全缓存模式中运行，可使用缓存连接管理器。 在此模式下，在查找转换运行前，引用数据集会加载到缓存中。  
  
 有关如何使用缓存连接管理器和“缓存转换”转换配置完全缓存模式下的查找转换的说明，请参阅 [在完全缓存模式下使用缓存连接管理器实现查找转换](../../connection-manager/lookup-transformation-full-cache-mode-cache-connection-manager.md)。  
  
 有关缓存引用数据集的详细信息，请参阅 [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md)。  
  
> [!NOTE]  
>  “缓存转换”仅向缓存连接管理器写入唯一行。  
  
 在单个包中，只有一个缓存转换可以将数据写入同一缓存连接管理器。 如果某个包中包含多个缓存转换，则该包在运行时所调用的第一个缓存转换会将数据写入连接管理器。 后续缓存转换将无法执行写入操作。  
  
 有关详细信息，请参阅[缓存连接管理器](../../connection-manager/cache-connection-manager.md)。  
  
## <a name="configuration-of-the-cache-transform"></a>缓存转换的配置  
 可以将缓存连接管理器配置为向缓存文件 (.caw) 中保存数据。  
  
 可以采用下列方法配置缓存转换：  
  
-   指定缓存连接管理器。  
  
-   将缓存转换中的输入列映射到缓存连接管理器中的目标列。  
  
    > [!NOTE]  
    >  每个输入列都必须映射到目标列，而且二者的列数据类型必须匹配。 否则， [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 设计器将显示一则错误消息。  
  
     可以使用 **“缓存连接管理器编辑器”** 修改列数据类型、名称以及其他列属性。  
  
 可以通过 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 设计器来设置属性。 有关可在 **“高级编辑器”** 对话框中设置的属性的详细信息，请参阅 [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="cache-transformation-editor-connection-manager-page"></a>缓存转换编辑器（“连接管理器”页）
  可以使用 **“缓存转换编辑器”** 对话框的 **“连接管理器”** 页，选择现有缓存连接管理器或创建新的缓存连接管理器。  
  
 若要了解有关缓存连接管理器的详细信息，请参阅 [Cache Connection Manager](../../connection-manager/cache-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **“完全缓存”**  
 使用列表选择现有的缓存连接管理器，或使用“新建”按钮创建新的连接。  
  
 **新建**  
 使用“缓存连接管理器编辑器”对话框创建新的连接。  
  
 **编辑**  
 修改现有连接  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [数据流](../../../integration-services/data-flow/data-flow.md)  
  
