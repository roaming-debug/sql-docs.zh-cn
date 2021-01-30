---
description: SQL 最低语法
title: SQL 最低语法 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 4f36d785-104f-4fec-93be-f201203bc7c7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6d611fb753034a7cde370f87700aa8b8c3db2e11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159076"
---
# <a name="sql-minimum-grammar"></a>SQL 最低语法
本部分介绍 ODBC 驱动程序必须支持的最小 SQL 语法。 本部分中描述的语法是 SQL-92 的入门级语法的子集。  
  
 应用程序可以使用本节中的任意语法，确保任何符合 ODBC 标准的驱动程序都支持该语法。 若要确定是否支持此部分中未包含的 SQL-92 的其他功能，应用程序应调用 SQLGetInfo 信息类型 SQL_SQL_CONFORMANCE 为的 。 即使驱动程序不符合任何 SQL-92 一致性级别，应用程序仍可以使用本节中所述的语法。 另一方面，如果驱动程序符合 SQL-92 级别，则它支持包含在该级别中的所有语法。 这包括此部分中的语法，因为此处所述的最小语法是最低 SQL-92 一致性级别的纯子集。 应用程序了解支持的 SQL-92 级别后，可以通过使用与该功能对应的单个信息类型调用 **SQLGetInfo** ，来确定是否支持较高级别的功能)  (。  
  
 仅适用于只读数据源的驱动程序可能不支持本部分中包含的用于处理更改数据的语法部分。 应用程序可以通过使用 SQL_DATA_SOURCE_READ_ONLY 信息类型调用 **SQLGetInfo** 来确定数据源是否为只读。  
  
## <a name="statement"></a>语句  
 *create-table 语句* ：： =  
  
 CREATE TABLE *基准表名称*  
  
  (*列标识符数据类型* [*，列标识符数据类型*] ... )   
  
> [!IMPORTANT]  
>  作为 *create table 语句* 中的 *数据类型*，应用程序必须使用 **SQLGetTypeInfo** 返回的结果集的 TYPE_NAME 列中的数据类型。  
  
 *delete-已搜索语句* ：： =  
  
 从表中删除 *-名称* [WHERE *搜索条件*]  
  
 *drop table 语句* ：： =  
  
 删除表 *基-表名*  
  
 *insert 语句* ：： =  
  
 插入 *表名称* [ ( *列标识符* [， *列标识符*] ... ) ] 值 (*插入值*[， *插入值*] ... )   
  
 *select-statement* ::=  
  
 选择 [全部 &#124; DISTINCT] *选择列表*  
  
 FROM *表引用-列表*  
  
 [WHERE *搜索条件*]  
  
 [*逐子句*]  
  
 *语句* ：： = *create table 语句*  
  
 &#124; *delete-搜索*  
  
 &#124; *drop table 语句*  
  
 &#124; *insert 语句*  
  
 &#124; *select 语句*  
  
 &#124; *更新-搜索*  
  
 *update-搜索*  
  
 更新 *表-名称*  
  
 设置 *列标识符* = {*expression* &#124; NULL}  
  
 [， *列标识符* = {*expression* &#124; NULL}] .。。  
  
 [WHERE *搜索条件*]  
  
 本部分包含以下主题。  
  
-   [SQL 语句中使用的元素](../../../odbc/reference/appendixes/elements-used-in-sql-statements.md)  
  
-   [数据类型支持](../../../odbc/reference/appendixes/data-type-support.md)  
  
-   [参数数据类型](../../../odbc/reference/appendixes/parameter-data-types.md)  
  
-   [参数标记](../../../odbc/reference/appendixes/parameter-markers.md)
