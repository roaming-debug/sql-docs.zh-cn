---
title: olapR R 包
description: olapR 是 Microsoft 提供的 R 包，用于针对 SQL Server Analysis Services OLAP 多维数据集进行 MDX 查询。 函数不支持所有 MDX 操作，但可以生成对维度进行详尽分析、向下钻取、汇总和透视的查询。 该包在 SQL Server 机器学习服务和 SQL Server 2016 R Services 中提供。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: fc9c222bfb1229deea7ef734658b1d3b6b86149d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470798"
---
# <a name="olapr-r-package-in-sql-server-machine-learning-services"></a>olapR（SQL Server 机器学习服务中的 R 包）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

olapR 是 Microsoft 提供的 R 包，用于针对 SQL Server Analysis Services OLAP 多维数据集进行 MDX 查询。 函数不支持所有 MDX 操作，但可以生成对维度进行详尽分析、向下钻取、汇总和透视的查询。 该包在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)和 [SQL Server 2016 R Services](sql-server-r-services.md) 中提供。

可以在连接到所有受支持 SQL Server 版本上的 Analysis Services OLAP 多维数据集时使用此包。 此时不支持连接到表格模型。

## <a name="load-package"></a>加载包

olapR 包未预加载到 R 会话中。 运行以下命令以加载包。

```R
library(olapR)
```

## <a name="package-version"></a>包版本

在所有仅适用于 Windows 的产品和提供该包的下载中，当前版本为 1.0.0。

## <a name="full-reference-documentation"></a>完整参考文档

olapr 包分布于多种 Microsoft 产品中，但不管你是在 SQL Server 还是在其他产品中获取该包，用法都是一样的。 由于函数相同，因此[单个 sqlrutils 函数的文档](/machine-learning-server/r-reference/olapr/olapr)仅发布到 Microsoft Machine Learning Server 的 [R 引用](/machine-learning-server/r-reference/introducing-r-server-r-package-reference)下的一个位置。 如果存在任何特定于产品的行为，这些差异将在函数帮助页中注明。

## <a name="availability-and-location"></a>可用性和位置

以下产品以及 Azure 上的多个虚拟机映像中都提供了此包。 包位置也会相应地变化。

Products | 位置 |
--------|----------|
SQL Server机器学习服务（包含 R 集成） | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library | 
SQL Server 2016 R Services | C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library
Microsoft Machine Learning Server (R Server) | C:\Program Files\Microsoft\R_SERVER\library |
Microsoft R Client | C:\Program Files\Microsoft\R Client\R_SERVER\library |
Data Science Virtual Machine （在 Azure 上） | C:\Program Files\Microsoft\R Client\R_SERVER\library |
SQL Server Virtual Machine（在 Azure 上）<sup>1</sup> | C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library |

<sup>1</sup> R 集成在 SQL Server 中是可选的。 在配置 VM 期间添加机器学习或 R 功能时，将安装 olapR 包。


## <a name="see-also"></a>另请参阅

[如何使用 olapR 创建 MDX 查询](how-to-create-mdx-queries-using-olapr.md)