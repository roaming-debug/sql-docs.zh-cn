---
title: DbProviderFactories
description: 描述提供程序工厂模型及说明如何在 `System.Data.Common` 命名空间中使用基类。
ms.date: 12/22/2020
ms.assetid: 2a8e2640-3a49-42a1-a3a9-b43026907ae1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1a373e7bde1e93d8dc92294f983a96de64e28ae1
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771759"
---
# <a name="dbproviderfactories"></a>DbProviderFactories

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:System.Data.Common> 命名空间提供用于创建与特定数据源一起使用的 <xref:System.Data.Common.DbProviderFactory> 实例的类。 当创建 <xref:System.Data.Common.DbProviderFactory> 实例并向其传递有关数据提供程序的信息时，`DbProviderFactory` 可以根据为其提供的信息确定要返回的正确的强类型连接对象。  

数据提供程序 <xref:Microsoft.Data.SqlClient> 不再在 machine.config 文件中列出，但自定义提供程序将继续在此处列出。  

## <a name="in-this-section"></a>在本节中  

[获取 SqlClientFactory](obtain-sqlclientfactory.md)  
演示如何从 `DbProviderFactories` 类获取 `SqlClientFactory` 以处理 .NET 中的特定数据源。  

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
