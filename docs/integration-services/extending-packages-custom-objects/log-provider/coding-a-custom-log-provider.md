---
description: 编写自定义日志提供程序代码
title: 编写自定义日志提供程序代码 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom log providers [Integration Services], coding
ms.assetid: 979a29ca-956e-4fdd-ab47-f06e84cead7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 93ddf518405b44b5abb6956ee5da063b98ece1e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348893"
---
# <a name="coding-a-custom-log-provider"></a>编写自定义日志提供程序代码

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  创建继承自 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase> 基类的类并将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> 属性应用于该类后，必须重写基类的属性和方法的实现以提供自定义功能。  
  
 有关自定义日志提供程序的工作示例，请参阅[为自定义日志提供程序开发用户界面](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)。  
  
## <a name="configuring-the-log-provider"></a>配置日志提供程序  
  
### <a name="initializing-the-log-provider"></a>初始化日志提供程序  
 重写 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.InitializeLogProvider%2A> 方法以缓存对连接集合和事件接口的引用。 可以稍后在日志提供程序的其他方法中使用这些缓存的引用。  
  
### <a name="using-the-configstring-property"></a>使用 ConfigString 属性  
 在设计时。日志提供程序从“配置”  列接收配置信息。 此配置信息与日志提供程序的 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> 属性对应。 默认情况下，此列包含一个文本框，您可以从该文本框中检索任何字符串信息。 随 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 提供的大部分日志提供程序都使用此属性存储连接管理器的名称，以供提供程序连接外部数据源时使用。 如果日志提供程序使用 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> 属性，可使用 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> 方法验证此属性，并确保此属性已正确设置。  
  
### <a name="validating-the-log-provider"></a>验证日志提供程序  
 重写 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> 方法以确保提供程序已正确配置并且可以执行。 通常，最低级别的验证应确保 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> 已正确设置。 在日志提供程序从 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> 方法返回 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> 之前，执行不能继续。  
  
 下面的代码示例演示 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> 的实现，它确保连接管理器的名称已指定、连接管理器存在于包中以及连接管理器返回 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> 属性中的文件名。  
  
```csharp  
public override DTSExecResult Validate(IDTSInfoEvents infoEvents)  
{  
    if (this.ConfigString.Length == 0 || connections.Contains(ConfigString) == false)  
    {  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
        return DTSExecResult.Failure;  
    }  
    else  
    {  
        string fileName = connections[ConfigString].AcquireConnection(null) as string;  
  
        if (fileName == null || fileName.Length == 0)  
        {  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
            return DTSExecResult.Failure;  
        }  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As IDTSInfoEvents) As DTSExecResult  
    If Me.ConfigString.Length = 0 Or connections.Contains(ConfigString) = False Then  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
        Return DTSExecResult.Failure  
    Else   
        Dim fileName As String =  connections(ConfigString).AcquireConnectionCType(as string, Nothing)  
  
        If fileName = Nothing Or fileName.Length = 0 Then  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
            Return DTSExecResult.Failure  
        End If  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
### <a name="persisting-the-log-provider"></a>使日志提供程序持久化  
 通常不需要对连接管理器实现自定义持久性。 自定义持久性仅在对象的属性使用复杂数据类型时才需要。 有关详细信息，请参阅[开发用于 Integration Services 的自定义对象](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)。  
  
## <a name="logging-with-the-log-provider"></a>使用日志提供程序进行日志记录  
 有三个所有日志提供程序都必须重写的运行时方法：<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>、<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>。  
  
> [!IMPORTANT]  
>  在单个包的验证和执行期间，将多次调用 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> 方法。 请确保您的自定义代码不会导致较早的日志条目被下一次打开和关闭日志的操作所覆盖。 如果已选择在测试包中记录验证事件，则应该看到的第一个记录事件是 OnPreValidate；如果您看到的第一个记录事件是 PackageStart，则表示初始验证事件已被覆盖。  
  
### <a name="opening-the-log"></a>打开日志  
 大部分日志提供程序都连接外部数据源，如文件或数据库，以便存储在包执行期间收集的事件信息。 与运行时的任何其对象一样，与外部数据源的连接通常使用连接管理器对象来完成。  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> 方法在包执行开始时调用。请重写此方法以建立与外部数据源的连接。  
  
 下面的示例代码演示了在 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> 执行期间打开一个文本文件供写入的日志提供程序。 该提供程序通过调用在 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> 属性中指定的连接管理器的 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> 方法来打开文件。  
  
```csharp  
public override void OpenLog()  
{  
    if(!this.connections.Contains(this.ConfigString))  
        throw new Exception("The ConnectionManager " + this.ConfigString + " does not exist in the Connections collection.");  
  
    this.connectionManager = connections[ConfigString];  
    string filePath = this.connectionManager.AcquireConnection(null) as string;  
  
    if(filePath == null || filePath.Length == 0)  
        throw new Exception("The ConnectionManager " + this.ConfigString + " is not a valid FILE ConnectionManager");  
  
    //  Create a StreamWriter to append to.  
    sw = new StreamWriter(filePath,true);  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString());  
}  
```  
  
```vb  
Public Overrides  Sub OpenLog()  
    If Not Me.connections.Contains(Me.ConfigString) Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " does not exist in the Connections collection.")  
    End If  
  
    Me.connectionManager = connections(ConfigString)  
    Dim filePath As String =  Me.connectionManager.AcquireConnectionCType(as string, Nothing)  
  
    If filePath = Nothing Or filePath.Length = 0 Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " is not a valid FILE ConnectionManager")  
    End If  
  
    '  Create a StreamWriter to append to.  
    sw = New StreamWriter(filePath,True)  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString())  
