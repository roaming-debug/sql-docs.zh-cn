---
description: 在 SQL Server 代理中计划自动管理任务
title: 在 SQL Server 代理中计划自动管理任务
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- scheduling administrative tasks [SMO]
- SQL Server Agent [SMO]
- automatic administrative SMO tasks
ms.assetid: 900242ad-d6a2-48e9-8a1b-f0eea4413c16
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b777b9f7343a0014d01da34b7681ef382e4821d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475448"
---
# <a name="scheduling-automatic-administrative-tasks-in-sql-server-agent"></a>在 SQL Server 代理中计划自动管理任务
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在 SMO 中，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理由下列对象表示：  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Agent.JobServer> 对象具有作业、警报和操作员三个集合。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Agent.OperatorCollection> 对象表示可由 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理自动向其发送事件通知的寻呼程序、电子邮件地址和 net send 操作员列表。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Agent.AlertCollection> 对象表示由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 监视的系统事件或性能条件等情况的列表。  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Agent.JobCollection> 对象稍微复杂一些。 它表示按指定计划运行的多步骤任务列表。 步骤和计划信息存储在 <xref:Microsoft.SqlServer.Management.Smo.Agent.JobStep> 和 <xref:Microsoft.SqlServer.Management.Smo.Agent.JobSchedule> 对象中。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理对象位于 <xref:Microsoft.SqlServer.Management.Smo.Agent> 命名空间中。  
  
## <a name="examples"></a>示例  
 若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
对于使用代理的程序 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，必须包括 **using** 语句以限定代理命名空间。 在应用程序中的任何声明之前，将语句插入到其他 **using** 语句之后，例如：
  
 ```
using Microsoft.SqlServer.Management.Smo;
  
using Microsoft.SqlServer.Management.Common;
  
using Imports Microsoft.SqlServer.Management.Smo.Agent;
  ```
  
## <a name="creating-a-job-with-steps-and-a-schedule-in-visual-c"></a>在 Visual C# 中创建包含步骤和计划的作业  
 此代码示例将创建包含步骤和计划的作业，然后通知操作员。  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.  
            Server srv = new Server();  
  
            //Define an Operator object variable by supplying the Agent (parent JobServer object) and the name in the constructor.   
            Operator op = new Operator(srv.JobServer, "Test_Operator");  
  
            //Set the Net send address.   
            op.NetSendAddress = "Network1_PC";  
  
            //Create the operator on the instance of SQL Server Agent.   
            op.Create();  
  
            //Define a Job object variable by supplying the Agent and the name arguments in the constructor and setting properties.   
            Job jb = new Job(srv.JobServer, "Test_Job");  
  
            //Specify which operator to inform and the completion action.   
            jb.OperatorToNetSend = "Test_Operator";  
            jb.NetSendLevel = CompletionAction.Always;  
  
            //Create the job on the instance of SQL Server Agent.   
            jb.Create();  
  
            //Define a JobStep object variable by supplying the parent job and name arguments in the constructor.   
            JobStep jbstp = new JobStep(jb, "Test_Job_Step");  
            jbstp.Command = "Test_StoredProc";  
            jbstp.OnSuccessAction = StepCompletionAction.QuitWithSuccess;  
            jbstp.OnFailAction = StepCompletionAction.QuitWithFailure;  
  
            //Create the job step on the instance of SQL Agent.   
            jbstp.Create();  
  
            //Define a JobSchedule object variable by supplying the parent job and name arguments in the constructor.   
  
            JobSchedule jbsch = new JobSchedule(jb, "Test_Job_Schedule");  
  
            //Set properties to define the schedule frequency, and duration.   
            jbsch.FrequencyTypes = FrequencyTypes.Daily;  
            jbsch.FrequencySubDayTypes = FrequencySubDayTypes.Minute;  
            jbsch.FrequencySubDayInterval = 30;  
            TimeSpan ts1 = new TimeSpan(9, 0, 0);  
            jbsch.ActiveStartTimeOfDay = ts1;  
  
            TimeSpan ts2 = new TimeSpan(17, 0, 0);  
            jbsch.ActiveEndTimeOfDay = ts2;  
            jbsch.FrequencyInterval = 1;  
  
            System.DateTime d = new System.DateTime(2003, 1, 1);  
            jbsch.ActiveStartDate = d;  
  
            //Create the job schedule on the instance of SQL Agent.   
            jbsch.Create();  
        }  
