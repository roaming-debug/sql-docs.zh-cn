---
description: 使用 WMI 提供程序管理服务和网络设置
title: 使用 WMI 提供程序管理服务和网络设置
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- WMI provider [SMO]
- services [SQL Server], SMO
- network settings [SMO]
- monitoring [SMO]
ms.assetid: ef8c3986-1098-4f21-b03a-f1f6bdb51c26
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 087fd4ce2ffa37fe16a5609c721072169ff644fc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475478"
---
# <a name="managing-services-and-network-settings-by-using-wmi-provider"></a>使用 WMI 提供程序管理服务和网络设置
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  WMI 提供程序是可供 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 管理控制台 (MMC) 用来管理 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务和网络协议的已发布接口。 在 SMO 中，<xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> 对象表示 WMI 提供程序。  
  
 <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> 对象的运行与通过 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象建立的与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的连接无关，而是使用 Windows 凭据连接到 WMI 服务。  
  
## <a name="example"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  

  
 对于使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] wmi 提供程序的程序，必须包括 **Imports** 语句以限定 wmi 命名空间。 请在应用程序的其他 **Imports** 语句之后、任何声明之前插入该语句，例如：  
  
 `Imports Microsoft.SqlServer.Management.Smo`  
  
 `Imports Microsoft.SqlServer.Management.Common`  
  
 `Imports Microsoft.SqlServer.Management.Smo.Wmi`  
  
## <a name="stopping-and-restarting-the-microsoft-sql-server-service-to-the-instance-of-sql-server-in-visual-basic"></a>在 Visual Basic 中停止并重新启动 SQL Server 实例的 Microsoft SQL Server 服务  
 该代码示例演示了如何通过使用 SMO <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> 对象停止和启动服务。 该示例为用于配置管理的 WMI 提供程序提供了接口。  
  
```VBNET
'Declare and create an instance of the ManagedComputer object that represents the WMI Provider services.
Dim mc As ManagedComputer
mc = New ManagedComputer()
'Iterate through each service registered with the WMI Provider.
Dim svc As Service
For Each svc In mc.Services
    Console.WriteLine(svc.Name)
Next
'Reference the Microsoft SQL Server service.
svc = mc.Services("MSSQLSERVER")
'Stop the service if it is running and report on the status continuously until it has stopped.
If svc.ServiceState = ServiceState.Running Then
    svc.Stop()

    Console.WriteLine(String.Format("{0} service state is {1}", svc.Name, svc.ServiceState))
    Do Until String.Format("{0}", svc.ServiceState) = "Stopped"
        Console.WriteLine(String.Format("{0}", svc.ServiceState))
        svc.Refresh()
    Loop
    Console.WriteLine(String.Format("{0} service state is {1}", svc.Name, svc.ServiceState))
    'Start the service and report on the status continuously until it has started.
    svc.Start()
    Do Until String.Format("{0}", svc.ServiceState) = "Running"
        Console.WriteLine(String.Format("{0}", svc.ServiceState))
        svc.Refresh()
    Loop
    Console.WriteLine(String.Format("{0} service state is {1}", svc.Name, svc.ServiceState))

Else
    Console.WriteLine("SQL Server service is not running.")
End If
```
  
## <a name="enabling-a-server-protocol-using-a-urn-string-in-visual-basic"></a>在 Visual Basic 中使用 URN 字符串启用服务器协议  
 此代码示例演示如何使用 URN 对象标识服务器协议并启用该协议。  
  
```VBNET  
'This program must run with administrator privileges.  
        'Declare the ManagedComputer WMI interface.  
        Dim mc As New ManagedComputer()  
  
        'Create a URN object that represents the TCP server protocol.  
        Dim u As New Urn("ManagedComputer[@Name='V-ROBMA3']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']")  
  
        'Declare the serverProtocol variable and return the ServerProtocol object.  
        Dim sp As ServerProtocol  
        sp = mc.GetSmoObject(u)  
  
        'Enable the protocol.  
        sp.IsEnabled = True  
  
        'propagate back to the service  
        sp.Alter()  
```  
  
## <a name="enabling-a-server-protocol-using-a-urn-string-in-powershell"></a>在 PowerShell 中使用 URN 字符串启用服务器协议  
 此代码示例演示如何使用 URN 对象标识服务器协议并启用该协议。  
  
