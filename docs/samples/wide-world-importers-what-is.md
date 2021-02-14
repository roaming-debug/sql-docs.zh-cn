---
title: 宽世界导入程序-SQL 数据库示例 |Microsoft Docs
description: 了解 WideWorldImporters 示例数据库如何支持虚构 WideWorldImporters 公司的工作流。
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f844299a392f9521afe3af7d72bc72272304d3a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354045"
---
# <a name="wide-world-importers-sample-databases-for-microsoft-sql"></a>宽世界导入 Microsoft SQL 示例数据库
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
这是虚构的公司广泛的导入程序以及在 SQL Server 和 Azure SQL 数据库的 WideWorldImporters 示例数据库中解决的工作流的概述。  

广泛的导入 (WWI) 是一种大规模的新奇商品导入程序和分销商操作。

作为 wholesaler，WWI 的客户主要是转售个人的公司。 WWI 在美国中销售零售客户，包括专业商店、supermarkets、计算商店、旅游引力店和某些个人。 WWI 还通过代表 WWI 促销产品的代理网络销售到其他批发商。 尽管 WWI 的所有客户当前都基于美国，公司仍打算将扩展推送到其他国家/地区。

WWI 从供应商购买货物，包括新奇和玩具制造商以及其他新奇批发商。 他们将货物放入其 WWI 仓库中，并根据需要从供应商重新排序以满足客户订单。 他们还购买大量的打包材料，并以较小的数量出售这些材料，以便为客户提供便利。

最近 WWI 开始销售各种 edible novelties，如 chilli 甜蜜。  公司以前不必处理 chilled 项。 现在，为了满足食物处理要求，他们必须在冷却器房间内监视其温度，以及任何具有冷却器部分的卡车。

## <a name="workflow-for-warehouse-stock-items"></a>仓库库存项的工作流

如何贮存和分发项目的典型流程如下：
- WWI 创建采购订单并将订单提交给供应商。
- 供应商发送项，WWI 接收这些项，并在其仓库中将它们股票。
- 客户从 WWI 订购项
- WWI 使用仓库中的库存项来填充客户订单，当他们没有足够的库存时，他们将从供应商处订购额外股票。
- 一些客户不想等待库存中的物品。 如果它们订购五个不同的股票项目，并且有四个可用项，则他们需要接收四项内容并将剩余项的延期。 然后，稍后会在单独的发货中发送该项目。
- WWI 通过将订单转换为发票来为股票商品开具发票。
- 客户可以对不在库存中的项进行排序。 这些项为订货不足。
- WWI 通过其自身的交付 van 或其他 couriers 或运费方法向客户提供股票商品。
- 客户将发票支付给 WWI。
- WWI 会定期为采购订单上的商品支付供应商。 这通常是收到货物后的一段时间。

## <a name="data-warehouse-and-analysis-workflow"></a>数据仓库和分析工作流

尽管 WWI 的团队使用 SQL Server Reporting Services 从 WideWorldImporters 数据库生成操作报表，但他们还需要对其数据执行分析并且需要生成战略性报表。 团队已在数据库 WideWorldImportersDW 中创建了维度数据模型。 此数据库由 Integration Services 包进行填充。

SQL Server Analysis Services 用于根据维度数据模型中的数据创建分析数据模型。 SQL Server Reporting Services 用于直接从维度数据模型和分析模型生成战略报表。 Power BI 用于通过相同数据创建仪表板。 仪表板用于网站、手机和平板电脑上。 *注意：这些数据模型和报表尚不可用*

## <a name="additional-workflows"></a>其他工作流

这些都是附加工作流。
- 当客户出于某种原因或货物出现故障时，WWI 会发出信用备注。 它们被视为负发票。
- WWI 定期计算库存物品的现有数量，以确保在其系统中显示为可用库存数量准确无误。  (的过程称为 stocktake) 。
- 冷房间温度。 Perishable 货物存储在 refrigerated 房间中。 这些会议室中的传感器数据引入到数据库中，以便进行监视和分析。
- 车辆位置跟踪。 传输 WWI 货物的车辆包含跟踪位置的传感器。 此位置将再次引入到数据库中，以便进行监视和进一步分析。

## <a name="fiscal-year"></a>财政年度

公司使用从11月1日开始的财务年度运营。

## <a name="terms-of-use"></a>使用条款

此处描述了示例数据库和示例代码的许可证： [license.txt](https://github.com/Microsoft/sql-server-samples/blob/master/license.txt)

示例数据库包含已从 data.gov 和天然 EarthData 加载的公共数据。 使用条款如下： [https://www.naturalearthdata.com/about/terms-of-use/](https://www.naturalearthdata.com/about/terms-of-use/)
