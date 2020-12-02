---
description: 使用脚本组件创建 ODBC 目标
title: 使用脚本组件创建 ODBC 目标 | Microsoft Docs
ms.custom: ''
ms.date: 10/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], destination components
- ODBC destination [Integration Services]
- destinations [Integration Services], components
- Script component [Integration Services], examples
ms.assetid: d198c866-78f4-4a50-ae15-333160645815
author: chugugrace
ms.author: chugu
ms.openlocfilehash: dd25e6cf4816b0ff39ed1129a1d98f9de5603d2c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123047"
---
# <a name="creating-an-odbc-destination-with-the-script-component"></a>使用脚本组件创建 ODBC 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中，通常使用 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 目标和用于 ODBC 的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序将数据保存到 ODBC 目标。 当然，还可以创建供单个包使用的即席 ODBC 目标。 若要创建此即席 ODBC 目标，可使用脚本组件，如下面的示例中所示。  
  
> [!NOTE]  
>  如果希望创建可更方便地重用于多个数据流任务和多个包的组件，请考虑以此脚本组件示例中的代码为基础，创建自定义数据流组件。 有关详细信息，请参阅 [开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)。  
  
## <a name="example"></a>示例  
 下面的示例演示如何创建一个目标组件，该组件使用现有 ODBC 连接管理器将数据流中的数据保存到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
 本示例是[使用脚本组件创建目标](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)主题中演示的自定义 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 目标的修改版本。 但是，在此示例中，自定义 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 目标已修改为使用 ODBC 连接管理器并将数据保存到 ODBC 目标。 此外，还包括以下更改：  
  
-   不能从托管代码调用 ODBC 连接管理器的 **AcquireConnection** 方法，因为它返回的是一个本机对象。 因此，本示例使用连接管理器的连接字符串通过托管 ODBC [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口直接连接到数据源。  
  
-   **OdbcCommand** 需要位置参数。 在命令的文本中，参数的位置由问号 (?) 指示。 （相反，**SqlCommand** 需要命名参数。）  
  
 本示例使用 **AdventureWorks** 示例数据库中的 **Person.Address** 表。 此示例通过数据流传递此表的第一列和第四列：int AddressID 和 nvarchar(30) City 列 ********。 [开发特定类型的脚本组件](../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md)主题中的源、转换和目标示例使用了相同的数据。  
  
#### <a name="to-configure-this-script-component-example"></a>配置此脚本组件示例  
  
1.  创建连接 **AdventureWorks** 数据库的 ODBC 连接管理器。  
  
2.  通过在 **AdventureWorks** 数据库中运行以下 Transact-SQL 命令来创建一个目标表：  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
3.  向数据流设计器图面添加新的脚本组件并将其配置为目标。  
  
4.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中将上游源或转换的输出连接到目标组件。 （无需任何转换，即可将源直接连接到目标。）为了确保本示例能够正常工作，上游组件的输出必须至少包含 **AdventureWorks** 示例数据库的 **Person.Address** 表中的 **AddressID** 和 **City** 列。  
  
5.  打开“脚本转换编辑器”  。 在“输入列”  页中，选择 AddressID  和 City  列。  
  
6.  在“输入和输出”页中，用更具说明性的名称（如 **MyAddressInput**）重命名输入。  
  
7.  在“连接管理器”页中，添加或创建一个具有说明性名称（如 **MyODBCConnectionManager**）的 ODBC 连接管理器。  
  
8.  在“脚本”页中，单击“编辑脚本”，并在 **ScriptMain** 类中输入下面所示的脚本。  
  
9. 关闭脚本开发环境和“脚本转换编辑器”，然后运行该示例。  
  
    ```vb  
    Imports System.Data.Odbc  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim odbcConn As OdbcConnection  
        Dim odbcCmd As OdbcCommand  
        Dim odbcParam As OdbcParameter  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connectionString As String  
            connectionString = Me.Connections.MyODBCConnectionManager.ConnectionString  
            odbcConn = New OdbcConnection(connectionString)  
            odbcConn.Open()  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            odbcCmd = New OdbcCommand("INSERT INTO Person.Address2(AddressID, City) " & _  
                "VALUES(?, ?)", odbcConn)  
            odbcParam = New OdbcParameter("@addressid", OdbcType.Int)  
            odbcCmd.Parameters.Add(odbcParam)  
            odbcParam = New OdbcParameter("@city", OdbcType.NVarChar, 30)  
            odbcCmd.Parameters.Add(odbcParam)  
  
        End Sub  
  
        Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
            With odbcCmd  
                .Parameters("@addressid").Value = Row.AddressID  
                .Parameters("@city").Value = Row.City  
                .ExecuteNonQuery()  
            End With  
  
        End Sub  
  
        Public Overrides Sub ReleaseConnections()  
  
            odbcConn.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.Data.Odbc;  
    ...  
    public class ScriptMain :  
        UserComponent  
    {  
        OdbcConnection odbcConn;  
        OdbcCommand odbcCmd;  
        OdbcParameter odbcParam;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            string connectionString;  
            connectionString = this.Connections.MyODBCConnectionManager.ConnectionString;  
            odbcConn = new OdbcConnection(connectionString);  
            odbcConn.Open();  
  
        }  
  
        public override void PreExecute()  
        {  
  
            odbcCmd = new OdbcCommand("INSERT INTO Person.Address2(AddressID, City) " +  
                "VALUES(?, ?)", odbcConn);  
            odbcParam = new OdbcParameter("@addressid", OdbcType.Int);  
            odbcCmd.Parameters.Add(odbcParam);  
            odbcParam = new OdbcParameter("@city", OdbcType.NVarChar, 30);  
            odbcCmd.Parameters.Add(odbcParam);  
  
        }  
  
        public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
        {  
  
            {  
                odbcCmd.Parameters["@addressid"].Value = Row.AddressID;  
                odbcCmd.Parameters["@city"].Value = Row.City;  
                odbcCmd.ExecuteNonQuery();  
            }  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            odbcConn.Close();  
  
        }  
    }  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [使用脚本组件创建目标](../extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)  
  
  
