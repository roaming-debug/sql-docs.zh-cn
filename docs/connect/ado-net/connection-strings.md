---
title: 连接字符串
description: 了解 Microsoft SqlClient Data Provider for SQL Server 中的连接字符串，其中包含作为参数从数据提供程序传递到数据源的初始化信息。
ms.date: 11/13/2020
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 257ed7d43f8ab204c7c7e7575c69251be6f2efdf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771617"
---
# <a name="connection-strings-in-adonet"></a>ADO.NET 中的连接字符串

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

连接字符串包含作为参数从数据提供程序传递到数据源的初始化信息。 数据提供程序接收连接字符串，作为 <xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType> 属性的值。 提供程序解析连接字符串，并确保语法正确且支持关键字。 然后 <xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType> 方法将已解析的连接参数传递到数据源。 数据源执行进一步验证并建立连接。

## <a name="connection-string-syntax"></a>连接字符串语法

连接字符串是使用分号分隔的键/值参数对列表：

```csharp
keyword1=value; keyword2=value;
```

关键字不区分大小写。 但是，值可能会区分大小写，具体取决于数据源。 关键字和值都可以包含[空白字符](https://en.wikipedia.org/wiki/Whitespace_character#Unicode)。 关键字和不带引号的值中忽略前导空格和尾随空格。

如果值包含分号、[Unicode 控制字符](https://en.wikipedia.org/wiki/Unicode_control_characters)、前导空格或尾随空格，则必须用单引号或双引号将其括起。 例如：

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

封闭字符不能出现在其括起的值内。 因此，包含单引号的值只能用双引号括起，反之亦然：

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

还可以通过同时使用两者来转义封闭字符：

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

引号本身以及等号不需要转义，因此以下连接字符串是有效的：

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

由于每个值都是读取到下一个分号或字符串的末尾，因此后一个示例中的值是 `a=b=c`，且最后的分号是可选的。

所有连接字符串都适用于上述相同的基本语法。 可识别的关键字集取决于提供程序。 Microsoft SqlClient Data Provider for SQL Server  支持旧 API 中的许多关键字，但通常更灵活，并接受许多常见连接字符串关键字的同义词。

键入错误会导致错误。 例如，“`Integrated Security=true`”是有效的，而“`IntegratedSecurity=true`”则会导致出错。

在运行时从无效用户手动输入构造的连接字符串易受字符串注入式攻击，从而危害数据源的安全。 为了解决这些问题，创建了 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> 类。 此[连接字符串生成器](connection-string-builders.md)类将参数作为强类型属性公开，并在连接字符串发送到数据源之前对其进行验证。

## <a name="in-this-section"></a>在本节中

[连接字符串生成器](connection-string-builders.md)\
演示如何使用 `ConnectionStringBuilder` 类在运行时构造有效的连接字符串。

[连接字符串和配置文件](connection-strings-and-configuration-files.md)\
演示如何在配置文件中存储和检索连接字符串。

[连接字符串语法](connection-string-syntax.md)\
介绍如何为 `SqlClient` 配置特定于提供程序的连接字符串。

[保护连接信息](protecting-connection-information.md)\
演示保护用于连接到数据源的信息的各项技术。

## <a name="see-also"></a>另请参阅

- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
