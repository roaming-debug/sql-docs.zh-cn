---
description: MSSQLSERVER_6602
title: MSSQLSERVER_6602
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 6602 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 594898082fb21f712cfb99abfb535aea46b5164d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348721"
---
# <a name="mssqlserver_6602"></a>MSSQLSERVER_6602
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|6602|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|XMLERR_PARSEERR2|
|消息正文|错误说明是 '%.*ls'。|
||

## <a name="explanation"></a>说明

当你尝试在 `xmltext` 参数的内容为复杂的 XML 文档的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行 `sp_xml_preparedocument` 存储过程时，将发生此错误，并向用户报告如下错误消息

> XML 文本“\<XML document sample>”附近的行号 1 处出现 XML 分析错误 0x80004005。  
消息 6602，级别 16，状态 2，过程 sp_xml_preparedocument，行 1  
错误说明为“未指定错误”。

## <a name="cause"></a>原因

之所以出现此问题，是因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的 MSXML 分析器 (Msxmlsql.dll) 存在设计限制。

严格来说，此问题与 XML 文档大小无关，而是与它的复杂结构相关。 XML 元素的结构深度、属性的数量和大小以及属性中的实体数的组合都可能导致此问题。 但是，在几兆字节的 XML 文档中可以找到达到此限制所需的复杂性级别。

## <a name="user-action"></a>用户操作

若要解决此问题，请尝试降低 XML 文档的复杂性。

> [!NOTE]
> 请注意包含多个 XML \ 实体的非常大的单个字符串属性。
