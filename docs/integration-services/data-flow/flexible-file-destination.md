---
description: 灵活的文件目标
title: 灵活的文件目标 | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpextfiledest.f1
- sql14.dts.designer.afpextfiledest.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d331004e82b155a1c101a31253a82e029ec4a31b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091642"
---
# <a name="flexible-file-destination"></a>灵活的文件目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

通过“灵活的文件目标”组件，SSIS 包可将数据写入到各种受支持的存储服务中。

当前支持的存储服务为

- [Azure Blob 存储](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)
   
将“灵活的文件目标”拖放到数据流设计器中，然后双击它来打开编辑器。
  
“灵活的文件目标”是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组成部分。  

“灵活的文件目标编辑器”上提供了以下属性。

- **文件连接管理器类型：** 指定源连接管理器类型。 然后，从指定的类型中选择一个现成的或新建一个。
- **文件夹路径：** 指定目标文件夹路径。
- **文件名：** 指定目标文件名称。
- **文件格式：** 指定目标文件格式。 支持格式为 Text、Avro、ORC 和 Parquet。 ORC/Parquet 需要 Java。 请参阅[此处](../../integration-services/azure-feature-pack-for-integration-services-ssis.md#dependency-on-java)了解详细信息。
- **列分隔符字符：** 指定要用作列分隔符的字符（不支持多字符分隔符）。
- **作为列名称的第一行：** 指定是否将列名称写入第一行。
- **压缩文件：** 指定是否压缩文件。
- **压缩类型：** 指定要使用的压缩格式。 支持格式为 GZIP、DEFLATE 和 BZIP2。
- **压缩级别：** 指定要使用的压缩级别。

“高级编辑器”上提供了以下属性。

- **rowDelimiter：** 用于分隔文件中的行的字符。 只能使用一个字符。 默认值为 \r\n。
- **escapeChar：** 用于转义输入文件内容中的列分隔符的特殊字符。 不能同时指定表的 escapeChar 和 quoteChar。 只能使用一个字符。 没有默认值。
- **quoteChar：** 将字符串值用引号括起来的字符。 引号字符内的列和行分隔符将被视为字符串值的一部分。 此属性适用于输入和输出数据集。 不能同时指定表的 escapeChar 和 quoteChar。 只能使用一个字符。 没有默认值。
- **nullValue：** 用于表示 null 值的一个或多个字符。 默认值为 \N。
- **encodingName：** 指定编码名称。 请参阅 [Encoding.EncodingName](/dotnet/api/system.text.encoding) 属性。
- **skipLineCount：** 指示从输入文件读取数据时要跳过的非空行数。 如果同时指定了 skipLineCount 和 firstRowAsHeader，则先跳过行，然后从输入文件读取标头信息。
- **treatEmptyAsNull：** 指定是否在从输入文件读取数据时将 null 或空字符串视为 null 值。 默认值为 True。

指定连接信息后，切换到“列”  页，将源列映射到 SSIS 数据流的目标列。

**有关服务主体权限配置的说明**

要使“测试连接”起作用（Blob 存储或 Data Lake Storage Gen2），应向服务主体分配至少存储帐户的“存储 Blob 数据读取器”角色   。
可通过 [RBAC](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal) 实现。

对于 Blob 存储，通过分配至少“存储 Blob 数据读参与者”角色来授予写入权限。

对于 Data Lake Storage Gen2，权限由 RBAC 和 [ACL](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer) 共同决定。
请注意，ACL 使用用于注册应用的服务主体对象 ID (OID) 进行配置，如[此处](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal)所述。
这与用于 RBAC 配置的应用程序（客户端）ID 有所不同。
通过内置角色或自定义角色向安全主体授予 RBAC 数据权限时，将首先根据请求的授权来评估这些权限。
如果请求的操作已获得安全主体的 RBAC 分配的授权，则授权会立即得到解决，且不会执行任何其他 ACL 检查。
或者，如果安全主体没有 RBAC 分配，或请求的操作与分配的权限不匹配，则会执行 ACL 检查来确定是否已授权安全主体执行请求的操作。
对于写入权限，请从接收器文件系统开始授予至少“执行”权限，并授予接收器文件夹的“写入”权限   。
或者，通过 RBAC 授予至少“存储 Blob 数据参与者”角色  。
有关详细信息，请参阅[此](/azure/storage/blobs/data-lake-storage-access-control)文章。