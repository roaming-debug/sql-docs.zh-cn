---
title: 为 SQL Server 单元测试设计器创建测试条件
description: 请参阅如何扩展 TestCondition 类，为 SQL Server 单元测试设计器创建自定义测试条件。 查看自定义测试条件的示例。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 48076062-1ef5-419a-8a55-3c7b4234cc35
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d74fe60ff94d2893dc5a2dd899643cabfd0a4f30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018277"
---
# <a name="how-to-create-test-conditions-for-the-sql-server-unit-test-designer"></a>如何：为 SQL Server 单元测试设计器创建测试条件

可以使用可扩展的 [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) 类来创建新的测试条件。 例如，您可以创建一个新的测试条件，验证结果集中列或值的数目。  
  
## <a name="to-create-a-test-condition"></a>创建测试条件  
此过程说明如何创建要在 SQL Server 单元测试设计器中出现的测试条件。  
  
1.  在 Visual Studio 中，创建一个类库项目。  
  
2.  在“项目”菜单上，单击“添加引用”。  
  
3.  单击 .NET 选项卡。  
  
4.  在“组件名称”列表中，选择“System.ComponentModel.Composition”，然后单击“确定”。  
  
5.  添加所需的程序集引用。 右键单击项目节点，然后单击“添加引用”。 单击“浏览”，并导航到 C:\Program Files (x86)\\MicrosoftSQL Server\110\DAC\Bin 文件夹。 选择 Microsoft.Data.Tools.Schema.Sql.dll 并单击“添加”，然后单击“确定”。  
  
6.  在“项目”菜单上，单击“卸载项目”。  
  
7.  在“解决方案资源管理器”中右键单击项目，然后选择“编辑 <project name>.csproj”。  
  
8.  在导入 Microsoft.CSharp.targets 后添加以下 Import 语句：  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. 保存文件并将其关闭。 在“解决方案资源管理器”中右键单击该项目，然后选择“重新加载项目”。  
  
10. 从 [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) 类派生你的类。  
  
11. 用强名称为程序集签名。 有关详细信息，请参阅[操作说明：使用强名称为程序集签名](/dotnet/standard/assembly/sign-strong-name)。  
  
12. 生成类库。  
  
13. 必须首先将已签名的程序集复制到 %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions 文件夹，然后才能使用这个新的测试条件。 如果该文件夹不存在，则创建它。 为了复制到此目录，您需要针对您的计算机的管理权限。  
  
14. 安装测试条件。 有关详细信息，请参阅 [SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)。  
  
15. 将一个新的 SQL Server 单元测试添加到该项目，以便创建对要添加到该项目的测试条件的引用。 你可以手动添加对项目中的测试条件程序集的引用。 完成此步骤后，重新加载设计器。  
  
    > [!NOTE]  
    > 为创建该引用，必须添加一个测试类。 您可以在添加引用后删除该测试类。  
  
在下面的示例中，您创建一个简单的测试条件，该条件验证在结果集中返回的列数。 您可以使用这个简单的测试条件确保某一存储过程的协定是正确的。  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace Ssdt.Samples.SqlUnitTesting  
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
  
        // method you need to override  
        // to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            // call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            // verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            // verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            // actual condition verification  
            // verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        // this method is called to provide the string shown in the  
        // test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        // below are the test condition properties  
        // that are exposed to the user in the property browser  
        #region Properties  
  
        // property specifying the resultset for which  
        // you want to check the column count  
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
  
        // property specifying  
        // expected column count  
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
  
该自定义测试条件的类是从基 [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) 类继承的。 由于自定义测试条件上的其他属性，用户可以在安装了测试条件后从“属性”窗口配置该条件。  
  
[ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)) 必须添加到扩展 [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) 的类中。 此特性使该类能够被 SQL Server Data Tools 发现并且在单元测试设计和执行过程中使用。 该特性有两个参数：  
  
|特性参数|位置|说明|  
|-----------------------|------------|---------------|  
|DisplayName|1|在“测试条件”组合框中标识字符串。 此名称必须唯一。 如果两个条件具有相同的显示名称，将向用户显示找到的第一个条件，并且在 Visual Studio 错误管理器将显示警告。|  
|ImplementingType|2|该参数用于唯一标识扩展。 您需要更改该参数以便匹配您要放置特性的类型。 本示例使用 ResultSetColumnCountCondition 类型，因此请使用 typeof(ResultSetColumnCountCondition)。 如果你的类型是 NewTestCondition，请使用 typeof(NewTestCondition)。|  
  
在这个示例中，您添加两个属性。 自定义测试条件的用户可以使用 ResultSet 属性指定应验证其列计数的结果集。 然后，用户可以使用 Count 属性指定期望的列计数。  
  
为每个属性都添加三个特性：  
  
-   帮助对属性进行组织的类别名称。  
  
-   属性的显示名称。  
  
-   属性的说明。  
  
对属性执行验证，以便验证 ResultSet 属性的值小于 1 并且 Count 属性的值大于 0。  
  
Assert 方法执行测试条件的主要任务。 您重写 Assert 方法以便验证是否满足期望的条件。 此方法提供两个参数：  
  
-   第一个参数的用于验证测试条件的数据库连接。  
  
-   第二个且更为重要的参数是结果数组，它为已执行的每个批处理都返回单个数组元素。  
  
每个测试脚本仅支持一个批处理。 因此，测试条件将始终检查第一个数组元素。 该数组元素包含一个数据集，而该数据集又包含为测试脚本返回的结果集。 在这个示例中，代码验证结果集中的数据表包含适当的列数。 有关更多信息，请参见“数据集”。  
  
您必须设置包含要签名的测试条件的类库，您可以在“签名”选项卡上的项目属性中执行此设置。  
  
## <a name="see-also"></a>另请参阅  
[SQL Server 单元测试的自定义测试条件](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
