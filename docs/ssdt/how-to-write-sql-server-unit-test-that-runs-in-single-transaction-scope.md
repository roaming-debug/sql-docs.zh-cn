---
title: 编写在单个事务范围内运行的 SQL Server 单元测试
description: 了解如何启动分布式事务处理协调器服务，编写单事务 SQL Server 单元测试，以及回滚测试更改。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cb241e94-d81c-40e9-a7ae-127762a6b855
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6c03450778defd73b21c19d72aa1e0acc6af342b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017937"
---
# <a name="how-to-write-a-sql-server-unit-test-that-runs-within-the-scope-of-a-single-transaction"></a>如何：编写在单个事务范围内运行的 SQL Server 单元测试

您可以修改单元测试以便在单个事务的范围内运行。 如果您采用此方法，则可以在测试结束后回滚测试所执行的所有更改。 下面的过程介绍了如何执行以下操作：  
  
-   在 Transact\-SQL 测试脚本中创建一个使用 BEGIN TRANSACTION 和 ROLLBACK TRANSACTION 的事务。  
  
-   为测试类中的单个测试方法创建事务。  
  
-   为给定的测试类中的所有测试方法创建事务。  
  
**先决条件**  
  
对于本主题中的某些过程，必须在运行单元测试的计算机上运行分布式事务处理协调器服务。 有关详细信息，请参见本主题末尾的过程。  
  
## <a name="to-create-a-transaction-using-transact-sql"></a>使用 Transact\-SQL 创建事务  
  
#### <a name="to-create-a-transaction-using-transact-sql"></a>使用 Transact\-SQL 创建事务  
  
1.  在“SQL Server 单元测试设计器”中打开单元测试。 （双击单元测试的源代码文件可显示设计器。）  
  
2.  指定要为其创建事务的脚本的类型。 例如，可以指定预先测试、测试或后期测试。  
  
3.  在 Transact\-SQL 编辑器中输入测试脚本。  
  
4.  插入 `BEGIN TRANSACTION` 和 `ROLLBACK TRANSACTION` 语句，如下面的简单示例中所示。 该示例使用一个包含 50 行数据的名为 OrderDetails 的数据库表：  
  
    ```  
    BEGIN TRANSACTION TestTransaction  
    UPDATE "OrderDetails" set Quantity = Quantity + 10  
    IF @@ROWCOUNT!=50  
    RAISERROR('Row count does not equal 50',16,1)  
    ROLLBACK TRANSACTION TestTransaction  
    ```  
  
    > [!NOTE]  
    > 在执行 COMMIT TRANSACTION 语句后不能回滚事务。  
  
    有关 ROLLBACK TRANSACTION 如何与存储过程和触发器一起使用的详细信息，请参见 Microsoft 网站上的网页：[ROLLBACK TRANSACTION (Transact-SQL)](../t-sql/language-elements/rollback-transaction-transact-sql.md)。  
  
## <a name="to-create-a-transaction-for-a-single-test-method"></a>为单个测试方法创建事务  
在本示例中，当使用 [System.Transactions.TransactionScope](/dotnet/api/system.transactions.transactionscope) 类型时，你将使用环境事务。 默认情况下，Execution 和 Privileged 连接将不使用环境事务，因为这些连接是在执行方法之前创建的。 SqlConnection 具有一个将活动连接与事务关联的 [System.Data.SqlClient.SqlConnection.EnlistTransaction](/dotnet/api/system.data.sqlclient.sqlconnection.enlisttransaction) 方法。 创建某个环境事务时，该事务会将自身注册为当前事务，并且你可以通过 [System.Transactions.Transaction.Current](/dotnet/api/system.transactions.transaction.current) 属性访问它。 在此示例中，当释放环境事务时将会回滚该事务。 如果想要提交在运行单元测试时所做的任何更改，则必须调用 [System.Transactions.TransactionScope.Complete](/dotnet/api/system.transactions.transactionscope.complete) 方法。  
  
#### <a name="to-create-a-transaction-for-a-single-test-method"></a>为单个测试方法创建事务  
  
1.  在“解决方案资源管理器”中，右键单击测试项目中的“引用”节点，然后单击“添加引用”。  
  
    此时将显示“添加引用”对话框。  
  
2.  单击 .NET 选项卡。  
  
3.  在程序集列表中，单击“System.Transactions”，然后单击“确定”。  
  
4.  打开你的单元测试的 Visual Basic 或 C# 文件。  
  