End Sub  
```  
  
### <a name="writing-log-entries"></a>写入日志条目  
 每次包中的对象通过对某一事件接口调用 Fire\<event> 方法来引发事件时，都会调用 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> 方法。 每次引发事件时，都会提供其上下文的相关信息，通常是说明性消息。 但是，并非所有对 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> 方法的调用都包含每个方法参数的信息。 例如，一些具有自我说明性名称的标准事件不提供 MessageText，并且 DataCode 和 DataBytes 只提供可选的补充信息。  
  
 下面的代码示例实现了 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> 方法，并将事件写入前述打开的流中。  
  
```csharp  
public override void Log(string logEntryName, string computerName, string operatorName, string sourceName, string sourceID, string executionID, string messageText, DateTime startTime, DateTime endTime, int dataCode, byte[] dataBytes)  
{  
    sw.Write(logEntryName + ",");  
    sw.Write(computerName + ",");  
    sw.Write(operatorName + ",");  
    sw.Write(sourceName + ",");  
    sw.Write(sourceID + ",");  
    sw.Write(messageText + ",");  
    sw.Write(dataBytes + ",");  
    sw.WriteLine("");  
}  
```  
  
```vb  
Public Overrides  Sub Log(ByVal logEnTryName As String, ByVal computerName As String, ByVal operatorName As String, ByVal sourceName As String, ByVal sourceID As String, ByVal executionID As String, ByVal messageText As String, ByVal startTime As DateTime, ByVal endTime As DateTime, ByVal dataCode As Integer, ByVal dataBytes() As Byte)  
    sw.Write(logEnTryName + ",")  
    sw.Write(computerName + ",")  
    sw.Write(operatorName + ",")  
    sw.Write(sourceName + ",")  
    sw.Write(sourceID + ",")  
    sw.Write(messageText + ",")  
    sw.Write(dataBytes + ",")  
    sw.WriteLine("")  
End Sub  
```  
  
### <a name="closing-the-log"></a>关闭日志  
 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> 方法在包执行结束时、包中的所有对象都执行完毕后或包由于错误而停止时调用。  
  
 下面的代码示例演示了 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> 方法的实现，该方法用于关闭在 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> 方法执行期间打开的文件流。  
  
```csharp  
public override void CloseLog()  
{  
    if (sw != null)  
    {  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString());  
        sw.Close();  
    }  
}  
```  
  
```vb  
Public Overrides  Sub CloseLog()  
    If Not sw Is Nothing Then  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString())  
        sw.Close()  
    End If  
End Sub  
```  
 
## <a name="see-also"></a>另请参阅  
 [创建自定义日志提供程序](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)   
 [为自定义日志提供程序开发用户界面](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
