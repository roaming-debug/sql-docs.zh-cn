---
title: SQL Server 选项页 - 查询执行
description: 选项（查询执行）
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Query_Execution.Sql_Server.General
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/13/2021
ms.openlocfilehash: 25ac37cdb9095151c90fdf81314d4c32044e6159
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193125"
---
# <a name="query-options-execution-general-page"></a>“查询选项”中的“执行”（“常规”页）

使用此页可指定用于运行 Microsoft SQL Server 查询的选项。 若要访问此对话框，请右键单击“查询编辑器”窗口的主体，再单击“查询选项”。

- SET ROWCOUNT  默认值为 0，指示 SQL Server 在收到所有结果之前将一直等待结果。 如果希望 SQL Server 在获取指定数目的行后暂停查询，请提供一个大于 0 的值。 若要关闭此选项（以便返回所有的行），请将 SET ROWCOUNT 指定为 0。

- SET TEXTSIZE 默认值为 2,147,483,647 个字节，表示 SQL Server 将针对 text、ntext、nvarchar(max) 和 varchar(max) 数据字段提供最高上限的数据。 它将不影响 XML 数据类型。 提供较小的数值，可以在存在大量值时限制结果数量。 超出指定数量的列将被截断。

- 执行超时  指示在取消查询之前等待的秒数。 值 0 指示无限期的等待或无超时。

- 批处理分隔符 键入用来将 Transact-SQL 语句分隔为批的词。 默认值为 GO。

- **默认情况下，在 SQLCMD 模式下打开新查询** 选中此复选框可在 SQLCMD 模式下打开新查询。 只有从“工具”菜单打开该对话框时，此复选框才可见。

    选择此选项时，请记住下列限制：

    - 数据库引擎查询编辑器中的 IntelliSense 处于关闭状态。

    - 由于查询编辑器不能从命令行运行，因此不能传入命令行参数（如变量）。

    - 由于查询编辑器无法响应操作系统提示，因此您一定要记住不要运行交互式语句。

- **重置为默认值** 将此页上的所有值重置为原始默认值。