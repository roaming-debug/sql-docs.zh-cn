---
title: 连接到数据源
description: 了解用于连接到 ADO.NET 中的数据源的连接对象。 你选择的 Connection 对象取决于数据源的类型。
ms.date: 11/13/2020
ms.assetid: 9abc3f92-1be3-4e1a-b360-762dc689650e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 0fdf91c5a4da33b585f10939cd6a801a46b236b3
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96761515"
---
# <a name="connecting-to-a-data-source-in-adonet"></a>连接到 ADO.NET 中的数据源

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

在 Microsoft SqlClient 数据提供程序中，通过在连接字符串中提供必要的身份验证信息，使用 Connection 对象连接到特定的数据源。 你使用的 Connection 对象取决于数据源的类型。

Microsoft SqlClient Data Provider for SQL Server 包括一个派生自 <xref:System.Data.Common.DbConnection> 类的 <xref:Microsoft.Data.SqlClient.SqlConnection> 类型。

## <a name="in-this-section"></a>本节内容  

[建立连接](establishing-connection.md)\
说明如何使用 Connection 对象与数据源建立连接。

[连接事件](connection-events.md)\
说明如何使用 InfoMessage 事件从数据源中检索信息性消息。

## <a name="see-also"></a>请参阅

- [连接字符串](connection-strings.md)
- [连接池](connection-pooling.md)
- [命令和参数](commands-parameters.md)
- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
