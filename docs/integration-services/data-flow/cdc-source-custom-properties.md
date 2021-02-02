---
description: CDC 源自定义属性
title: CDC 源自定义属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 744e9357-94a9-4202-abe8-1d3d202697e9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: afc10a2cfaa58565ac1ea45521f2eb0370ee48c8
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049236"
---
# <a name="cdc-source-custom-properties"></a>CDC 源自定义属性

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  下表介绍 CDC 源的自定义属性。 所有属性均可读/写。  
  
|属性名称|数据类型|说明|  
|-------------------|---------------|-----------------|  
|连接|ADO.Net 连接|用于对更改表进行访问的与 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] CDC 数据库的 ADO.NET 连接。|  
|StateVariable|String|用于维护当前 CDC 运行的 CDC 状态的 SSIS 字符串包变量。|  
|CdcProcessingMode|Integer（枚举）|此模式确定处理方式。 可能值为 **“全部”** 、 **“全部且具有旧值”** 、 **“净值”** 、 **“具有更新掩码的净值”** 和 **“净值且具有合并”** 。<br /><br /> 使用“全部”启动的模式将返回所有更改，使用“净值”启动的模式将仅返回净更改。<br /><br /> 没有主键的表只能取“全部”值。<br /><br /> “具有更新掩码的净值”添加命名模式为 __$\<column-name>\_Changed 的布尔值列（指示当前更改行中已更改的列） 。<br /><br /> 有关此属性的值的其他信息，请参阅 [CDC 源编辑器（“连接管理器”页）](./cdc-source.md)。|  
|CaptureInstance|String|具有要读取的 CDC 表的 CDC 捕获实例的名称。 一个捕获源表可具有一个或两个捕获实例，以便通过架构更改处理表定义的无缝转换。 如果为要捕获的源表定义了一个捕获实例，则选择要在此处使用的捕获实例。 表 [schema].[table] 的默认捕获实例名称为 \<schema>_\<table>，但使用的实际捕获实例名称可能会不同。 读取的实际表是 CDC 表 cdc .\<capture-instance>_CT。|  
|ReprocessingIndicator|Boolean|一个值，指定是否要添加 **__$reprocessing** 列。 通过此特殊的输出列，SSIS 开发人员可以在处理初始处理范围时以不同方式处理一致性错误。<br /><br /> 如果为 **true**，则添加  **__$reprocessing** 列。<br /><br /> 在 CDC 处理范围与初始处理范围（与初始加载期间相对应的 LSN 的范围）重叠时，或者在之前的运行存在错误后重新处理某一 CDC 处理范围时，该列的值为 **true** 。 通过此指示器列，SSIS 开发人员可以在重新处理更改时以不同方式处理错误（例如，可忽略删除不存在的行和插入在重复键上失败之类的操作）。<br /><br /> 默认值是 **false** 秒。|  
|CommandTimeout|Integer|该值指定在与 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 数据库通信时要使用的超时值（秒）。 在来自数据库的响应时间非常慢并且默认值（30 秒）不足够时使用该值。|  
  
 有关 CDC 源的详细信息，请参阅 [CDC Source](../../integration-services/data-flow/cdc-source.md)。  
  
