---
description: OData 源
title: OData 源 | Microsoft Docs
ms.date: 09/17/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.DTS.DESIGNER.ODATASOURCE.F1
- sql13.dts.designer.odatasource.connection.f1
- sql13.dts.designer.odatasource.columns.f1
- sql13.dts.designer.odatasource.erroroutput.f1
ms.assetid: cc9003c9-638e-432b-867e-e949d50cec90
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8f872916b7b93a1aab3447bad6579dd672c915e1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194788"
---
# <a name="odata-source"></a>OData 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


在 SSIS 包中使用 OData 源组件可以使用开放数据协议 (OData) 服务中的数据。

## <a name="supported-protocols-and-data-formats"></a>支持的协议和数据格式

该组件支持 OData v3 和 v4 协议。  
  
-   对于 OData V3 协议，该组件支持 ATOM 和 JSON 数据格式。  
  
-   对于 OData V4 协议，该组件支持 JSON 数据格式。  

## <a name="supported-data-sources"></a>支持的数据源

OData 源包括对以下数据源的支持：
-   Microsoft Dynamics AX Online 和 Microsoft Dynamics CRM Online
-   SharePoint 列表。 要查看 SharePoint 服务器上的所有列表，请使用以下 URL：`https://<server>/_vti_bin/ListData.svc`。 有关 SharePoint URL 约定的详细信息，请参阅 [SharePoint Foundation REST 接口](/previous-versions/office/developer/sharepoint-2010/ff521587(v=office.14))。

## <a name="supported-data-types"></a>支持的数据类型

OData 源支持以下简单的数据类型：int、byte[]、bool、byte、DateTime、DateTimeOffset、decimal、double、Guid、Int16、Int32、Int64、sbyte、float、string 和 TimeSpan。

若要发现数据源中列的数据类型，请查看 `https://<OData feed endpoint>/$metadata` 页。

对于十进制数据类型，精度和小数位数由源元数据决定。 如果源元数据未指定“精度”和“小数位数”属性，则数据可能会被截断。

> [!IMPORTANT]
> OData 源组件不支持 SharePoint 列表中的复杂类型，例如多选项。

> [!Note]
> 如果源仅允许 TLS 1.2 连接，则需要通过注册表设置在计算机上强制执行 TLS 1.2。 在提升的命令提示符下运行以下命令：
>
> reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SchUseStrongCrypto /t REG_DWORD /d 1 /reg:64
>
> reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SchUseStrongCrypto /t REG_DWORD /d 1 /reg:32

## <a name="odata-format-and-performance"></a>OData 格式和性能
 大多数 OData 服务都可以多种格式返回结果。 可以使用 `$format` 查询选项指定结果集的格式。 JSON 和 JSON 轻型这类格式比 ATOM 或 XML 更高效，并且在传输大量数据时的性能更佳。 下表提供来自示例测试的结果。 可以看到，从 ATOM 切换至 JSON 后，性能提高 30-53%，从 ATOM 切换至新的 JSON 轻型格式（WCF Data Services 5.1 中提供）后，性能提高 67%。  
  
|“行”|ATOM|JSON|JSON（轻型）|  
|-|-|-|-|  
|10000|113 秒|74 秒|68 秒|  
|1000000|1110 秒|853 秒|665 秒|  
  
## <a name="related-topics-in-this-section"></a>本节中的相关主题  
  
-   [教程：使用 OData 源](../../integration-services/data-flow/tutorial-using-the-odata-source.md)  
  
-   [在运行时修改 OData 源查询](../../integration-services/data-flow/modify-odata-source-query-at-runtime.md)  
  
-   [OData 源属性](../../integration-services/data-flow/odata-source-properties.md)  
  
## <a name="odata-source-editor-connection-page"></a>OData 源编辑器（“连接”页）
  可以使用 **“OData 源编辑器”** 对话框的 **“连接”** 页，为 OData 源选择 OData 连接管理器。 通过此页还可以指定集合或资源路径和任何查询选项以指示需要从 OData 源检索的数据。 
  
### <a name="static-options"></a>静态选项  
 **OData 连接管理器**  
 从列表中选择一个现有连接管理器，或通过单击“新建”  创建一个新连接。  
  
 选择或创建连接管理器以后，对话框会显示连接管理器所使用的 OData 协议版本。  
  
 **新建**  
 通过使用“OData 连接管理器编辑器”对话框创建新的连接管理器。  
  
 **使用集合或资源路径**  
 指定从源选择数据的方法。  
  
|选项|说明|  
|------------|-----------------|  
|集合|使用集合名称查询从 OData 源检索数据。|  
|资源路径|使用资源路径查询从 OData 源检索数据。|  
  
 **查询选项**  
 为查询指定选项。 例如： `$top=5` 
  
 **馈送 url**  
 基于在此对话框中选择的选项显示只读馈送 URL。  
  
 **预览**  
 通过使用“预览”对话框预览结果。 **“预览”** 最多可以显示 20 行。  
  
### <a name="dynamic-options"></a>动态选项  
  
#### <a name="use-collection-or-resource-path--collection"></a>使用集合或资源路径 = 集合  
 **集合**  
 从下拉列表中选择集合。  
  
#### <a name="use-collection-or-resource-path--resource-path"></a>使用集合或资源路径 = 资源路径  
 **Resource path**  
 键入资源路径。 例如：Employees  
  
## <a name="odata-source-editor-columns-page"></a>OData 源编辑器（“列”页）
  使用“OData 源编辑器”对话框的“列”页可选择要包含在输出中的外部（源）列并将它们映射到输出列。  
  
### <a name="options"></a>选项  
 **可用外部列**  
 查看数据源中可用源列的列表。 使用列表中的复选框可向页底部的表添加或删除列。 所选列会添加到输出中。  
  
 **“外部列”**  
 查看您选择包含在输出中的源列。  
  
 **输出列**  
 为每个输出列提供唯一的名称。 默认值为所选外部（源）列的名称；不过，您也可以任选一个唯一的描述性名称。  
  
## <a name="odata-source-editor-error-output-page"></a>OData 源编辑器（“错误输出”页）
  可以使用 **“OData 源编辑器”** 对话框的 **“错误输出”** 页选择错误处理选项以及设置错误输出列的属性。  
  
### <a name="options"></a>选项  
 **输入/输出**  
 查看数据源的名称。  
  
 **列**  
 查看在“OData 源编辑器”对话框中“Connection Manager”页上选择的外部（源）列。  
  
 **错误**  
 指定发生错误时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **相关主题：** [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **截断**  
 指定发生截断时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **说明**  
 查看对错误的说明。  
  
 **将此值设置到选定的单元格**  
 指定发生错误或截断时应对所有选定单元格执行的操作：忽略失败、重定向行或使组件失败。  
  
 **应用**  
 将错误处理选项应用到选定的单元格。  
  
## <a name="see-also"></a>另请参阅  
 [OData 连接管理器](../../integration-services/connection-manager/odata-connection-manager.md)  
  
