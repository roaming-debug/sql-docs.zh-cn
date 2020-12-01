---
title: 连接池
description: 了解连接池，这是一种优化技术，ADO.NET 使用它来最大程度地降低打开与数据源的连接的成本。
ms.date: 11/13/2020
ms.assetid: 955c057f-aea8-4ba8-aa6d-e3dfa18ba8d5
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 41842a2eb754aedc31bad206ad427a86bb65f00f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126369"
---
# <a name="connection-pooling"></a>连接池

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

连接到数据源可能需要很长时间。 为了最大程度地降低打开连接的成本，ADO.NET 使用一种称为“连接池”的优化技术，这种技术可最大程度地降低重复打开和关闭连接所造成的成本。

## <a name="in-this-section"></a>本节内容  

[SQL Server 连接池 (ADO.NET)](sql-server-connection-pooling.md) 概述连接池并说明 SQL Server 中连接池的工作原理。

## <a name="see-also"></a>另请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
