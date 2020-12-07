---
title: 执行命令
description: 介绍 Microsoft SqlClient Data Provider for SQL Server `Command` 对象，以及如何使用它对数据源执行查询和命令。
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 40494916-c25a-4cb8-8f7c-fcb8d378464e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: b1427fa78e52c985478996bfb41cb7a20e1ee608
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96428183"
---
# <a name="executing-a-command"></a>执行命令

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Microsoft SqlClient Data Provider for SQL Server 具有从 <xref:System.Data.Common.DbCommand> 继承的 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象。 此对象公开基于命令类型和所需返回值执行命令的方法，如下表所述。

|命令|返回值|  
|-------------|------------------|  
|`ExecuteReader`|返回一个 `DataReader` 对象。|  
|`ExecuteScalar`|返回一个标量值。|  
|`ExecuteNonQuery`|执行不返回任何行的命令。|  
|`ExecuteXMLReader`|返回 <xref:System.Xml.XmlReader>。 只用于 `SqlCommand` 对象。|

 每个强类型命令对象还支持指定如何解释命令字符串的 <xref:System.Data.CommandType> 枚举，如下表所述。

|CommandType|描述|
|-----------------|-----------------|  
|`Text`|定义要在数据源处执行的语句的 SQL 命令。|  
|`StoredProcedure`|存储过程的名称。 您可以使用某一命令的 `Parameters` 属性访问输入和输出参数，并返回值（无论调用哪种 `Execute` 方法）。|  
|`TableDirect`|表的名称。|

> [!IMPORTANT]
> 当使用 `ExecuteReader` 时，在关闭 `DataReader` 后才能访问返回值和输出参数。

## <a name="example"></a>示例

下面的代码示例演示如何创建 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象以通过设置其属性执行存储过程。 <xref:Microsoft.Data.SqlClient.SqlParameter> 对象用于指定存储过程的输入参数。 使用 <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A> 方法执行此命令，并在控制台窗口中显示 <xref:Microsoft.Data.SqlClient.SqlDataReader> 的输出。

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

### <a name="troubleshooting-commands"></a>命令疑难解答

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Microsoft SqlClient Data Provider for SQL Server 添加了性能计数器，使你能够检测与命令执行失败相关的间歇性问题。

## <a name="see-also"></a>请参阅

- [命令和参数](commands-parameters.md)
