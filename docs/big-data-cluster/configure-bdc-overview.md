---
title: SQL Server 大数据群集配置概述
titleSuffix: SQL Server big data clusters
description: 大数据群集配置概述
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cda6e61e8f5f13f5fd414879f888c7ed72a1bd0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343979"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>配置 SQL Server 大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
配置管理使管理员能够确保其大数据群集始终能够满足其工作负载需求。 利用此功能，群集管理员可以在部署时或部署后更改或调整大数据群集的各个部分，更深入地了解其 BDC 中运行的配置。 

配置管理功能使管理员可以启用 SQL 代理，为其组织的 Spark 作业定义基线资源，甚至查看每个范围的可配置设置。 部署时，可通过部署 `bdc.json` 文件进行配置，部署后，可通过 azdata CLI 进行配置。

## <a name="configuration-scopes"></a>配置范围
大数据群集配置具有两个范围级别：`cluster`、`service` 和 `resource`。 这些设置的层次结构也遵循此顺序，即从最高到最低。 BDC 组件将使用在最低范围定义的设置的值。 如果未在给定范围定义设置，则它将继承其更高的父范围中的值。

例如，最好定义 Spark 驱动程序将在存储池和 `Sparkhead` 资源中使用的默认核心数。 若要定义默认的核心数，可以执行以下操作之一：

- 在 `Spark` 服务范围指定默认内核值

- 在 `storage-0` 和 `sparkhead` 资源范围指定默认内核值

在第一种方案中，Spark 服务的所有较低范围的资源（存储池和 `Sparkhead`）都将从 Spark 服务默认值继承默认核心数。

在第二种方案中，每个资源将使用在其各自范围中定义的值。

如果在服务和资源范围同时配置了默认内核数，则资源范围的值将替代服务范围值，因为这是用户针对给定设置配置的最低范围。

## <a name="next-steps"></a>后续步骤

有关配置的具体信息，请参阅相应文章：

- [自定义大数据群集部署](deployment-custom-configuration.md)
- [配置大数据群集后期部署](configure-bdc-postdeployment.md)
- [配置大数据群集 - CU8 版本及更早版本](configure-bdc-pre-configuration.md)

参考： 
- [SQL Server 大数据群集配置属性](reference-config-bdc-overview.md)
- [Apache Spark 和 Apache Hadoop (HDFS) 配置属性](reference-config-spark-hadoop.md)
- [SQL Server 主实例配置属性 - Pre CU9 版本](reference-config-master-instance.md)
- [azdata CLI](../azdata/reference/reference-azdata.md)