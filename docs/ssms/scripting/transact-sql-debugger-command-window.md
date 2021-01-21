---
title: “命令”窗口
description: 了解如何使用 Transact-SQL 调试器的“命令”窗口来运行调试命令以及如何在要调试的代码上编辑命令。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Command Window [Transact-SQL]
ms.assetid: e567ebf9-0793-451b-92c7-26193a02d9da
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9c895d2ac72f2c763357a2a2352009f8b2d7cab2
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597105"
---
# <a name="transact-sql-debugger---command-window"></a>Transact-SQL 调试器 -“命令”窗口

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用“命令”窗口以对 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 查询编辑器窗口中当前所调试的代码运行命令，例如调试和编辑命令。 只有在调试模式下才可以使用 **“命令窗口”** 。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器支持许多在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]“命令”窗口中支持的命令。 有关详细信息，请参阅 [Visual Studio“命令”窗口](/previous-versions/visualstudio/visual-studio-2015/ide/reference/command-window)。  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>任务列表

**访问“命令”窗口**

- 在 **“调试”** 菜单中，单击 **“启动调试”** 。

**打印变量的值**

- 在“命令”窗口中，键入“Debug.Print \<VariableName>”，然后按 Enter 键 。

**列出有关当前线程的信息**

- 在“命令窗口”中，键入 **Debug.ListThread**，然后按 Enter 键。

**将变量添加到“快速监视”窗口**

- 在“命令”窗口中，键入“Debug.QuickWatch \<VariableName>”，然后按 Enter 键 。

## <a name="see-also"></a>另请参阅

[Transact-SQL 调试器](./transact-sql-debugger.md)