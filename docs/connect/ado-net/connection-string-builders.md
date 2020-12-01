---
title: 连接字符串生成器
description: 了解用于 ADO.NET 中不同提供程序的连接字符串生成器类，它们都继承自 DbConnectionStringBuilder。
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 74031c0c3711ce768b919692fde0cb687060f227
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126370"
---
# <a name="connection-string-builders"></a>连接字符串生成器

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

在 ADO.NET 的早期版本中，不会对具有串联字符串值的连接字符串进行编译时检查，因此在运行时会产生不正确的关键字 <xref:System.ArgumentException>。 Microsoft SqlClient Data Provider for SQL Server 包括从 <xref:System.Data.Common.DbConnectionStringBuilder> 继承的强类型连接字符串生成器类 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType>。

## <a name="connection-string-injection-attacks"></a>连接字符串注入式攻击

当使用动态字符串串联根据用户输入生成连接字符串时，可能发生连接字符串注入式攻击。 如果未验证字符串并且未转义恶意文本或字符，则攻击者可能会访问服务器上的敏感数据或其他资源。 例如，攻击者可以通过提供分号并追加其他值来发起攻击。 通过使用“last one wins”算法解析连接字符串，恶意输入将替换为合法值。

连接字符串生成器类旨在排除推测，防止出现语法错误和安全漏洞。 它们提供与数据提供程序所允许的已知键/值对相对应的方法和属性。 每个类都保持一个固定的同义词集合，可以将同义词转换为相应的已知键名。 将执行键/值对的有效性检查，无效对会引发异常。 此外，还会以一种安全方式处理插入的值。

下面的示例演示 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> 如何处理为 `Initial Catalog` 设置插入的额外值。

[!code-csharp[SqlConnectionStringBuilder_InjectionAttack#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_InjectionAttack.cs#1)]

输出结果表明，<xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> 对其进行了正确的处理，方法是用双引号转义多余的值，而不是将它作为新的键/值对追加到连接字符串中。

```output
data source=(local);Integrated Security=True;
initial catalog="AdventureWorks;NewValue=Bad"
```

## <a name="building-connection-strings-from-configuration-files"></a>从配置文件生成连接字符串

如果事先知道连接字符串的某些元素，则可以将其存储在配置文件中，并在运行时检索它们以构造完整连接字符串。 例如，可能事先知道数据库的名称，但不知道服务器的名称。 或者，您可能希望用户在运行时提供用户名和密码，而不能在连接字符串中插入其他值。

连接字符串生成器的一个重载构造函数将 <xref:System.String> 作为自变量，这可让你提供部分连接字符串，然后通过用户输入使这部分连接字符串成为完整字符串。 该部分连接字符串可以存储在配置文件中并在运行时进行检索。

> [!NOTE]
> <xref:System.Configuration> 命名空间允许通过编程方式访问配置文件（对 Web 应用程序使用 <xref:System.Web.Configuration.WebConfigurationManager>，对 Windows 应用程序使用 <xref:System.Configuration.ConfigurationManager>）。 有关使用连接字符串和配置文件的详细信息，请参阅[连接字符串和配置文件](connection-strings-and-configuration-files.md)。

### <a name="example"></a>示例

此示例演示如何从配置文件中检索部分连接字符串并通过设置 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> 的 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A>、<xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> 和 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> 属性完成该连接字符串。 配置文件定义如下。

```xml
<connectionStrings>
  <clear/>
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"
    providerName="Microsoft.Data.SqlClient" />
</connectionStrings>
```

> [!NOTE]
> 必须在项目中设置对 `System.Configuration.dll` 的引用，才能运行代码。

[!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_UserNamePwd.cs#1)]
  
## <a name="see-also"></a>另请参阅

- [连接字符串](connection-strings.md)
