---
description: MSSQLSERVER_8632
title: MSSQLSERVER_8632
ms.custom: ''
ms.date: 10/27/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8632 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: b7c59634423bd6df623725720bf8bd0d262d5be0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348696"
---
# <a name="mssqlserver_8632"></a>MSSQLSERVER_8632
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|8632|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|QUERY_EXPRESSION_TOO_COMPLEX|
|消息正文|内部错误：达到了表达式服务限制。 请在您的查询中查找潜在的复杂表达式，并尝试简化它们。|
||

## <a name="explanation"></a>说明

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中运行查询时，如果在单个表达式中包含大量的标识符和常数，则会引发错误 8632。 将向用户报告如下错误消息：

> 服务器:消息 8632，级别 17，状态 2，行 1  
内部错误：达到了表达式服务限制。 请在您的查询中查找潜在的复杂表达式，并尝试简化它们。

## <a name="cause"></a>原因

出现此问题，是因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 限制一条查询的单个表达式中可包含的标识符和常量数。 此限制为 65,535。 例如，下面的查询只包含一个表达式：

```sql
select a, b + c, d + e
```

此表达式会检索所有五列，计算加法运算符，并将三个预测结果发送到客户端。

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 展开所有引用的标识符和常数后，对标识符和常数的数量进行测试。 例如，可能会展开以下项：

- 选择列表中的星号 (*)
- 视图
- 计算列定义

如果扩展后的数量超出限制，则查询将无法运行。

## <a name="user-action"></a>用户操作

若要规避此问题，请重写查询。 在查询中的最大表达式中引用更少的标识符和常数。 必须确保查询的每个表达式中标识符和常数的数目不超过此限制。 为此，可能需要将查询分解为多个查询。 然后，创建一个临时的中间结果。