5.  包装预先测试、测试和后期测试操作，如下面的 Visual Basic 代码示例中所示：  
  
    ```  
    <TestMethod()> _  
    Public Sub dbo_InsertTable1Test()  
  
        Using ts as New System.Transactions.TransactionScope( System.Transactions.TransactionScopeOption.Required)  
            ExecutionContext.Connection.EnlistTransaction(Transaction.Current)  
            PrivilegedContext.Connection.EnlistTransaction(Transaction.Current)  
  
            Dim testActions As DatabaseTestActions = Me.dbo_InsertTable1TestData  
            'Execute the pre-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PretestAction) Is Nothing), "Executing pre-test script...")  
            Dim pretestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PretestAction)  
            'Execute the test script  
  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.TestAction) Is Nothing), "Executing test script...")  
            Dim testResults() As ExecutionResult = TestService.Execute(ExecutionContext, Me.PrivilegedContext, testActions.TestAction)  
  
            'Execute the post-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PosttestAction) Is Nothing), "Executing post-test script...")  
            Dim posttestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PosttestAction)  
  
            'Because the transaction is not explicitly committed, it  
            'is rolled back when the ambient transaction is   
            'disposed.  
            'To commit the transaction, remove the comment delimiter  
            'from the following statement:  
            'ts.Complete()  
  
    End Sub  
    Private dbo_InsertTable1TestData As DatabaseTestActions  
    ```  
  
    > [!NOTE]  
    > 如果你正在使用 Visual Basic，则必须添加 `Imports System.Transactions`（除了 `Imports Microsoft.VisualStudio.TestTools.UnitTesting`、`Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTesting` 和 `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTest.Conditions` 之外）。如果你正在使用 Visual C#，则必须添加 `using System.Transactions`（除了用于 Microsoft.VisualStudio.TestTools、Microsoft.VisualStudio.TeamSystem.Data.UnitTesting 和 Microsoft.VisualStudio.TeamSystem.Data.UnitTesting.Conditions 的 `using` 语句之外）。 您还必须在这些程序集中添加对您的项目的引用。  
  
## <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>为测试类中的所有测试方法创建事务  
  
#### <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>为测试类中的所有测试方法创建事务  
  
1.  打开你的单元测试的 Visual Basic 或 C# 文件。  
  
2.  在 TestInitialize 中创建事务，并在 TestCleanup 中释放它，如下面的 Visual C# 代码示例中所示：  
  
    ```  
    TransactionScope _trans;  
  
            [TestInitialize()]  
            public void Init()  
            {  
                _trans = new TransactionScope();  
                base.InitializeTest();  
            }  
  
            [TestCleanup()]  
            public void Cleanup()  
            {  
                base.CleanupTest();  
                _trans.Dispose();  
            }  
  
            [TestMethod()]  
            public void TransactedTest()  
            {  
                DatabaseTestActions testActions = this.DatabaseTestMethod1Data;  
                // Execute the pre-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PretestAction != null), "Executing pre-test script...");  
                ExecutionResult[] pretestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PretestAction);  
                // Execute the test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.TestAction != null), "Executing test script...");  
                ExecutionResult[] testResults = TestService.Execute(this.ExecutionContext, this.PrivilegedContext, testActions.TestAction);  
                // Execute the post-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PosttestAction != null), "Executing post-test script...");  
                ExecutionResult[] posttestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PosttestAction);  
  
            }  
    ```  
  
## <a name="to-start-the-distributed-transaction-coordinator-service"></a>启动分布式事务处理协调器服务  
本主题中的一些过程将会使用 System.Transactions 程序集中的类型。 在按照这些过程进行操作之前，您必须确保在运行单元测试的计算机上正在运行分布式事务处理协调器服务。 否则，测试将失败并显示以下错误消息：“测试方法 ProjectName.TestName.MethodName 引发异常: System.Data.SqlClient.SqlException: 服务器‘ComputerName’上的 MSDTC 不可用”。  
  
#### <a name="to-start-the-distributed-transaction-coordinator-service"></a>启动分布式事务处理协调器服务  
  
1.  打开“控制面板”  
  
2.  在“控制面板”中，打开“管理工具”。  
  
3.  在“管理工具”中，打开“服务”。  
  
4.  在“服务”窗格中，右键单击“分布式事务处理控制器”服务，然后单击“启动”。  
  
    该服务的状态应更新为“已启动”。 现在，您应该能够运行使用 System.Transactions 的单元测试。  
  
> [!IMPORTANT]  
> 即使您已启动分布式事务处理控制器服务，也可能会出现下面的错误：`System.Transactions.TransactionManagerCommunicationException: Network access for Distributed Transaction Manager (MSDTC) has been disabled. Please enable DTC for network access in the security configuration for MSDTC using the Component Services Administrative tool. ---> System.Runtime.InteropServices.COMException: The transaction manager has disabled its support for remote/network transactions. (Exception from HRESULT: 0x8004D024)`。 如果出现此错误，则您必须对分布式事务处理控制器服务进行网络访问方面的配置。 有关详细信息，请参见[启用网络 DTC 访问](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753510(v=ws.10))。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
