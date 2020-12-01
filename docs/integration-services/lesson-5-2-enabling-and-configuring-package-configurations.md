---
description: 第 5-2 课：启用和配置包配置
title: 步骤 2：启用和配置包配置 | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 005218ab-8dd5-48e9-a185-6bc60cd43a7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7237d7356ffb97861779c4d5427fd4fb05083695
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88345323"
---
# <a name="lesson-5-2-enable-and-configure-package-configurations"></a>第 5-2 课：启用和配置包配置

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在此任务中，将项目转换为包部署模型并使用包配置向导配置包。 使用此向导生成 XML 配置文件，该文件包含 Foreach 循环容器的 Directory 属性的配置设置。 Directory 属性的值由新的包级别变量提供，该变量可以在运行时进行更新。 此外，还填充了一个新的示例数据文件夹以用于测试。  
  
## <a name="create-a-package-level-variable-mapped-to-the-directory-property"></a>创建映射到 Directory 属性的包级别变量  
  
1.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，选择“控制流”选项卡的背景。 此选择会将所创建变量的作用域设置为包。  
  
2.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 菜单中，选择“变量”。  
  
3.  在“变量”窗口中，选择“添加变量”图标。  
  
4.  在“名称”框中，输入 varFolderName。  
  
    > [!IMPORTANT]  
    > 变量名称区分大小写。  
  
5.  确认“作用域”框显示包的名称 Lesson 5。  
  
6.  将 `varFolderName` 变量的“数据类型”框的值设置为“字符串”。  
  
7.  返回到“控制流”选项卡，然后双击“文件夹中的 Foreach 文件”容器。  
  
8.  在“Foreach 循环编辑器”的“集合”页上，依次选择“表达式”和省略号按钮 (…)。  
  
9. 在“属性表达式编辑器”中，选择“属性”列表，然后选择“Directory”。  
  
10. 在“表达式”框中，选择省略号按钮 (…)。  
  
11. 在“表达式生成器”中，展开“变量和参数”文件夹，然后将变量 User::varFolderName 拖动到“表达式”框中 ****。  
  
12. 选择“确定”以退出“表达式生成器”。  
  
13. 选择“确定”以退出“属性表达式编辑器”。  
  
14. 选择“确定”以退出“Foreach 循环编辑器”。  
  
## <a name="enable-package-configurations"></a>启用包配置  
  
1.  在“项目菜单”上，选择“转换为包部署模型”。  
  
2.  出现警告提示时选择“确定”，转换完成后，在“转换为包部署模型”对话框中选择“确定”。  
  
3.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，选择“控制流”选项卡的背景。  
  
4.  在“SSIS”菜单上，选择“包配置”。  
  
5.  在“包配置组织程序”对话框中，选择“启用包配置”，然后选择“添加”。  
  
6.  在“包配置向导”的欢迎页中，选择“下一步”。  
  
7.  在“选择配置类型”页上，确认“配置类型”已设置为“XML 配置文件”。  
  
8.  在“选择配置类型”页上，选择“浏览”。  
  
9. “选择配置文件位置”对话框将打开至项目文件夹。  
  
10. 在“选择配置文件位置”对话框的“文件名”中，输入 SSISTutorial，然后选择“保存”。  
  
11. 在“选择配置类型”页上，选择“下一步”。
  
12. 在“选择要导出的属性”页的“对象”窗格中，依次展开“变量”、“varFolderName”和“属性”，然后选择“Value”。  
  
13. 在“选择要导出的属性”页上，选择“下一步”。  
  
14. 在“完成向导”页上，输入该配置的配置名称，例如 SSIS Tutorial Directory configuration。 配置名显示在“包配置组织程序”对话框中。  
  
15. 选择“完成”  。  
  
16. 选择“关闭”。  
  
17. 向导将创建名为 SSISTutorial.dtsConfig 的配置文件，该文件包含特定变量的“值”的配置设置，且该变量可用于设置枚举器的 Directory 属性  。  
  
    > [!NOTE]  
    > 配置文件通常包含有关包属性的复杂信息，但对于本教程，唯一的配置信息应当如下所示：

    ```
    <Configuration 
        ConfiguredType="Property"  
        Path="\Package.Variables[User::varFolderName].Properties[Value]" 
        ValueType="String">  
      <ConfiguredValue></ConfiguredValue>  
    </Configuration>
    ```
  
## <a name="create-and-populate-a-new-sample-data-folder"></a>创建并填充新的示例数据文件夹  
  
1.  在 Windows 资源管理器中，在驱动器的根位置（例如，C:\\）创建名为 New Sample Data 的新文件夹。  
  
2.  找到计算机上的示例文件并从文件夹复制其中的三个文件。  
  
3.  在 **New Sample Data** 文件夹中，粘贴所复制的文件。  
  
## <a name="go-to-next-task"></a>转到下一个任务  
[步骤 3：修改 Directory 属性配置值](../integration-services/lesson-5-3-modifying-the-directory-property-configuration-value.md)  
  
