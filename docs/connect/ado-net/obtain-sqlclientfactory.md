---
title: 获取 SqlClientFactory
description: 了解如何从 DbProviderFactories 类获取 SqlClientFactory，以处理 .NET 中的特定数据源。
ms.date: 12/22/2020
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 891cabf04bc8e63537983a91d8526a5cbdf238bf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771750"
---
# <a name="obtain-a-sqlclientfactory"></a>获取 SqlClientFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

获取 <xref:System.Data.Common.DbProviderFactory> 的过程涉及将有关数据提供程序的信息传递给 <xref:System.Data.Common.DbProviderFactories> 类。 <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> 方法将基于此信息创建一个强类型提供程序工厂。 例如，若要创建 <xref:Microsoft.Data.SqlClient.SqlClientFactory>，可以传递 `GetFactory` 字符串，该字符串的提供程序名称指定为“Microsoft.Data.SqlClient”。

`GetFactory` 的其他重载采用 <xref:System.Data.DataRow>。 创建该提供程序工厂后，可以使用其方法创建其他对象。 `SqlClientFactory` 的部分方法包括 <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateConnection%2A>、<xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateCommand%2A> 和 <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>。

## <a name="register-sqlclientfactory"></a>注册 SqlClientFactory

若要在 .NET Framework 中按 <xref:System.Data.Common.DbProviderFactories> 类检索 <xref:Microsoft.Data.SqlClient.SqlClientFactory> 对象，则需要在 App.config 或 web.config 文件中注册该对象。 下面的配置文件片断演示 <xref:Microsoft.Data.SqlClient> 的语法和格式。  

```xml  
<system.data>
  <DbProviderFactories>
    <add name="Microsoft SqlClient Data Provider"
      invariant="Microsoft.Data.SqlClient"
      description="Microsoft SqlClient Data Provider for SQL Server"
      type="Microsoft.Data.SqlClient.SqlClientFactory, Microsoft.Data.SqlClient, Version=2.0.20168.4, Culture=neutral, PublicKeyToken=23ec7fc2d6eaa4a5"/>
  </DbProviderFactories>
</system.data>  
```  

invariant 属性标识基础数据提供程序。 在创建新工厂时也使用这种由三部分组成的命名语法，并用于标识应用程序配置文件中的提供程序，以便在运行时能够检索提供程序名称及其关联的连接字符串。  

> [!NOTE]  
> 在 .NET core 中，由于没有 GAC 或全局配置支持，因此应通过调用项目中的 <xref:System.Data.Common.DbProviderFactories.RegisterFactory%2A> 方法来注册 <xref:Microsoft.Data.SqlClient.SqlClientFactory> 对象。

以下示例演示如何在 .NET Core 应用程序中使用 <xref:Microsoft.Data.SqlClient.SqlClientFactory>。

[!code-csharp[SqlClientFactory_Netcoreapp#1](~/../sqlclient/doc/samples/SqlClientFactory_Netcoreapp.cs#1)]

## <a name="see-also"></a>另请参阅

- [DbProviderFactories](dbproviderfactories.md)
- [连接字符串](connection-strings.md)
- [使用配置类](/previous-versions/aspnet/ms228063(v=vs.100))
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
