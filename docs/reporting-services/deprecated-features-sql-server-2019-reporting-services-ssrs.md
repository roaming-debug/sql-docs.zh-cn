---
title: SQL Server 2019 Reporting Services 中弃用的功能 | Microsoft Docs
description: 本文介绍了下一版 SQL Server Reporting Services 中将弃用的 SQL Server 2019 Reporting Services 功能。
ms.date: 08/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, backward compatibility
- deprecated features [Reporting Services]
- HTML OWC rendering extension [Reporting Services]
- Report Server Web service, endpoints
ms.assetid: 3876c01e-f81d-4cce-9104-5106a8c369e6
author: maggiesMSFT
ms.author: maggies
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: d5f173bf7fa557a963a55b0a88a9dc48fae74a66
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425202"
---
# <a name="deprecated-features-in-sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services 中弃用的功能

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2019-and-later](../includes/ssrs-appliesto-2019-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../includes/ssrs-appliesto-pbirs.md)]

当我们将某项功能标记为“弃用”时，这意味着：

- 该功能仅处于维护模式。 将不会进行任何新的更改，包括与新功能的互操作性有关的更改。
- 我们尽量不从将来的版本中删除已弃用的功能，使升级更简单。 但是，在极少数情况下，如果该功能限制了将来的创新，我们可能选择从 Reporting Services 中将其永久删除。
- 对于新的开发工作，不建议使用已弃用的功能。

**SQL Server 未来版本中弃用的功能**

SQL Server Reporting Services 支持 SQL Server 下一版本中的以下功能，但会在更高版本中弃用它们。 具体是哪个 SQL Server 版本现在还未确定。

| **类别** | **弃用的功能** | **替代功能** |
| --- | --- | --- |
| 报表服务器 | 报表部件库 | 无 |
| 报表服务器 | 移动报表和移动报表发布服务器 | Power BI 报表服务器中的 Power BI 报表提供移动功能。 |
| 报表服务器 | XLS 和 DOC 呈现格式 | XLSX 和 DOCX 格式可用且受支持。 |
| 报表服务器 | Atom 数据馈送 | oData 源支持适用于 SSRS 和 Power BI 报表服务器中的共享数据集。 |
| 报表服务器 | 固定到 Power BI | Power BI 服务中现在直接提供分页报表支持。  |

## <a name="see-also"></a>另请参阅

[SQL Server 2019 Reporting Services (SSRS) 中停止使用的功能](discontinued-functionality-sql-server-reporting-services-2019.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
