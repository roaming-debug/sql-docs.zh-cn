---
title: 使用 CLR 数据库对象
description: 了解如何使用 .NET Framework 语言来检索和更新数据。 查看如何在项目中创建和使用公共语言运行 (CLR) 数据库对象。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.allowsqlclrdebugging
ms.assetid: 4a28d43d-eb5e-444d-aace-5df691f38709
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 02f5aa47948ee5cdbf2cc7f95d749a5f56e47328
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017947"
---
# <a name="how-to-work-with-clr-database-objects"></a>如何：使用 CLR 数据库对象

除了 Transact\-SQL 编程语言之外，还可以使用 .NET Framework 语言创建用于检索和更新数据的数据库对象。 使用托管代码编写的数据库对象被称为“SQL Server 公共语言运行时 (CLR) 数据库对象”。 有关使用在 SQL Server 中承载的 CLR 数据库对象的优点以及如何在 Transact\-SQL 和 CLR 之间进行选择的说明，请参阅 [CLR 集成的优点](../relational-databases/clr-integration/clr-integration-overview.md)和[使用托管代码创建数据库对象的优点](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100))。  
  
若要使用 SQL Server Data Tools 创建 CLR 数据库对象，应创建一个数据库项目，然后向其中添加一个 CLR 数据库对象。 与 Visual Studio 以前的版本不同的是，您无需创建不同的 CLR 项目然后从数据库项目添加对它的引用。 当您生成并发布数据库项目时，将会同时在该项目中自动发布 CLR 对象。 在发布这些 CLR 对象之后，可以像其他数据库对象一样调用和执行它们。  
  
CLR 属性页和 CLR 生成属性页包含很多关于在项目中使用 CLR 数据库对象的设置。 具体而言，CLR 属性页包含一个用于设置针对 CLR 程序集的权限的权限级别设置。 它还包含“生成 DDL”设置，用于控制是否生成添加到项目的 CLR 数据库对象的 DDL。 CLR 生成属性页包含所有编译器选项，您可以设置这些选项来配置项目中的 CLR 代码的编译。 通过在“解决方案资源管理器”中右键单击你的项目并选择“属性”，可以访问这些属性页。  
  
若要启用 CLR 数据库对象的调试，请打开 SQL Server 对象资源管理器。 右键单击包含你想要调试的 CLR 数据库项目的服务器，然后选择“允许 SQL/CLR 调试”。 此时将显示一个消息框，其中包含一条警告：“注意，调试过程中服务器上的所有托管线程都将停止。 是否要在此服务器上启用 SQL CLR 调试?”。 调试 CLR 数据库项目时，如果中断执行，则会中断服务器上的所有线程，从而会影响其他用户。 因此，不应在生产服务器上针对 CLR 数据库对象调试应用程序。 还应注意，一旦开始调试，更改 SQL Server 对象资源管理器中的设置就晚了。 在 SQL Server 对象资源管理器中所做的更改只有在开始下次调试会话时才会生效。  
  