```  
  
## <a name="creating-a-job-with-steps-and-a-schedule-in-powershell"></a>在 PowerShell 中创建包含步骤和计划的作业  
 此代码示例将创建包含步骤和计划的作业，然后通知操作员。  
  
```powershell  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Define an Operator object variable by supplying the Agent (parent JobServer object) and the name in the constructor.  
$op = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Agent.Operator -argumentlist $srv.JobServer, "Test_Operator"  
  
#Set the Net send address.  
$op.NetSendAddress = "Network1_PC"  
  
#Create the operator on the instance of SQL Agent.  
$op.Create()  
  
#Define a Job object variable by supplying the Agent and the name arguments in the constructor and setting properties.   
$jb = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Agent.Job -argumentlist $srv.JobServer, "Test_Job"   
  
#Specify which operator to inform and the completion action.   
$jb.OperatorToNetSend = "Test_Operator";   
$jb.NetSendLevel = [Microsoft.SqlServer.Management.SMO.Agent.CompletionAction]::Always  
  
#Create the job on the instance of SQL Server Agent.   
$jb.Create()  
  
#Define a JobStep object variable by supplying the parent job and name arguments in the constructor.   
$jbstp = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Agent.JobStep -argumentlist $jb, "Test_Job_Step"   
$jbstp.Command = "Test_StoredProc";   
$jbstp.OnSuccessAction = [Microsoft.SqlServer.Management.SMO.Agent.StepCompletionAction]::QuitWithSuccess;   
$jbstp.OnFailAction =[Microsoft.SqlServer.Management.SMO.Agent.StepCompletionAction]::QuitWithFailure;   
  
#Create the job step on the instance of SQL Agent.   
$jbstp.Create();   
  
#Define a JobSchedule object variable by supplying the parent job and name arguments in the constructor.   
$jbsch =  New-Object -TypeName Microsoft.SqlServer.Management.SMO.Agent.JobSchedule -argumentlist $jb, "Test_Job_Schedule"   
  
#Set properties to define the schedule frequency, and duration.   
$jbsch.FrequencyTypes =  [Microsoft.SqlServer.Management.SMO.Agent.FrequencyTypes]::Daily  
  
$jbsch.FrequencySubDayTypes = [Microsoft.SqlServer.Management.SMO.Agent.FrequencySubDayTypes]::Minute  
$jbsch.FrequencySubDayInterval = 30  
$ts1 =  New-Object -TypeName TimeSpan -argumentlist 9, 0, 0  
$jbsch.ActiveStartTimeOfDay = $ts1  
$ts2 = New-Object -TypeName TimeSpan -argumentlist 17, 0, 0  
$jbsch.ActiveEndTimeOfDay = $ts2  
$jbsch.FrequencyInterval = 1  
$jbsch.ActiveStartDate = "01/01/2003"  
  
#Create the job schedule on the instance of SQL Agent.   
$jbsch.Create();  
```  
  
## <a name="creating-an-alert-in-visual-c"></a>在 Visual C# 中创建警报  
 此代码示例将创建由性能条件触发的警报。 该条件必须以特定格式提供：  
  
 **ObjectName |CounterName |实例 |ComparisionOp |CompValue**  
  
 对于警报通知，操作员是必需的。 <xref:Microsoft.SqlServer.Management.Smo.Agent.Operator>类型需要方括号，因为 **运算符** 是一个 [!INCLUDE[csprcs](../../../includes/csprcs-md.md)] 关键字。  
  
```csharp  
{  
             //Connect to the local, default instance of SQL Server.   
            Server srv = new Server();  
  
            //Define an Alert object variable by supplying the SQL Server Agent and the name arguments in the constructor.   
            Alert al = new Alert(srv.JobServer, "Test_Alert");  
  
            //Specify the performance condition string to define the alert.   
            al.PerformanceCondition = "SQLServer:General Statistics|User Connections||>|3";  
  
            //Create the alert on the SQL Agent.   
            al.Create();  
  
            //Define an Operator object variable by supplying the SQL Server Agent and the name arguments in the constructor.   
  
            Operator op = new Operator(srv.JobServer, "Test_Operator");  
            //Set the net send address.   
            op.NetSendAddress = "NetworkPC";  
            //Create the operator on the SQL Agent.   
            op.Create();  
            //Run the AddNotification method to specify the operator is notified when the alert is raised.   
            al.AddNotification("Test_Operator", NotifyMethods.NetSend);  
        }  
