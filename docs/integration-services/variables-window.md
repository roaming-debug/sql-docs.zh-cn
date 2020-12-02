---
description: “变量”窗口
title: “变量”窗口 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.variables.f1
- sql13.dts.designer.variableoptionswindow.f1
helpviewer_keywords:
- Variables Window dialog box
ms.assetid: f405e5ce-ef69-4c58-8c7d-a3d44dfe9ab0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa326be8b57eed58f0aa52876d0d32b889320c58
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193734"
---
# <a name="variables-window"></a>“变量”窗口

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用“变量”窗口创建和修改用户定义变量，以及查看系统变量。  
  
 默认情况下， **“变量”** 窗口位于 **中 SSIS 设计器的** “连接管理器” [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]区域之下。 如果看不到“变量”窗口，请单击 SSIS 菜单上的“变量”以显示该窗口  。  
  
 您可以通过将 View.Variables 命令映射到在 **“选项”** 对话框的 **“键盘”** 页上选择的组合键来显示 **“变量”** 窗口。  
  
> [!NOTE]
>  **Name** 和 **Namespace** 属性的值必须以 Unicode 标准 2.0 定义的字母字符或下划线 (_) 开头。 后续字符可以是在 Unicode 标准 2.0 中定义的字母或数字，或是下划线 (\_)。  
  
## <a name="options"></a>选项  
 **添加变量**  
 添加用户定义变量。  
  
 **移动变量**  
 单击列表中的变量，然后单击“移动变量”更改变量作用域。 在 **“选择新作用域”** 对话框中，选择包或包中的容器、任务或事件处理程序，以更改变量作用域。  
  
 有关变量作用域的详细信息，请参阅 [Integration Services (SSIS) 变量](../integration-services/integration-services-ssis-variables.md)区域之下。  
  
 **删除变量**  
 从列表中选择变量，然后单击“删除变量”。  
  
 **网格选项**  
 单击此项可打开“变量网格选项”对话框，从中可以更改列选择并对“变量”窗口应用筛选器。 有关详细信息，请参阅 [变量网格选项]()。  
  
 **名称**  
 查看变量名称。 您可以更新用户定义变量的名称。  
  
 **范围**  
 查看变量的作用域。 变量的作用域可以是整个程序包，也可以是容器或任务。 变量的作用域必须足够大，以便变量对任何需要读取或设置其值的其他任务或组件都是可见的。  
  
 您可以更改作用域，方法是单击该变量，然后单击 **“变量”** 窗口中的 **“移动变量”** 。  
  
 **数据类型**  
 查看变量的数据类型。 对于用户定义变量，你可以从列表中选择数据类型。  
  
> [!NOTE]  
>  如果为变量指定表达式，则无法更改数据类型。  
  
 **值**  
 查看变量值。 您可以更新用户定义变量的值。 此值可以是文字或表达式，还可以是多线串。 若要为变量指定表达式，请单击 **“变量”** 窗口中的 **“表达式”** 列旁边的省略号按钮。  
  
 **Namespace**  
 查看命名空间名称。 用户定义变量最初在 **User** 命名空间中创建，不过你可以在“命名空间”字段中更改命名空间名称。 若要显示此列，请单击 **“网格选项”**。  
  
 **引发更改事件**  
 指示在值发生更改时是否引发 **OnVariableValueChanged** 事件。 您可以更新用户定义变量和系统变量的值。 默认情况下， **“变量”** 窗口不列出此列。 若要显示此列，请单击 **“网格选项”**。  
  
 **说明**  
 查看变量说明。 您可以更改用户定义变量的说明。 默认情况下， **“变量”** 窗口不列出此列。 若要显示此列，请单击 **“网格选项”**。  
  
 **表达式**  
 查看为变量指定的表达式。 若要指定表达式，请单击省略号按钮。  
  
 如果为变量指定了表达式，该变量旁边将显示特殊的图标标记。 这个特殊的图标标记还显示在设置有表达式的连接管理器和任务旁边。  

## <a name="variable-grid-options-dialog-box"></a>“变量网格选项”对话框
 使用 **“变量网格选项”** 对话框，可以选择将在 **“变量”** 窗口中显示的列并选择要应用于变量列表的筛选器。 有关相应变量属性的详细信息，请参阅 [Integration Services (SSIS) 变量](../integration-services/integration-services-ssis-variables.md)。  
  
### <a name="options-for-filter"></a>筛选器选项  
 **显示系统变量**  
 选择此项可在“变量”窗口中列出系统变量。 系统变量是预定义的。 您无法添加或删除系统变量。 可以修改 **“RaiseChangedEvent”** 属性设置。  
  
 此列表使用颜色进行标记。 系统变量呈灰色，而用户定义变量则呈黑色。  
  
 **显示所有作用域的变量**  
 选择以显示包范围内的变量以及包中的容器、任务和事件处理程序范围内的变量。 清除此选项以仅显示包范围内的变量以及所选容器、任务或事件处理程序范围内的变量。  
  
 有关变量作用域的详细信息，请参阅 [Integration Services (SSIS) 变量](../integration-services/integration-services-ssis-variables.md)区域之下。  
  
### <a name="options-for-columns"></a>列选项  
 选择希望在 **“变量”** 窗口中显示的列。  
  
-   **范围**  
  
-   **Data type**  
  
-   **值**  
  
-   **Namespace**  
  
-   **变量值更改时引发事件**  
  
-   **说明**  
  
-   **表达式**  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 变量](../integration-services/integration-services-ssis-variables.md)   
 [在包中使用变量](./integration-services-ssis-variables.md)   
 [Integration Services (SSIS) 表达式](../integration-services/expressions/integration-services-ssis-expressions.md)   
 [生成包执行的转储文件](../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
