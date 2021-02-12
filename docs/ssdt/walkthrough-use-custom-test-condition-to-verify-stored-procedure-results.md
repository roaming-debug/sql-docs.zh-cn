---
title: 用于验证存储过程结果的自定义测试条件
description: 演练设置自定义测试条件的步骤，用于检查存储过程是否返回正确的列数。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 4c33b494-a85e-4dd2-97b6-c88ee858a99c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d434df88cae0a5fb28add27cc9785e659f059ced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042487"
---
# <a name="walkthrough-using-a-custom-test-condition-to-verify-the-results-of-a-stored-procedure"></a>演练：使用自定义测试条件来验证存储过程的结果

在本功能扩展演练中，将创建一个测试条件，并且将通过创建 SQL Server 单元测试来验证其功能。 此过程包括为该测试条件创建一个类库项目，并且对该项目进行签名和安装它。 如果已有要更新的测试条件，请参阅[如何：将 Visual Studio 2010 自定义测试条件从早期版本升级到 SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md)。  
  
本演练演示以下任务：  
  
-   如何创建测试条件。  
  
-   如何用强名称为程序集签名。  
  
-   如何添加对项目的必需的引用。  
  
-   如何生成测试条件。  
  
-   如何安装新的测试条件。  
  
-   如何测试新的测试条件。  
  
若要完成本演练，必须具有 Visual Studio 2010 或 Visual Studio 2012 以及 SQL Server Data Tools 的最新版本。 有关详细信息，请参阅[安装 SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md)。  
  
## <a name="creating-a-custom-test-condition"></a>创建自定义测试条件  
首先，您将创建一个类库。  
  
1.  在“文件”菜单上，单击“新建”，然后单击“项目”。  
  
2.  在“新建项目”对话框中的“项目类型”下，单击 Visual C\#。  
  
3.  在“模板”下，选择“类库”。  
  
4.  在“名称”文本框中，键入 ColumnCountCondition，然后单击“确定”。  
  
接下来，对该项目进行签名。  
  
1.  在“项目”菜单上，单击“ColumnCountCondition 属性”。  
  
2.  在“签名”选项卡上，选中“为程序集签名”复选框。  
  
3.   在“选择强名称密钥文件”框中，单击“\<New...>”。  
  
    “创建强名称密钥”对话框随即出现。  
  
4.  在“密钥文件名称”框中，键入 SampleKey。  
  
5.  键入并确认密码，然后单击“确定”。 生成您的解决方案时，将使用该密钥文件对程序集进行签名。  
  
6.  在“文件”  菜单上，单击“全部保存” 。  
  
7.  在“生成”菜单中，单击“生成解决方案”。  
  
接下来，您将添加对项目的必需的引用。  
  
1.  在“解决方案资源管理器”中，选择 ColumnCountCondition 项目。  
  
2.  在“项目”菜单中，单击“添加引用”以便显示“添加引用”对话框。  
  
3.  选择 .NET 选项卡。  
  
4.  在“组件名称”列中，找到并选择 System.ComponentModel.Composition 组件。 在选择该组件后，单击“确定”。  
  
5.  添加所需的程序集引用。 右键单击项目节点，然后单击“添加引用”。 单击“浏览”，并导航到 C:\Program Files (x86)\\MicrosoftSQL Server\110\DAC\Bin 文件夹。 选择 Microsoft.Data.Tools.Schema.Sql.dll 并单击“添加”，然后单击“确定”。  
  
6.  在“项目”菜单上，单击“卸载项目”。  
  
7.  在“解决方案资源管理器”中右键单击项目，然后选择“编辑 <project name>.csproj”。  
  
8.  在导入 Microsoft.CSharp.targets 后添加以下 Import 语句：  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. 保存文件并将其关闭。 在“解决方案资源管理器”中右键单击该项目，然后选择“重新加载项目”。  
  
    所需引用将显示在“解决方案资源管理器”中该项目的“引用”节点下。  
  
## <a name="creating-the-resultsetcolumncountcondition-class"></a>创建 ResultSetColumnCountCondition 类  
现在，将 Class1 重命名为 ResultSetColumnCountCondition 并从 [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) 派生它。 ResultSetColumnCountCondition 类是一个简单的测试条件，该条件验证在结果集中返回的列数。 您可以使用此条件确保某一存储过程的协定是正确的。  
  
