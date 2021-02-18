---
title: 编辑断点位置
description: 了解如何将 Transact-SQL 脚本文件中的断点移至脚本中的其他位置或移至其他脚本。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint location
ms.assetid: 5c28e411-0377-4210-a7ce-2a5c13dcdf74
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a5cc11057258c1a85f6b185187055a04e05b7689
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354653"
---
# <a name="edit-a-breakpoint-location"></a>编辑断点位置

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

断点位置指定断点在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本文件中所处的行和字符位置。 您可以编辑断点位置以将断点移至脚本的其他位置，或移至其他脚本。

## <a name="editing-a-location"></a>编辑位置

编辑断点位置时，断点将移至新的位置，并携带所有现有属性一起移动，例如命中计数或条件。  

#### <a name="to-edit-a-breakpoint-location"></a>编辑断点位置

1. 在编辑器窗口中，右键单击断点符号，然后单击快捷菜单上的“位置”。  
  
     -或-  
  
     在“断点”窗口中，右键单击断点符号，然后单击快捷菜单的“位置”。  
  
2. 在 **“文件断点”** 对话框中，编辑 **“文件”** 以指定新的文件，编辑 **“行”** 以指定新行，或者编辑 **“字符”** 以指定该行内的新位置。 如果您指定的新文件已经在查询编辑器窗口中打开，则断点会移至该编辑器窗口。 如果文件未打开，则会打开新的编辑器窗口，并在其中载入文件，断点随即移至新的位置。  
  
     **“允许源代码与原始版本不同”** 选项在调试 [!INCLUDE[tsql](../../includes/tsql-md.md)]时不起作用。  
  
## <a name="see-also"></a>另请参阅

- [指定命中计数](./specify-a-hit-count.md)
- [指定断点操作](./specify-a-breakpoint-action.md)
- [指定断点条件](./specify-a-breakpoint-condition.md)
- [指定断点筛选器](./specify-a-breakpoint-filter.md)