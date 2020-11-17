---
title: 配置群集
titleSuffix: Configure a SQL Server big data cluster
description: 有关如何配置 SQL Server 大数据群集的概述
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 08/04/2020
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b75f6946af9a13d6a8202c627d4c24a2225a51c1
ms.sourcegitcommit: 4545b502e3cae7136411fd9a7c15450315665f38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94549981"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>配置 SQL Server 大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

部署时，可以在 SQL Server 2019 大数据群集中配置 SQL Server 主实例、Apache Spark 和 Apache Hadoop 的属性。

部署后，大数据群集不支持修改配置属性。

## <a name="configuration-scopes"></a>配置范围
大数据群集配置具有两个范围级别：`service` 和 `resource`。 这些设置的层次结构也遵循此顺序，即从最高到最低。 BDC 组件将使用在最低范围定义的设置的值。 如果未在给定范围定义设置，则它将继承其更高的父范围中的值。

例如，你可能需要定义 Spark 驱动程序将在存储池和 Sparkhead `resources` 中使用的默认内核数。 可通过两种方式实现此目的： 
- 在 `Spark` 服务范围指定默认内核值  
- 在 `storage-0` 和 `sparkhead` 资源范围指定默认内核值

在第一种方案中，Spark 服务的所有较低范围的资源（存储池和 Sparkhead）都将从 Spark 服务默认值继承默认内核数。

在第二种方案中，每个资源将使用在其各自范围中定义的值。

如果在服务和资源范围同时配置了默认内核数，则资源范围的值将替代服务范围值，因为这是用户针对给定设置配置的最低范围。

有关配置的具体信息，请参阅相应文章：

如何： 
- [配置 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的主实例](configure-sql-server-master-instance.md)
- [在大数据群集中配置 Apache Spark 和 Apache Hadoop](configure-spark-hdfs.md)

参考： 
- [SQL Server 主实例配置属性](reference-config-master-instance.md)
- [Apache Spark 和 Apache Hadoop (HDFS) 配置属性](reference-config-spark-hadoop.md)
