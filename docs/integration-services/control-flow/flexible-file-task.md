---
description: 灵活的文件任务
title: 灵活的文件任务 | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPEXTFILETASK.F1
- SQL14.DTS.DESIGNER.AFPEXTFILETASK.F1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 89cb7e2de8cc4fa78afa93ca35f4eb36dab7136a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345721"
---
# <a name="flexible-file-task"></a>灵活的文件任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

通过灵活的文件任务，用户可在各种支持的存储服务上执行文件操作。
当前支持的存储服务为

- 本地文件系统
- [Azure Blob 存储](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)

灵活的文件任务是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组成部分。

要将灵活的文件任务添加到包，请将其从 SSIS 工具箱拖到设计器画布。 然后，双击该任务，或右键单击任务并选择“编辑”，以打开“灵活的文件任务编辑器”对话框   。

“操作”属性指定要执行的文件操作  。
当前支持的操作包括：
- **复制** 操作
- **删除** 操作

以下属性可用于“复制”操作  。

- **SourceConnectionType：** 指定源连接管理器类型。
- **SourceConnection：** 指定源连接管理器。
- **SourceFolderPath：** 指定源文件夹路径。
- **SourceFileName：** 指定源文件名称。 如果留空，则将复制源文件夹。 源文件名称中允许使用以下通配符：`*`（匹配零个或多个字符）、`?`（匹配零个或单个字符）和 `^`（转义字符）。
- **SearchRecursively：** 指定是否以递归方式复制子文件夹。
- **DestinationConnectionType：** 指定目标连接管理器类型。
- **DestinationConnection：** 指定目标连接管理器。
- **DestinationFolderPath：** 指定目标文件夹路径。
- **DestinationFileName：** 指定目标文件名称。 如果留空，则将使用源文件名称。

以下属性可用于“删除”操作  。
- **ConnectionType：** 指定连接管理器类型。
- **Connection：** 指定连接管理器。
- **FolderPath：** 指定文件夹路径。
- **FileName：** 指定文件名。 如果留空，则将删除文件夹。 对于 Azure Blob 存储，不支持删除文件夹。 文件名称中允许使用以下通配符：`*`（匹配零个或多个字符）、`?`（匹配零个或单个字符）和 `^`（转义字符）。
- **DeleteRecursively：** 指定是否递归删除文件。

***有关服务主体权限配置的说明***

要使“测试连接”起作用（Blob 存储或 Data Lake Storage Gen2），应向服务主体分配至少存储帐户的“存储 Blob 数据读取器”角色   。
可通过 [RBAC](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal) 实现。

对于 Blob 存储，通过分别分配至少“存储 Blob 数据读取器”和“存储 Blob 数据参与者”角色来授予读取和写入权限   。

对于 Data Lake Storage Gen2，权限由 RBAC 和 [ACL](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer) 共同决定。
请注意，ACL 使用用于注册应用的服务主体对象 ID (OID) 进行配置，如[此处](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal)所述。
这与用于 RBAC 配置的应用程序（客户端）ID 有所不同。
通过内置角色或自定义角色向安全主体授予 RBAC 数据权限时，将首先根据请求的授权来评估这些权限。
如果请求的操作已获得安全主体的 RBAC 分配的授权，则授权会立即得到解决，且不会执行任何其他 ACL 检查。
或者，如果安全主体没有 RBAC 分配，或请求的操作与分配的权限不匹配，则会执行 ACL 检查来确定是否已授权安全主体执行请求的操作。

- 对于读取权限，请从源文件系统开始授予至少“执行”权限，并授予要复制的文件的“读取”权限   。 或者，通过 RBAC 授予至少“存储 Blob 数据读取器”角色  。
- 对于写入权限，请从接收器文件系统开始授予至少“执行”权限，并授予接收器文件夹的“写入”权限   。 或者，通过 RBAC 授予至少“存储 Blob 数据参与者”角色  。

有关详细信息，请参阅[此](/azure/storage/blobs/data-lake-storage-access-control)文章。