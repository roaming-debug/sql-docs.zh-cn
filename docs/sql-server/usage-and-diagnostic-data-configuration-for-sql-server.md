---
title: 配置 SQL Server 使用情况和诊断数据收集 (CEIP) | Microsoft Docs
description: 了解 SQL Server 从用户处收集的用于改善产品的信息。 了解如何配置 SQL Server 以使其不发送此信息。
author: MikeRayMSFT
ms.author: mikeray
ms.date: 03/27/2019
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: configuration
ms.openlocfilehash: 0fc8437d6ffb3382dbcc53edc816b00ec22b7a07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353577"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-ceip"></a>配置 SQL Server 使用情况和诊断数据收集 (CEIP)

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

## <a name="summary"></a>总结

默认情况下，Microsoft SQL Server 收集有关其客户如何使用应用程序的信息。 具体来说，SQL Server 收集有关安装体验、使用情况和性能的信息。 此信息有助于 Microsoft 改进产品以更好地满足客户需求。 例如，Microsoft 收集有关客户遇到的错误代码类型信息，这样我们就可以修复相关 bug，改进关于如何使用 SQL Server 的文档，并确定是否应将功能添加到产品中以更好地为客户服务。

具体而言，Microsoft 不会通过这种机制发送以下任何类型的信息：
- 用户表中的任何值
- 任何登录凭据或其他身份验证信息
- 个人身份信息 (PII)

以下示例场景包括有助于改进产品的功能使用情况信息。

SQL Server 2017 支持列存储索引，以实现快速分析方案。 列存储索引将新插入数据的传统“B 树”索引结构与面向列的特殊压缩结构相结合，以压缩数据并加快查询执行速度。 该产品使用探索性方法在后台将数据从 B 树结构迁移到压缩结构，可加快后续获取查询结果的过程。

如果后台操作未与插入数据的速度保持一致，则查询性能可能比预期要慢。 为了改进产品，Microsoft 收集有关 SQL Server 与自动数据压缩过程同步情况的信息。 产品团队使用此信息来微调执行压缩的代码的频率和并行结构。 此查询偶尔运行以收集此信息，以便我们 (Microsoft) 可以评估数据移动速度。 这有助于我们优化产品探索方法。  

```sql
SELECT object_id, type_desc, data_space_id, db_id() AS database_id FROM sys.indexes WITH(nolock) WHERE type = 5 or type = 6 
```

```sql
SELECT cntr_value as merge_policy_evaluation
FROM sys.dm_os_performance_counters WITH(nolock)
WHERE object_name LIKE '%columnstore%' 
AND counter_name ='Total Merge Policy Evaluations' 
AND instance_name = '_Total'
```

请注意，此过程专注于为客户提供价值的必要机制。 产品团队不会查看索引中的数据，也不会将该数据发送给 Microsoft。 SQL Server 2017 始终收集和发送从安装过程开始的安装体验相关信息，方便我们快速查找并修复客户遇到的任何安装问题。 通过以下机制可将 SQL Server 2017 配置为不（基于每个服务器实例）向 Microsoft 发送信息：
- 通过使用错误和使用情况报告应用程序
- 通过在服务器上设置注册表子项

对于 Linux 上的 SQL Server，请参阅 [Linux 上的 SQL Server 客户反馈](../linux/usage-and-diagnostic-data-configuration-for-sql-server-linux.md)

> [!NOTE]
> 只能在付费版本的 SQL Server 中禁用向 Microsoft 发送信息的功能。

## <a name="remarks"></a>备注
 - 不支持删除或禁用 SQL CEIP 服务。 
 - 不支持从群集组中删除 SQL CEIP 资源。 

若要选择退出数据收集，请参阅[启用或禁用本地审核](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off)

## <a name="error-and-usage-reporting-application"></a>错误和使用情况报告应用程序 

设置完成后，SQL Server 组件和实例的使用情况和诊断数据收集设置可以通过“错误和使用情况报告”应用程序进行更改。 可将此应用程序作为 SQL Server 安装的一部分。 借助此工具，每个 SQL Server 实例都可以配置自己的使用情况报告设置。

> [!NOTE]
> 错误和使用情况报告应用程序在 SQL Server 的“配置工具”下列出。 使用此工具，可以 SQL Server 2017 中的相同方式，管理错误报告以及使用情况和诊断数据收集的偏好设置。 错误报告独立于使用情况和诊断数据收集，因此可以与使用情况和诊断数据收集分开启用或禁用。 错误报告收集发送到 Microsoft 的故障转储，其中可能包含[隐私声明](./sql-server-privacy.md)中所述的敏感信息。

