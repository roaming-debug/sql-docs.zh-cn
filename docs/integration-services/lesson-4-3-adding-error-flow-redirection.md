---
description: 第 4-3 课：添加错误流重定向
title: 步骤 3：添加错误流重定向 | Microsoft Docs
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5683a45d-9e73-4cd5-83ca-fae8b26b488c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac63d922423bcbb9c0c7f513d34a5834b3f0c1b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352019"
---
# <a name="lesson-4-3-add-error-flow-redirection"></a>第 4-3 课：添加错误流重定向

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在上一个任务中，转换尝试处理已损坏的示例平面文件时，Lookup Currency Key 无法生成匹配项，从而产生错误。 由于转换针对错误输出使用了默认设置，因此，任何错误都将导致该转换失败。 当转换失败时，该包的其余部分也将失败。  
  
可以使用错误输出将组件配置为将失败的行重定向到其他处理路径，而不是允许转换失败。 使用单独的错误处理路径提供了更多选项。 例如，可以清除数据，然后重新处理失败的行。 或者，可以保存失败的行及其错误信息，以便以后进行验证和重新处理。  
  
在本任务中，你将 Lookup Currency Key 转换配置为将所有失败的行重定向到错误输出。 在数据流的错误分支中，[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 会将这些行写入文件。  
  
默认情况下，[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 错误输出中的两个额外列 ErrorCode 和 ErrorColumn 仅包含数字错误代码和发生错误的列的 ID。 在本任务中，在程序包将失败的行写入文件之前，使用脚本组件来访问 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] API 并获取错误说明。  
  
## <a name="configure-an-error-output"></a>配置错误输出  
  
1.  在“SSIS 工具箱”中，展开“公共”，然后将“脚本组件”拖动到“数据流”选项卡的设计图面上。将“脚本”放置在“Lookup Currency Key”转换的右侧。  
  
2.  在“选择脚本组件类型”对话框中，选择“转换”，然后选择“确定”。  
  
3.  要连接这两个组件，请选择“Lookup Currency Key”转换，然后将其红色箭头拖到新的“脚本”转换中。  
  
    红色箭头表示“Lookup Currency Key”转换的错误输出。 通过使用红色箭头将转换连接到脚本组件，可以将所有处理错误重定向到脚本组件，该组件会处理这些错误并将其发送到目标。  
  
4.  在“配置错误输出”对话框的“错误”列中，选择“重定向行”，再单击“确定”。  
  
5.  在“数据流”设计图面上，在新的“ScriptComponent”中单击名称“脚本组件”，然后将该名称更改为“获取错误说明”。  
  
6.  双击“Get Error Description”转换。  
  
7.  在“脚本转换编辑器”对话框中的“输入列”页中，选择“ErrorCode”列。  
  
8.  在“输入和输出”页中，展开“输出 0”，依次选择“输出列”、“添加列”。  
  
9. 在“名称”属性中，输入“ErrorDescription”并将“DataType”属性设置为“Unicode 字符串 [DT_WSTR]”。  
  
10. 在“脚本”页中，验证是否已将“LocaleID”属性设置为“英语(美国)”  。
  
11. 选择“编辑脚本”，打开 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA)。 在“Input0_ProcessInputRow”方法中，键入或粘贴以下代码：  
  
    [Visual Basic]  
  
    ```vb  
    Row.ErrorDescription =   
      Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
    ```  
  
    [Visual C#]  
  
    ```cs
    Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
    ```  
  
    已完成的子例程如以下代码所示：  
  
    [Visual Basic]  
  
    ```vb
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
      Row.ErrorDescription =   
        Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
  
    End Sub  
    ```  
  
    [Visual C#]  
  
    ```cs
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
        {  
  
            Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
  
        }  
    ```  
  
12. 在“生成”菜单上，选择“生成解决方案”，生成脚本并保存所做的更改，然后关闭 VSTA。  
  
13. 单击“确定”，关闭“脚本转换编辑器”对话框。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 4：添加平面文件目标](../integration-services/lesson-4-4-adding-a-flat-file-destination.md)  
  
  
  
