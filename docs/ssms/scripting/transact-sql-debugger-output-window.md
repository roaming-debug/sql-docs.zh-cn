---
title: 输出窗口
description: 了解如何使用“输出”窗口查看 SQL Server Management Studio 调试器及其他工具的状态消息和其他输出。
titleSuffix: T-SQL Debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Output Window [Transact-SQL]
- Output Window [SQL Server Management Studio]
ms.assetid: 9808e00c-c8f6-45cc-896e-192b8420f747
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66eda4d5c7cf3d5098eec0f00e4cb5dbf50cd0d7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478718"
---
# <a name="transact-sql-debugger---output-window"></a>Transact-SQL 调试器 -“输出”窗口

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此窗口显示 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中各种功能的状态消息。 输出是从 **调试器、外部工具功能或调试器** “命令窗口” [!INCLUDE[tsql](../../includes/tsql-md.md)] 中运行的命令发送到 **“输出”** 窗口的特殊窗格中。 此外，来自外部工具（例如 .bat 或 .com 文件）且通常在“命令提示符”窗口中显示的输出也在此窗口中显示。

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
 **访问“输出”窗口**  
  
-   在 **“视图”** 菜单上单击 **“其他窗口”** ，再单击 **“输出”** 。  
  
## <a name="options"></a>选项  
 **输出窗格列表**  
 显示要查看的输出窗格的列表。 可能会有多个信息窗格可用，取决于哪些工具已经使用 **“输出”** 窗口将信息传递给用户。  
  
 **输出窗格**  
 显示“输出窗格列表”中所选窗格的输出结果。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-SQL 调试器](./transact-sql-debugger.md)