```  
  
## <a name="creating-an-alert-in-powershell"></a>在 PowerShell 中创建警报  
 此代码示例将创建由性能条件触发的警报。 该条件必须以特定格式提供：  
  
 **ObjectName |CounterName |实例 |ComparisionOp |CompValue**  
  
 对于警报通知，操作员是必需的。 <xref:Microsoft.SqlServer.Management.Smo.Agent.Operator>类型需要方括号，因为 **运算符** 是一个 [!INCLUDE[csprcs](../../../includes/csprcs-md.md)] 关键字。  
  
```powershell  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Define an Alert object variable by supplying the SQL Agent and the name arguments in the constructor.  
$al = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Agent.Alert -argumentlist $srv.JobServer, "Test_Alert"  
  
#Specify the performance condition string to define the alert.  
$al.PerformanceCondition = "SQLServer:General Statistics|User Connections||>|3"  
  
#Create the alert on the SQL Agent.  
$al.Create()  
  
#Define an Operator object variable by supplying the Agent (parent JobServer object) and the name in the constructor.  
$op = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Agent.Operator -argumentlist $srv.JobServer, "Test_Operator"  
  
#Set the Net send address.  
$op.NetSendAddress = "Network1_PC"  
  
#Create the operator on the instance of SQL Agent.  
$op.Create()  
  
#Run the AddNotification method to specify the operator is notified when the alert is raised.  
$ns = [Microsoft.SqlServer.Management.SMO.Agent.NotifyMethods]::NetSend  
$al.AddNotification("Test_Operator", $ns)  
  
#Drop the alert and the operator  
$al.Drop()  
$op.Drop()  
```
  
## <a name="allowing-user-access-to-subsystem-by-using-a-proxy-account-in-visual-c"></a>在 Visual C# 中使用代理帐户允许用户访问子系统  
 此代码示例演示如何使用 <xref:Microsoft.SqlServer.Management.Smo.Agent.ProxyAccount.AddSubSystem%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Agent.ProxyAccount> 方法来允许用户访问指定的子系统。  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Declare a JobServer object variable and reference the SQL Server Agent.   
JobServer js = default(JobServer);   
js = srv.JobServer;   
//Define a Credential object variable by supplying the parent server and name arguments in the constructor.   
Credential c = default(Credential);   
c = new Credential(srv, "Proxy_accnt");   
//Set the identity to a valid login represented by the vIdentity string variable.   
//The sub system will run under this login.   
c.Identity = vIdentity;   
//Create the credential on the instance of SQL Server.   
c.Create();   
//Define a ProxyAccount object variable by supplying the SQL Server Agent, the name, the credential, the description arguments in the constructor.   
ProxyAccount pa = default(ProxyAccount);   
pa = new ProxyAccount(js, "Test_proxy", "Proxy_accnt", true, "Proxy account for users to run job steps in command shell.");   
//Create the proxy account on the SQL Agent.   
pa.Create();   
//Add the login, represented by the vLogin string variable, to the proxy account.   
pa.AddLogin(vLogin);   
//Add the CmdExec subsytem to the proxy account.   
pa.AddSubSystem(AgentSubSystem.CmdExec);   
}   
//Now users logged on as vLogin can run CmdExec job steps with the specified credentials.   
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 代理](../../../ssms/agent/sql-server-agent.md)   
 [执行作业](../../../ssms/agent/implement-jobs.md)  
  
  