```powershell  
#This example shows how to identify a server protocol using a URN object, and then enable the protocol  
#This program must run with administrator privileges.  
  
#Load the assembly containing the classes used in this example  
[reflection.assembly]::LoadWithPartialName("Microsoft.SqlServer.SqlWmiManagement")  
  
#Get a managed computer instance  
$mc = New-Object -TypeName Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer  
  
#Create a URN object that represents the TCP server protocol  
#Change 'MyPC' to the name of the your computer   
$urn = New-Object -TypeName Microsoft.SqlServer.Management.Sdk.Sfc.Urn -argumentlist "ManagedComputer[@Name='MyPC']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
  
#Get the protocol object  
$sp = $mc.GetSmoObject($urn)  
  
#enable the protocol on the object  
$sp.IsEnabled = $true  
  
#propagate back to actual service  
$sp.Alter()  
```  
  
## <a name="starting-and-stopping-a-service-in-visual-c"></a>在 Visual C# 中停止和启动服务  
 该代码示例演示了如何停止和启动 SQL Server 实例。  
  
```csharp  
{   
   //Declare and create an instance of the ManagedComputer   
   //object that represents the WMI Provider services.   
   ManagedComputer mc;   
   mc = new ManagedComputer();   
   //Iterate through each service registered with the WMI Provider.   
  
   foreach (Service svc in mc.Services)  
   {   
      Console.WriteLine(svc.Name);   
   }   
//Reference the Microsoft SQL Server service.   
  Service Mysvc = mc.Services["MSSQLSERVER"];   
//Stop the service if it is running and report on the status  
// continuously until it has stopped.   
   if (Mysvc.ServiceState == ServiceState.Running) {   
      Mysvc.Stop();   
      Console.WriteLine(string.Format("{0} service state is {1}", Mysvc.Name, Mysvc.ServiceState));   
      while (!(string.Format("{0}", Mysvc.ServiceState) == "Stopped")) {   
         Console.WriteLine(string.Format("{0}", Mysvc.ServiceState));   
          Mysvc.Refresh();   
      }   
      Console.WriteLine(string.Format("{0} service state is {1}", Mysvc.Name, Mysvc.ServiceState));   
//Start the service and report on the status continuously   
//until it has started.   
      Mysvc.Start();   
      while (!(string.Format("{0}", Mysvc.ServiceState) == "Running")) {   
         Console.WriteLine(string.Format("{0}", Mysvc.ServiceState));   
         Mysvc.Refresh();   
      }   
      Console.WriteLine(string.Format("{0} service state is {1}", Mysvc.Name, Mysvc.ServiceState));  
      Console.ReadLine();  
   }   
   else {   
      Console.WriteLine("SQL Server service is not running.");  
      Console.ReadLine();  
   }   
}  
```  
  
## <a name="starting-and-stopping-a-service-in-powershell"></a>在 PowerShell 中停止和启动服务  
 该代码示例演示了如何停止和启动 SQL Server 实例。  
  
```powershell  
#Load the assembly containing the objects used in this example  
[reflection.assembly]::LoadWithPartialName("Microsoft.SqlServer.SqlWmiManagement")  
  
#Get a managed computer instance  
$mc = New-Object -TypeName Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer  
  
#List out all sql server instnces running on this mc  
foreach ($Item in $mc.Services){$Item.Name}  
  
#Get the default sql server datbase engine service  
$svc = $mc.Services["MSSQLSERVER"]  
  
# for stopping and starting services PowerShell must run as administrator  
  
#Stop this service  
$svc.Stop()  
$svc.Refresh()  
while ($svc.ServiceState -ne "Stopped")  
{  
$svc.Refresh()  
$svc.ServiceState  
}  
"Service" + $svc.Name + " is now stopped"  
"Starting " + $svc.Name  
$svc.Start()  
$svc.Refresh()  
while ($svc.ServiceState -ne "Running")  
{  
$svc.Refresh()  
$svc.ServiceState  
}  
$svc.ServiceState  
"Service" + $svc.Name + "is now started"  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [用于配置管理的 WMI 提供程序的概念](../../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md)  
  
  