1.  在“解决方案资源管理器”中，右键单击 Class1.cs，单击“重命名”，然后键入 ResultSetColumnCountCondition.cs。  
  
2.  单击“是”以便确认重命名对 Class1 的所有引用。  
  
3.  打开 ResultSetColumnCountCondition.cs 文件并且将以下 using 语句添加到该文件：  
  
    ```  
    using System;  
    using System.ComponentModel;  
    using System.Data;  
    using System.Data.Common;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
    namespace ColumnCountCondition {  
        public class ResultSetColumnCountCondition  
    ```  
  
4.  从 [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) 派生该类：  
  
    ```  
    public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
5.  添加 [ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103))。 请参阅[如何：为 SQL Server 单元测试设计器创建测试条件](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md)，了解有关 UnitTesting.Conditions.ExportTestConditionAttribute 的详细信息。  
  
    ```  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
        public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
6.  创建成员变量和构造函数：  
  
    ```  
            private int _resultSet;  
            private int _count;  
            private int _batch;  
  
            public ResultSetColumnCountCondition() {  
                _resultSet = 1;  
                _count = 0;  
                _batch = 1;  
            }  
    ```  
  
7.  重写 Assert 方法。 该方法包含 IDbConnection 参数（表示与数据库的连接）以及 SqlExecutionResult 参数。 该方法使用 DataSchemaException 进行错误处理：  
  
    ```  
           //method you need to override  
            //to perform the condition verification  
            public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
            {  
                //call base for parameter validation  
                base.Assert(validationConnection, results);  
  
                //verify batch exists  
                if (results.Length < _batch)  
                    throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
                SqlExecutionResult result = results[_batch - 1];  
  
                //verify resultset exists  
                if (result.DataSet.Tables.Count < ResultSet)  
                    throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
                DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
                //actual condition verification  
                //verify resultset column count matches expected  
                if (table.Columns.Count != Count)  
                    throw new DataException(String.Format(  
                        "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                        ResultSet, table.Columns.Count, Count));  
            }  
  
    Add the following method, which overrides the ToString method:  
    C#  
            //this method is called to provide the string shown in the  
            //test conditions panel grid describing what the condition tests  
            public override string ToString()  
            {  
                return String.Format(  
                    "Condition fails if ResultSet {0} does not contain {1} columns",  
                    ResultSet, Count);  
            }  
    ```  
  
8.  通过使用 CategoryAttribute、DisplayNameAttribute 和 DescriptionAttribute 属性，添加以下测试条件属性：  
  
    ```  
            //below are the test condition properties  
            //that are exposed to the user in the property browser  
            #region Properties  
  
            //property specifying the resultset for which  
            //you want to check the column count  
            [Category("Test Condition")]  
            [DisplayName("ResultSet")]  
            [Description("ResultSet Number")]  
            public int ResultSet  
            {  
                get { return _resultSet; }  
  
                set  
                {  
                    //basic validation  
                    if (value < 1)  
                        throw new ArgumentException("ResultSet cannot be less than 1");  
  
                    _resultSet = value;  
                }  
            }  
  
            //property specifying  
            //expected column count  
            [Category("Test Condition")]  
            [DisplayName("Count")]  
            [Description("Column Count")]  
            public int Count  
            {  
                get { return _count; }  
  
                set  
                {  
                    //basic validation  
                    if (value < 0)  
                        throw new ArgumentException("Count cannot be less than 0");  
  
                    _count = value;  
                }  
            }  
             #endregion  
    ```  
  