有关生成 CLR 数据库对象的要求的更多信息，请参阅[使用公共语言运行时 (CLR) 集成生成数据库对象](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的之前过程中创建的实体。  
  
### <a name="to-add-a-clr-database-object-to-your-project"></a>向项目中添加 CLR 数据库对象  
  
1.  在“解决方案资源管理器”中，右键单击“TradeDev”数据库项目，选择“添加”，然后选择“新项”。  
  
2.  选择 C# SQL CLR 模板，然后选择“SQL CLR 用户定义函数”。 接受默认名称，然后单击“添加”。  
  
3.  将下面的代码添加到类正文中。 此函数验证美国电话号码。 它必须包含 3 个数字字符，可以选择用括号括起来，后随 3 个数字字符构成的一组数字，再之后是 4 个数字字符构成的一组数字。 支持的格式示例是 (425) 555-0123、425-555-0123、425 555 0123 和 1-425-555-0123。  
  
    ```  
  
    [SqlFunction(IsDeterministic = true, IsPrecise = true)]  
    public static SqlBoolean validatePhone(SqlString phone)  
    {  
        string aNorthAmericanPhoneNumberPattern = @"^[01]?[- .]?(\([2-9]\d{2}\)|[2-9]\d{2})[- .]?\d{3}[- .]?\d{4}$";  
        if (!phone.IsNull)  
        {  
           Regex regex = new Regex(aNorthAmericanPhoneNumberPattern);  
           return regex.IsMatch(phone.Value);  
        }  
        return true;  
     }  
    ```  
  
4.  请注意，`Regex` 将为红色且带下划线。 右键单击 `Regex` 并选择“解析”，然后选择“使用 System.Text.RegularExpressions”。  
  
5.  如果你正在开发 Microsoft SQL Server 2012 服务器实例，则可以跳过此步骤。 否则，SQL Server 2005 和 SQL Server 2008 仅支持使用 .NET Framework 的 2.0、3.0 或 3.5 版本生成的数据库项目。 为了确保正确设置了 .NET 目标平台，请在“解决方案资源管理器”中右键单击 TradeDev 数据库项目，然后选择“属性”。 在 SQLCLR 属性页中，将“目标平台”更改为 .NET Framework 3.5 或更低版本。 在最后一个屏幕中单击“是”，以便关闭后再重新打开该项目。  
  
6.  右键单击 TradeDev 项目，然后选择“生成”以便生成该项目。  
  
7.  双击 Suppliers.sql 并选择“视图设计器”以便在表设计器中打开 Suppliers 表。  
  
8.  单击列网格中的空行以便向该表添加新列。 为“名称”字段输入 phone，为“数据类型”字段输入 nvarchar (128)，保持“允许 Null 值”字段处于选中状态。  
  
9. 在上下文窗格中右键单击“CHECK 约束”节点，然后选择“添加新的 CHECK 约束”。  
  
10. 使用以下内容替换脚本窗格中该约束的默认定义。  
  
    ```  
    CONSTRAINT [CK_Suppliers_CheckPhone] CHECK (dbo.validatePhone(phone)=1),  
    ```  
  
    这将确保在新的电话字段中进行的任何输入都将使用我们之前添加的 CLR UDF 进行检查。  
  
11. 按 F5 以生成该项目并将该项目部署到本地数据库中。  
  
### <a name="to-use-clr-database-objects"></a>使用 CLR 数据库对象  
  
1.  在 SQL Server 对象资源管理器中，导航到要部署项目的本地数据库。  
  
2.  默认情况下，在 SQL Server 中会关闭 CLR 集成。 若要使用 CLR 数据库对象，必须启用 CLR 集成。 为此，请使用 sp_configure 存储过程的“clr enabled”选项。 有关详细信息，请参阅[“CLR 已启用”选项主题](../relational-databases/clr-integration/clr-integration-enabling.md)。  
  
    右键单击该数据库并选择“新建查询”。 在查询窗格中，粘贴以下代码并按“执行查询”按钮。  
  
    ```  
  
    sp_configure 'clr enabled', 1;  
    GO  
    RECONFIGURE;  
    GO  
    ```  
  
3.  右键单击 Suppliers 表并选择“查看数据”。  
  
4.  为 ID 输入 5，为“名称”输入 Contoso，保留“地址”为空，并且为“电话”输入 425 3122 1222。 按下 Tab 键离开“电话”字段，并且注意到将弹出一个消息，指示 `INSERT` 语句与你的现有 CHECK 约束冲突，该约束将使用预定义的电话模式检查“电话”字段的输入。  
  
5.  将你的输入更改为 425 312 1222 并按 Tab 键离开该字段。 请注意，这次将接受该输入。  
  
## <a name="see-also"></a>另请参阅  
[CLR 集成的优点](../relational-databases/clr-integration/clr-integration-overview.md)  
[使用托管代码创建数据库对象的优点](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100))  
[使用公共语言运行时 (CLR) 集成生成数据库对象](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)  
