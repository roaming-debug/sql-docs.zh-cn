---
title: 升级到启用了配置管理的大数据群集
titleSuffix: SQL Server big data clusters
description: 升级到启用了配置管理的大数据群集
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0a9f6addcf73e0c50d67f75f19fedd51b2f3dbc9
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344010"
---
# <a name="upgrading-to-a-configuration-management-enabled-cluster-cu8-or-lower-to-cu9"></a>升级到启用了配置管理的群集（从 CU8 或更低版本升级到 CU9+）

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
从 CU9 开始，大数据群集将包含配置管理功能，管理员可使用该功能在部署后更改或调整大数据群集的各个部分，更深入地了解其 BDC 中运行的配置。 在 CU9 之前，通常只有在部署时才可修改大数据群集配置，变通方法是使用自定义 `mssql-custom.conf` 文件配置某些 SQL 配置。 现在已经解决了这个问题，可通过配置管理功能配置这些设置。

## <a name="migrating-sql-configurations-in-mssql-customconf-to-the-configuration-management-system"></a>将 mssql-custom.conf 中的 SQL 配置迁移到配置管理系统
如果已为 SQL Server 主实例创建了一个自定义 `mssql-custom.conf`，请按照下面的一次性说明，通过配置系统而不是文件来管理设置。 如果未按照这些步骤操作，则配置管理功能将不会管理这些 SQL 配置，`mssql-custom.conf` 设置将替代通过配置管理功能对这些设置所做的任何更改。

步骤：
1. 将大数据群集升级到 CU9
> [!NOTE]
> 通过 `mssql-custom.conf` 定义的设置不会被更改或删除。 只是配置框架不会反映和管理它们。

2. 使用新的配置功能设置并应用以前在 `mssql-custom.conf` 中定义的任何设置。 请参阅 [SQL Server大数据群集后期部署配置概述](configure-bdc-postdeployment.md)，获取更改设置的分步指南。 请参阅 [SQL Server 大数据群集配置属性](reference-config-bdc-overview.md)，获取各个范围的可用设置的完整列表。 请注意，某些设置（如 customerFeedback）的范围可能已发生更改，但仍可用。
3. 在每个主 Pod 中，将 `mssql-server` 容器的 `mssql-custom.conf` 文件重命名为 `deprecated-mssql-custom.conf`。 如果只有一个主 Pod，则重命名为 `master-0`。 如需降级或回退到未启用配置的群集（CU8 或更低版本），则可以重用此文件以应用这些自定义 SQL 配置。

## <a name="downgrading-from-a-configuration-management-enabled-cluster-to-a-non-configuration-management-enabled-cluster-cu9-to-cu8-or-lower"></a>从启用了配置管理的群集降级为未启用配置管理的群集（从 CU9+ 降至 CU8 或更低版本）
从启用了配置管理的群集 (CU9+) 降级为未启用配置管理的群集（CU8 或更低版本）后，用户将无法调整大数据群集后期部署。 还需要使用可选的 `mssql-custom.conf` 文件来设置 SQL 配置。 如果已在升级到 CU9+ 时将文件重命名为 `deprecated-mssql-custom.conf`，请将其重命名为 `mssql-custom.conf`。 如果删除了该文件或之前未创建该文件，且现在需要定义这些特殊的 SQL 配置，请按照此处的说明进行创建：[SQL Server 主实例配置属性 - CU9 之前的版本](reference-config-master-instance.md)。 通过配置管理体验定义和更改的所有设置都将还原为以前的配置或系统默认值。 

群集降级后，设置将还原为默认值或部署 bdc.json 中指定的值。 降级后，无需执行其他步骤。