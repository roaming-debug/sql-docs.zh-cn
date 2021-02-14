---
title: 从旧版升级 Visual Studio 2010 自定义测试条件
description: 了解如何升级 Visual Studio 2010 自定义测试条件以在 SQL Server Data Tools 中使用。 查看要进行哪些更改以及如何安装该条件。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 44c895a3-dee0-4032-a60f-812f5fe3c713
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2e0f9813db19e06729f7164f0ca419801364a365
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018047"
---
# <a name="how-to-upgrade-a-visual-studio-2010-custom-test-condition-from-a-previous-release-to-sql-server-data-tools"></a>如何：将 Visual Studio 2010 自定义测试条件从早期版本升级到 SQL Server Data Tools

要使用在低于 SQL Server Data Tools 的版本中创建的测试单元条件，必须升级它：  
  
-   [更新引用](#UpdateReferences)  
  
-   [更新类属性和类型引用](#UpdateClassAttributesandTypeReference)  
  
-   [安装已升级的测试条件](#ApplytheNewRegistrationProcess)  
  
## <a name="update-references"></a><a name="UpdateReferences"></a>更新引用  
要更新项目引用：  
  
1.  仅对于 Visual Basic，在“解决方案资源管理器”中单击“显示所有文件”。  
  
2.  在“解决方案资源管理器”中，展开“引用”节点。  
  
3.  右键单击以下程序集引用，然后单击“删除”：  
  
    1.  Microsoft.Data.Schema.UnitTesting  
  
    2.  Microsoft.Data.Schema  
  
4.  在“项目”菜单上，或通过右键单击“解决方案资源管理器”中的项目文件夹，单击“添加引用”。  
  
5.  单击 .NET 选项卡。  
  
6.  在“组件名称”列表中，选择“System.ComponentModel.Composition”，然后单击“确定”。  
  
7.  添加所需的程序集引用。 右键单击项目节点，然后单击“添加引用”。 单击“浏览”，并导航到 C:\Program Files (x86)\\MicrosoftSQL Server\110\DAC\Bin 文件夹。 选择 Microsoft.Data.Tools.Schema.Sql.dll 并单击“添加”，然后单击“确定”。  
  
8.  在“项目”菜单上，单击“卸载项目”。  
  
9. 在“解决方案资源管理器”中右键单击“项目”，然后选择“编辑 `project_name` .csproj” 。  
  
10. 在导入 `Microsoft.CSharp.targets` 后添加以下 Import 语句：  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
11. 保存文件并将其关闭。 在“解决方案资源管理器”中右键单击该项目，然后选择“重新加载项目”。  
  
12. 打开该测试条件类，删除以 Microsoft.Data.Schema 开头的所有 using 语句。 这样做最简单的方式是右键单击该文件，然后选择“组织 using”，选择“移除和排序”。 必须删除以下 using 语句：  
  
    ```  
    using Microsoft.Data.Schema.UnitTesting;  
    using Microsoft.Data.Schema.UnitTesting.Conditions;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema;  
    ```  
  
13. 如果以下 using 语句不存在，请将它们添加到该文件：  
  
    ```  
    using System.ComponentModel;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
    ```  
  
你的测试条件现在使用 SQL Server 单元测试程序集引用。  
  
## <a name="update-class-attributes-and-type-references"></a><a name="UpdateClassAttributesandTypeReference"></a>更新类属性和类型引用  
将较旧的单元测试类属性替换为新属性。 SQL Server 单元测试可扩展性现在基于托管可扩展性框架 (MEF)。 您还必须更新一些类型引用。  
  
### <a name="update-class-attributes"></a>更新类属性  
按以下方式更新您的代码：  
  
1.  删除 `DatabaseSchemaProviderCompatibility` 属性。 此（这些）属性是以前版本的可扩展性功能要求的，在 SQL Server 单元测试中不受支持。  
  
2.  请删除 `DisplayName` 属性。 显示名称包含在新属性中。  
  
3.  添加新的 `ExportTestCondition` 属性。 此属性对于测试条件必须存在，才能在 SQL Server 单元测试中发现和使用。 `ExportTestCondition` 并替换 `DatabaseSchemaProviderCompatibility` 属性。 `ExportTestCondition` 采用两个参数：  
  
    -   `DisplayName` 是第一个参数。 它替换 `DisplayName` 属性，用于描述此类型的所有测试条件。  
  
    -   第二个参数用于唯一标识您的扩展。 您可以使用 `typeof(NewTestCondition)` 仅传递您的类型，因为它应是唯一的。 但是，如果愿意还可以传递字符串 ID。  
  
4.  您的类定义应按如下方式更改：  
  
    早于:  
  
    ```  
    [DatabaseSchemaProviderCompatibility(typeof(DatabaseSchemaProvider))]  
    [DatabaseSchemaProviderCompatibility(null)]  
    [DisplayName("NewTestCondition")]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
    晚于：  
  
    ```  
    [ExportTestCondition("NewTestCondition ", typeof(NewTestCondition))]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
### <a name="update-type-references"></a>更新类型引用  
一些类型名称已在 SQL Server 单元测试框架中更改。 要更新你的代码以使用新的类型名称，请使用“编辑”菜单中的“查找和替换”。 类型名称现在以 Sql 开头。 应按以下方式更新类名称：  
  
|旧类型名称|新类型名称|  
|-----------------|-----------------|  
|`ExecutionResult`|`SqlExecutionResult`|  
  
## <a name="install-the-upgraded-test-condition"></a><a name="ApplytheNewRegistrationProcess"></a>安装已升级的测试条件  
在以前版本的数据库单元测试中，可能要求您在全局程序集缓存中安装您的测试条件，或要求创建包含您的程序集信息的 XML 文件。 使用 SQL Server 单元测试时，不再需要这一额外过程。 （有关详细信息，请参阅[编译项目并且安装你的测试条件](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md#xxx)。  
  
在更新引用后，请验证您的程序集已签名和编译。  
  
接下来，将程序集文件从输出目录（默认情况下，这一目录为 My Documents\Visual Studio 2010\Projects\\\<yoursolutionname>\\<yourprojectname>\bin\Debug\\）复制到 %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions 目录。 启动 Visual Studio 时，它将标识 TestConditions 目录中的所有扩展并使它们可供在会话中使用：  
  
## <a name="upgrade-existing-tests-that-need-to-use-the-new-test-condition"></a>升级需要使用新测试条件的现有测试  
查找使用旧测试条件并需要使用新条件的所有测试项目。 确保升级这些测试项目。 有关详细信息，请参阅[升级包含数据库单元测试的较旧的测试项目](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md)。  
  
删除对旧测试条件的程序集引用。  
  
将一个新的 SQL Server 单元测试添加到该项目，以便创建对项目中已升级的测试条件的程序集引用。 为创建该引用，必须添加一个测试类。 您可以在添加引用后删除该测试类。  
  
## <a name="see-also"></a>另请参阅  
[SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
  