最终的代码列表如下所示：  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace ColumnCountCondition  
{  
  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
    public class ResultSetColumnCountCondition : TestCondition  
    {  
        private int _resultSet;  
        private int _count;  
        private int _batch;  
  
        public ResultSetColumnCountCondition()  
        {  
            _resultSet = 1;  
            _count = 0;  
            _batch = 1;  
        }  
  
        //method you need to override  
        //to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            //call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            //verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            //verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            //actual condition verification  
            //verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        //this method is called to provide the string shown in the  
        //test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        //below are the test condition properties  
        //that are exposed to the user in the property browser  
        #region Properties  
  
        //property specifying the resultset for which  
        //you want to check the column count  
        [Category("Test Condition")]  
        [DisplayName("ResultSet")]  
        [Description("ResultSet Number")]  
        public int ResultSet  
        {  
            get { return _resultSet; }  
  
            set  
            {  
                //basic validation  
                if (value < 1)  
                    throw new ArgumentException("ResultSet cannot be less than 1");  
  
                _resultSet = value;  
            }  
        }  
  
        //property specifying  
        //expected column count  
        [Category("Test Condition")]  
        [DisplayName("Count")]  
        [Description("Column Count")]  
        public int Count  
        {  
            get { return _count; }  
  
            set  
            {  
                //basic validation  
                if (value < 0)  
                    throw new ArgumentException("Count cannot be less than 0");  
  
                _count = value;  
            }  
        }  
  
        #endregion  
    }  
}  
  
```  
  
接下来，我们将生成该项目。  
  
## <a name="compiling-the-project-and-installing-your-test-condition"></a><a name="xxx"></a>编译项目并且安装测试条件  
在“生成”菜单中，单击“生成解决方案”。  
  
接下来，您要将程序集信息复制到 Extensions 目录中。 在 Visual Studio 启动后，它会标识 %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions 目录和子目录中的任何扩展，并且使它们可供使用：  
  
将 ColumnCountCondition.dll 程序集文件从输出目录复制到 %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions 目录中。  
  
默认情况下，已编译的 .dll 文件的路径为 YourSolutionPath\\YourProjectPath\bin\Debug 或 YourSolutionPath\\YourProjectPath\bin\Release。  
  
接下来，你将启动 Visual Studio 的一个新会话并且创建一个数据库项目。 启动新 Visual Studio 会话并且创建数据库项目：  
  
1.  启动 Visual Studio 的第二个会话。  
  
2.  在“文件”菜单上，单击“新建”，然后单击“项目”。  
  
3.  在“新建项目”对话框的已安装模板列表中，选择 SQL Server 节点。  
  
4.  在详细信息窗格中，单击“SQL Server 数据库项目”。  
  
5.  在“名称”文本框中，键入 SampleConditionDB，然后单击“确定”。  
  
接下来，我们需要创建一个单元测试。 在新的测试类中创建 SQL Server 单元测试：  
  
1.  在“测试” 菜单中，单击“新建测试”以便显示“添加新测试”对话框。  
  
    也可以打开“解决方案资源管理器”，右键单击某个测试项目，指向“添加”，然后单击“新建测试”。  
  
2.  在模板列表中，单击“SQL Server 单元测试”。  
  
3.  在“测试名称”中，键入 SampleUnitTest。  
  
4.  在“添加到测试项目”中，单击“创建新的 Visual C\# 测试项目”。 然后，单击“确定”以便显示“新建测试项目”对话框。  
  
5.  为项目名称键入 SampleUnitTest。  
  
6.  单击“取消”以创建单元测试而不配置要使用数据库连接的测试项目。 空白测试将出现在 SQL Server 单元测试设计器中。 将 Visual C\# 源代码文件添加到该测试项目中。  
  
    有关创建和配置具有数据库连接的数据库单元测试的更多信息，请参见[如何：创建空的 SQL Server 单元测试](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)。  
  
7.  单击“单击此处以创建”以完成单元测试的创建。 你将看到在 SQL Server 项目中显示的新的测试条件。  
  
> [!NOTE]  
> 若要将自定义测试条件用于现有单元测试项目，必须创建至少一个新的 SQL Server 单元测试类。 在测试类创建过程中将所需对您的测试条件程序集的引用添加到您的测试项目上。  
  
查看新的测试条件：  
  
1.  在“SQL Server 单元测试设计器”中的“测试条件”下，在“名称”列下单击 inconclusiveCondition1 测试。  
  
2.  单击“删除测试条件”工具栏按钮以便删除 inconclusiveCondition1 测试。  
  
3.  单击“测试条件”下拉列表，然后选择“ResultSet 列计数”。  
  
4.  单击“添加测试条件”工具栏按钮以便添加自定义测试条件。  
  
5.  在“属性”窗口中，配置 Count、Enabled 和 ResultSet 属性。  
  
    有关详细信息，请参阅[操作说明：向 SQL Server 单元测试添加测试条件](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md)。  
  
## <a name="see-also"></a>另请参阅  
[SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