要启动 SQL Server 错误和使用情况报告，请单击或点击“启动”，然后在搜索框中搜索“错误”。 将显示 SQL Server 错误和使用情况报告项。 启动此工具后，可以管理为相应计算机上安装的实例和组件收集的使用情况和诊断数据以及严重错误。

对于付费版本，选中“使用情况报告”复选框可以管理向 Microsoft 发送使用情况和诊断数据。

对于付费或免费版本，请使用“错误报告”复选框来管理向 Microsoft 发送有关严重错误和故障转储反馈的功能。

## <a name="set-registry-subkeys-on-the-server"></a>在服务器上设置注册表子项

企业客户可以配置组策略设置来选择启用或禁用使用情况和诊断数据收集。 这可通过配置基于注册表的策略完成。 相关注册表子项和设置如下所示：

- 对于 SQL Server 实例功能：
    
    子项 = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{InstanceID}\CPE
    
    注册表项名称 = CustomerFeedback
    
    注册表项类型 DWORD：0 表示选择退出；1 表示选择加入
    
    {InstanceID} 是指实例类型和实例，如以下示例所示：

    - MSSQL14.CANBERRA 是指 SQL Server 2017 数据库引擎，“CANBERRA”指实例名称
    - MSAS14.CANBERRA 是指 SQL Server 2017 Analysis Services，“CANBERRA”指实例名称
    - MSRS14.CANBERRA 是指 SQL Server 2017 Reporting Services，“CANBERRA”指实例名称

- 对于所有共享功能：
    
    子项 = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Major Version}
    
    注册表项名称 = CustomerFeedback
    
    注册表项类型 DWORD：0 表示选择退出；1 表示选择加入

> [!NOTE]
> {Major Version} 是指 SQL Server 的版本，例如，140 表示 SQL Server 2017

- 对于 SQL Server Management Studio 17 和 SQL Server Management Studio 18，请参阅 [SQL Server Management Studio 中的用户协助](../ssms/sql-server-management-studio-telemetry-ssms.md)

## <a name="set-registry-subkeys-for-crash-dump-collection"></a>设置故障转储收集的注册表子项

与 SQL Server 早期版本的行为类似，SQL Server 2017 Enterprise 客户可以在服务器上配置组策略设置，以选择加入或退出故障转储收集。 这可通过配置基于注册表的策略完成。 相关注册表子项和设置如下所示： 

- 对于 SQL Server 实例功能：

    子项 = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{InstanceID}\CPE

    注册表项名称 = EnableErrorReporting

    注册表项类型 DWORD：0 表示选择退出；1 表示选择加入
 
    {InstanceID} 是指实例类型和实例，如以下示例所示： 

    - MSSQL14.CANBERRA 是指 SQL Server 2017 数据库引擎，“CANBERRA”指实例名称
    - MSAS14.CANBERRA 是指 SQL Server 2017 Analysis Services，“CANBERRA”指实例名称
    - MSRS14.CANBERRA 是指 SQL Server 2017 Reporting Services，“CANBERRA”指实例名称
 

- 对于所有共享功能：
    
    子项 = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Major Version}

    注册表项名称 = EnableErrorReporting

    注册表项类型 DWORD：0 表示选择退出；1 表示选择加入

> [!NOTE]
> {Major Version} 是指 SQL Server 的版本。 例如，“140”是指 SQL Server 2017。

这些注册表子项上基于注册表的组策略适用于 SQL Server 2017 故障转储收集。 

## <a name="crash-dump-collection-for-ssms"></a>SSMS 的故障转储收集
SSMS 不会收集其自身的故障转储。 与 SSMS 相关的任何故障转储都作为 Windows 错误报告的一部分进行收集。

启用或禁用此功能的过程取决于操作系统版本。 要启用或禁用该功能，请按照 Windows 版本相应文章中的步骤操作。
 
- Windows Server 2016 和 Windows 10

    [在组织中配置 Windows 诊断数据](/windows/privacy/configure-windows-diagnostic-data-in-your-organization)
- Windows Server 2008 R2 和 Windows 7

    [WER 设置](/windows/desktop/wer/wer-settings)
 
## <a name="feedback-for-analysis-services"></a>对于 Analysis Services 的反馈

在安装过程中，SQL Server 2016 Analysis Services 向 Analysis Services 实例添加一个特殊帐户。 此帐户是 Analysis Services 服务器管理员角色的成员。 该帐户用于收集来自 Analysis Services 实例的反馈信息。  

可以将服务配置为，不发送使用情况和诊断数据，如“在服务器上设置注册表子项”部分所述。 但是，执行此操作不会删除服务帐户。 
 
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]