---
description: 编写自定义 Foreach 枚举器代码
title: 编写自定义 Foreach 枚举器代码 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom foreach enumerators [Integration Services], coding
ms.assetid: 279cf6de-d06f-40e7-b8ca-569310449f36
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 046b40b0dd7502e57614691066d8c87bcfe5d9fa
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123123"
---
# <a name="coding-a-custom-foreach-enumerator"></a>编写自定义 Foreach 枚举器代码

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  创建继承自 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator> 基类的类并将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> 属性应用于该类后，必须重写基类的属性和方法的实现以提供自定义功能。  
  
 若要查看自定义枚举器的工作示例，请参阅[为自定义 ForEach 枚举器开发用户界面](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)。  
  
## <a name="initializing-the-enumerator"></a>初始化枚举器  
 可以重写 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.InitializeForEachEnumerator%2A> 方法以缓存对包中定义的连接管理器的引用，和缓存对事件接口（用于引发错误、警告和信息性消息）的引用。  
  
## <a name="validating-the-enumerator"></a>验证枚举器  
 重写 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.Validate%2A> 方法以验证枚举器是否正确配置。 如果该方法返回“Failure”，则枚举器和包含枚举器的包将不执行。 此方法的实现是特定于每个枚举器的，但如果枚举器依赖于 <xref:Microsoft.SqlServer.Dts.Runtime.Variable> 或 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象，则应该添加代码以验证这些对象存在于提供给方法的集合中。  
  
 下面的代码示例演示 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.Validate%2A> 的实现，该接口用于检查在枚举器的属性中指定的变量。  
  
```csharp  
private string variableNameValue;  
  
public string VariableName  
{  
    get{ return this.variableNameValue; }  
    set{ this.variableNameValue = value; }  
}  
  
public override DTSExecResult Validate(Connections connections, VariableDispenser variableDispenser, IDTSInfoEvents infoEvents, IDTSLogging log)  
{  
    if (!variableDispenser.Contains(this.variableNameValue))  
    {  
        infoEvents.FireError(0, "MyEnumerator", "The Variable " + this.variableNameValue + " does not exist in the collection.", "", 0);  
            return DTSExecResult.Failure;  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Private variableNameValue As String  
  
Public Property VariableName() As String  
    Get   
         Return Me.variableNameValue  
    End Get  
    Set (ByVal Value As String)   
         Me.variableNameValue = value  
    End Set  
End Property  
  
Public Overrides Function Validate(ByVal connections As Connections, ByVal variableDispenser As VariableDispenser, ByVal infoEvents As IDTSInfoEvents, ByVal log As IDTSLogging) As DTSExecResult  
    If Not variableDispenser.Contains(Me.variableNameValue) Then  
        infoEvents.FireError(0, "MyEnumerator", "The Variable " + Me.variableNameValue + " does not exist in the collection.", "", 0)  
            Return DTSExecResult.Failure  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
## <a name="returning-the-collection"></a>返回集合  
 在执行期间，<xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> 容器调用自定义枚举器的 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> 方法。 在此方法中，枚举器创建并填充它的项集合，然后返回该集合。 然后 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> 遍历集合中的所有项，并为集合中的每个项执行控制流。  
  
 下面的示例演示返回随机整数数组的 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> 的实现。  
  
```csharp  
public override object GetEnumerator()  
{  
    ArrayList numbers = new ArrayList();  
  
    Random randomNumber = new Random(DateTime.Now);  
  
    for( int x=0; x < 100; x++ )  
        numbers.Add( randomNumber.Next());  
  
    return numbers;  
}  
```  
  
```vb  
Public Overrides Function GetEnumerator() As Object  
    Dim numbers As ArrayList =  New ArrayList()   
  
    Dim randomNumber As Random =  New Random(DateTime.Now)   
  
        Dim x As Integer  
        For  x = 0 To  100- 1  Step  x + 1  
        numbers.Add(randomNumber.Next())  
        Next  
  
    Return numbers  
End Function  
```  
 
## <a name="see-also"></a>另请参阅  
 [创建自定义 Foreach 枚举器](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)   
 [为自定义 ForEach 枚举器开发用户界面](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
  
  
