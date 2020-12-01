---
title: SqlClient 中的数据发现和分类
description: 介绍如何检查 SQL Server 数据库是否支持数据分类，以及如何通过 SqlDataReader 对象访问数据分类信息。
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: ''
ms.openlocfilehash: 32c4968c4e734abf7bcb4addfde69bbdc5294d1c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123871"
---
# <a name="data-discovery-and-classification-in-sqlclient"></a>SqlClient 中的数据发现和分类

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

[数据发现和分类](../../../relational-databases/security/sql-data-discovery-and-classification.md)是一组高级服务，可用于发现数据库中的敏感数据并对其进行分类、标记和报告。 在基础数据源支持的情况下，SqlClient 会提供一个可公开只读“数据发现和分类”信息的 API。 可通过 SqlDataReader 访问此信息。

Microsoft.Data.SqlClient v2.1.0 引入了对数据分类的 `Sensitivity Rank` 信息的支持。 `Sensitivity Rank` 是基于预定义的一组值的标识符，这组值定义敏感度等级。 它由高级威胁防护等其他服务用于根据级别检测异常。 以下数据分类 API 现在可在 Microsoft.Data.SqlClient.DataClassification 命名空间中使用：

```csharp
// New in Microsoft.Data.SqlClient v2.1.0
public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}

public sealed class SensitivityClassification
{
  // Returns the sensitivity rank for the query associated with the active 'SqlDataReader'.
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the labels collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<Label> Labels;

  // Returns the information types collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<InformationType> InformationTypes;

  // Returns the column sensitivity for this 'SensitivityClassification' Object
  public ReadOnlyCollection<ColumnSensitivity> ColumnSensitivities;
}

public sealed class SensitivityProperty
{
  // Returns the sensitivity rank for this 'SensitivityProperty' Object
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the label for this 'SensitivityProperty' Object
  public Label Label;

  // Returns the information type for this 'SensitivityProperty' Object
  public InformationType InformationType;
}

public sealed class Label
{
  // Gets the name for this 'Label' object
  public string Name;

  // Gets the ID for this 'Label' object
  public string Id;
}

public sealed class InformationType
{
  // Gets the name for this 'InformationType' object
  public string Name;

  // Gets the ID for this 'InformationType' object
  public string Id;
}

public sealed class ColumnSensitivity
{
  // Returns the list of sensitivity properties as received from Server for this 'ColumnSensitivity' information      
  public ReadOnlyCollection<SensitivityProperty> SensitivityProperties;
}
```

> [!NOTE]
> 仅当 SQL Server 支持带有级别的数据分类时，Microsoft.Data.SqlClient 才读取 `Sensitivity Rank` 信息。 对于服务器使用不带级别的旧版本数据分类，查询的级别值为“未定义”。

此示例应用程序演示如何访问 SqlDataReader 的数据分类属性。

[!code-csharp [SqlDataReader_DataDiscoveryAndClassification#1](~/../sqlclient/doc/samples/SqlDataReader_DataDiscoveryAndClassification.cs#1)]


**另请参阅**  

 - [SQL Server 功能和 ADO.NET](sql-server-features-adonet.md)
 - [sys.sensitivity_classifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
 - [ADD SENSITIVITY CLASSIFICATION](../../../t-sql/statements/add-sensitivity-classification-transact-sql.md)