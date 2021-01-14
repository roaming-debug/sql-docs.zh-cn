---
title: 使用 Resource Governor 进行管理
description: 了解如何使用 Resource Governor 管理 SQL Server 机器学习服务中 Python 和 R 工作负载的 CPU、物理 IO 和内存资源分配。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3e71b33eb08fd386232e992e9b47da7b5057aa32
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170719"
---
# <a name="manage-python-and-r-workloads-with-resource-governor-in-sql-server-machine-learning-services"></a>在 SQL Server 机器学习服务中使用 Resource Governor 管理 Python 和 R 工作负载
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

了解如何使用 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) 管理 SQL Server 机器学习服务中 Python 和 R 工作负载的 CPU、物理 IO 和内存资源分配。

Python 和 R 中的机器学习算法需要大量计算。 根据工作负载优先级，你可能需要增加或减少可用于机器学习服务的资源。

有关详细常规信息，请参阅 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)。

> [!NOTE] 
> Resource Governor 是企业版功能。

## <a name="default-allocations"></a>默认分配

默认情况下，机器学习的外部脚本运行时限制为不超过总计算机内存的 20%。 这取决于系统，但一般情况下，你可能会发现，此限制不足以应对繁重的机器学习任务，例如定型模型或预测多行数据。 

## <a name="manage-resources-with-resource-governor"></a>使用 Resource Governor 管理资源
 
默认情况下，外部进程最多可使用本地服务器上总主机内存的 20%。 可借助使用了所有对外部进程可用的容量的 R 和 Python 进程，修改默认资源池以在服务器范围内进行更改。

另外，还可以使用关联工作负载组和分类器创建自定义外部资源池，以确定对源自特定程序、主机或所提供的其他条件的请求的资源分配。 外部资源池是 [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] 中引入的一种资源池，可帮助管理数据库引擎外部的 R 和 Python 进程。

1. [启用资源调控](../../relational-databases/resource-governor/enable-resource-governor.md)（默认情况下处于关闭状态）。

2. 运行 [CREATE EXTERNAL RESOURCE POOL](../../t-sql/statements/create-external-resource-pool-transact-sql.md) 创建和配置资源池，然后运行 [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md) 将其实现。

3. 创建用于具体分配的工作负荷组，例如在定型和评分之间。

4. 创建分类器以截获用于外部处理的调用。

5. 使用所创建的对象执行查询和过程。

有关演练，请参阅[为 SQL Server 机器学习服务创建资源池](create-external-resource-pool.md)，以了解分步说明。

有关术语和一般概念的简介，请参阅 [Resource Governor 资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)。

## <a name="processes-under-resource-governance"></a>资源调控下的进程
  
 可使用外部资源池来管理以下数据库引擎实例上可执行文件使用的资源  ：

+ Rterm.exe - 从 SQL Server 本地调用或当 SQL Server 作为远程计算上下文远程调用时
+ Python.exe - 从 SQL Server 本地调用或当 SQL Server 作为远程计算上下文远程调用时
+ BxlServer.exe 和附属进程
+ 启动板启动的附属进程，如 PythonLauncher.dll
  
> [!NOTE]
> 不支持使用 Resource Governor 直接管理启动板服务。 启动板是一项受信任的服务，它只能托管 Microsoft 提供的启动器。 受信任的启动器经显式配置，以避免消耗过多的资源。
  
## <a name="next-steps"></a>后续步骤

+ [为机器学习创建资源池](create-external-resource-pool.md)
+ [Resource Governor 资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)