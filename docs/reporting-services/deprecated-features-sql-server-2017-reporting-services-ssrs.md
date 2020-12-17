---
title: SQL Server 2017 Reporting Services 中弃用的功能 | Microsoft Docs
description: 本文介绍 SQL Server Reporting Services 下一版本中将弃用的功能。
ms.date: 11/20/2019
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
monikerRange: '>= sql-server-2017'
ms.openlocfilehash: 553432d2e79069da19eacdd193be9b3b3e7d498c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425252"
---
# <a name="deprecated-features-in-sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services 中弃用的功能

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017](../includes/ssrs-appliesto-2017.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

当我们将某项功能标记为“弃用”时，这意味着：

- 该功能仅处于维护模式。 将不会进行任何新的更改，包括与新功能的互操作性有关的更改。
- 我们尽量不从将来的版本中删除已弃用的功能，使升级更简单。 但是，在极少数情况下，如果该功能限制了将来的创新，我们可能选择从 Reporting Services 中将其永久删除。
- 对于新的开发工作，不建议使用已弃用的功能。

## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>SQL Server 的下一版本中弃用的功能

以下 SQL Server 2017 Reporting Services 功能将在下一个 SQL Server 版本中弃用。 请不要在新的开发工作中使用这些功能，并尽快修改当前还在使用这些功能的应用程序。

> [!NOTE]
> 此列表与 SQL Server 2016 Reporting Services (13.x) 列表相同。 SQL Server 2017 Reporting Services (14.x) 未宣布弃用或停止使用任何新功能。


| **类别** | **弃用的功能** | **替代功能** |
| --- | --- | --- |
| 报表服务器 | HTML 4.0 呈现器。 | HTML 5 呈现器 |

## <a name="see-also"></a>另请参阅

[SQL Server Reporting Services (SSRS) 中停止使用的功能](discontinued-functionality-to-sql-server-reporting-services-in-sql-server.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
