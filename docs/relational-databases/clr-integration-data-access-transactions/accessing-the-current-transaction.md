---
title: 访问当前事务 |Microsoft Docs
description: 在 SQL Server CLR 集成中，System.web 类的 Current 属性允许您访问当前事务。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- current transaction access
- Current property
- Transaction class
ms.assetid: 1a4e2ce5-f627-4c81-8960-6a9968cefda2
author: rothja
ms.author: jroth
ms.openlocfilehash: b9220519a926ca75a00843dba54dfad797064aee
ms.sourcegitcommit: 52252e8b4c9b50d3915aecd3135e8901d345e7e2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97599844"
---
# <a name="accessing-the-current-transaction"></a>访问当前事务
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  如果事务处于活动状态，则在公共语言运行时 (CLR) 上运行的编写代码时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，将通过 **system.web** 类公开该事务。 **Transaction。 current** 属性用于访问当前事务。 在大多数情况下，不必显式访问事务。 对于数据库连接，ADO.NET 会在调用 **Open** 方法时自动检查 **transaction** ，并以透明方式在该事务中登记连接 (除非) 的连接字符串中将 **征用** 关键字设置为 false。  
  
 您可能需要在以下情况下直接使用 **Transaction** 对象：  
  
-   如果您要登记未自动登记的资源或因为某个原因而未在初始化期间登记的资源。  
  
-   如果您要显式登记事务中的资源。  
  
-   如果您要从存储过程或函数的内部终止外部事务。 在这种情况下，使用 <xref:System.Transactions.TransactionScope>。 例如，以下代码将回滚当前事务：  
  
    ```  
    using(TransactionScope transactionScope = new TransactionScope(TransactionScopeOptions.Required)) { }  
    ```  
  
 本主题的其余内容介绍取消外部事务的其他方式。  
  
## <a name="canceling-an-external-transaction"></a>取消外部事务  
 可以用以下方式从托管过程或函数取消外部事务：  
  
-   通过使用输出参数，托管过程或函数可以返回值。 调用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程可以检查返回值，并在适当的情况下执行 **回滚事务**。  
  
-   托管过程或函数可以引发自定义异常。 调用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程可以捕获由托管过程或函数在 try/catch 块中引发的异常，并执行 **回滚事务**。  
  
-   如果满足特定条件，托管过程或函数可以通过调用 **transaction** 方法来取消当前事务。  
  
 当在托管过程或函数中调用该函数时， **Transaction** 方法会引发异常，并在 try/catch 块中包装。 错误消息类似于以下内容：  
  
```  
Msg 3994, Level 16, State 1, Procedure uspRollbackFromProc, Line 0  
Transaction is not allowed to roll back inside a user defined routine, trigger or aggregate because the transaction is not started in that CLR level. Change application logic to enforce strict transaction nesting.  
```  
  
 应出现该异常，而且需要 try/catch 块才能继续执行代码。 如果没有 try/catch 块，则立即对调用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程引发异常，并将完成托管代码的执行。 执行完托管代码时，将引发另一个异常：  
  
```  
Msg 3991, Level 16, State 1, Procedure uspRollbackFromProc, Line 1   
The context transaction which was active before entering user defined routine, trigger or aggregate " uspRollbackFromProc " has been ended inside of it, which is not allowed. Change application logic to enforce strict transaction nesting. The statement has been terminated.  
```  
  
 也应出现该异常，并且为了让执行得以继续，在执行激发触发器的操作的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句周围必须有 try/catch 块。 尽管引发了两个异常，但事务将回滚，并且不提交更改。  
  
### <a name="example"></a>示例  
 下面是使用 **事务 Rollback** 方法从托管过程回滚事务的示例。 请注意事务前后的 try/catch 块 **。** 此 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本创建了一个程序集和托管存储过程。 请注意， **EXEC uspRollbackFromProc** 语句包装在 try/catch 块中，以便捕获托管过程完成执行时引发的异常。  
  
```csharp  
using System;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.SqlTypes;  
using Microsoft.SqlServer.Server;  
using System.Transactions;  
  
public partial class StoredProcedures  
{  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void uspRollbackFromProc()  
{  
   using (SqlConnection connection = new SqlConnection(@"context connection=true"))  
   {  
      // Open the connection.  
      connection.Open();  
  
      bool successCondition = true;  
  
      // Success condition is met.  
      if (successCondition)  
      {  
         SqlContext.Pipe.Send("Success condition met in procedure.");   
         // Perform other actions here.  
      }  
  
      //  Success condition is not met, the transaction will be rolled back.  
      else  
      {  
         SqlContext.Pipe.Send("Success condition not met in managed procedure. Transaction rolling back...");  
         try  
         {  
               // Get the current transaction and roll it back.  
               Transaction trans = Transaction.Current;  
               trans.Rollback();  
         }  
         catch (SqlException ex)  
         {  
            // Catch the expected exception.   
            // This allows the connection to close correctly.                      
         }    
      }  
  
      // Close the connection.  
      connection.Close();  
   }  
}  
};  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Data.SqlClient  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
Imports System.Transactions  
  
Partial Public Class StoredProcedures  
<Microsoft.SqlServer.Server.SqlProcedure()> _  
Public Shared Sub  uspRollbackFromProc ()  
   Using connection As New SqlConnection("context connection=true")  
  
   ' Open the connection.  
   connection.Open()  
  
   Dim successCondition As Boolean  
   successCondition = False  
  
   ' Success condition is met.  
   If successCondition Then  
  
      SqlContext.Pipe.Send("Success condition met in procedure.")  
  
      ' Success condition is not met, the transaction will be rolled back.  
  
   Else  
      SqlContext.Pipe.Send("Success condition not met in managed procedure. Transaction rolling back...")  
      Try  
         ' Get the current transaction and roll it back.  
         Dim trans As Transaction  
         trans = Transaction.Current  
         trans.Rollback()  
  
      Catch ex As SqlException  
         ' Catch the exception instead of throwing it.    
         ' This allows the connection to close correctly.                      
      End Try  
  
   End If  
  
   ' Close the connection.  
   connection.Close()  
  
End Using  
End Sub  
End Class  
```  
  
 **Transact-SQL**  
  
```  
--Register assembly.  
CREATE ASSEMBLY TestProcs FROM 'C:\Programming\TestProcs.dll'   
Go  
CREATE PROCEDURE uspRollbackFromProc AS EXTERNAL NAME TestProcs.StoredProcedures.uspRollbackFromProc  
Go  
  
-- Execute procedure.  
BEGIN TRY  
BEGIN TRANSACTION   
-- Perform other actions.  
Exec uspRollbackFromProc  
-- Perform other actions.  
PRINT N'Commiting transaction...'  
COMMIT TRANSACTION  
END TRY  
  
BEGIN CATCH  
SELECT ERROR_NUMBER() AS ErrorNum, ERROR_MESSAGE() AS ErrorMessage  
PRINT N'Exception thrown, rolling back transaction.'  
ROLLBACK TRANSACTION  
PRINT N'Transaction rolled back.'   
END CATCH  
Go  
  
-- Clean up.  
DROP Procedure uspRollbackFromProc;  
Go  
DROP ASSEMBLY TestProcs;  
Go  
```  
  
## <a name="see-also"></a>另请参阅  
 [CLR 集成和事务](../../relational-databases/clr-integration-data-access-transactions/clr-integration-and-transactions.md)  
  
  
