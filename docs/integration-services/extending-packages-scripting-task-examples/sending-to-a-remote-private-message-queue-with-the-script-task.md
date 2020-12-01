---
description: 使用脚本任务向远程私有消息队列发送消息
title: 使用脚本任务向远程私有消息队列发送消息 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], remote private message queues
- Message Queue task [Integration Services]
- Script task [Integration Services], examples
ms.assetid: 636314fd-d099-45cd-8bb4-f730d0a06739
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5bc25b37bb4dfac2aa03795972edb56eb406aebd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430339"
---
# <a name="sending-to-a-remote-private-message-queue-with-the-script-task"></a>使用脚本任务向远程私有消息队列发送消息

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  消息队列（又称为 MSMQ）使开发人员可以通过发送和接收消息，轻松实现与应用程序之间的快速可靠通信。 消息队列既可位于本地计算机，也可位于远程计算机；既可以是公共的，也可以是私有的。 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中，MSMQ 连接管理器和消息队列任务不支持向远程计算机上的私有队列发送消息。 但通过使用脚本任务，可轻松地向远程私有队列发送消息。  
  
> [!NOTE]  
>  如果希望创建可更方便地重用于多个包的任务，请考虑以此脚本任务示例中的代码为基础，创建自定义任务。 有关详细信息，请参阅 [开发自定义任务](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  
  
## <a name="description"></a>说明  
 下面的示例使用一个现有 MSMQ 连接管理器以及 System.Messaging 命名空间的对象和方法，向远程私有消息队列发送包含在包变量中的文本。 对 MSMQ 连接管理器的 M:Microsoft.SqlServer.Dts.ManagedConnections.MSMQConn.AcquireConnection(System.Object) 方法的调用可返回 MessageQueue 对象，其 Send 方法可完成此任务。  
  
#### <a name="to-configure-this-script-task-example"></a>配置此脚本任务示例  
  
1.  使用默认名称创建一个 MSMQ 连接管理器。 设置有效远程私有队列的路径，格式如下：  
  
    ```  
    FORMATNAME:DIRECT=OS:<computername>\private$\<queuename>  
    ```  
  
2.  创建一个名为 MessageText 的 String 类型的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 变量，将消息文本传入脚本。 输入默认消息作为该变量的值。  
  
3.  向设计图面添加一个脚本任务，并对其进行编辑。 在“脚本任务编辑器”的“脚本”选项卡中，将 `MessageText` 变量添加到 ReadOnlyVariables 属性中，使该变量在脚本内可用。  
  
4.  单击“编辑脚本”，打开 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 脚本编辑器。  
  
5.  在脚本项目中添加对 System.Messaging 命名空间的引用。  
  
6.  用下面部分中的代码替换脚本窗口中的内容。  
  
## <a name="code"></a>代码  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports System.Messaging  
  
Public Class ScriptMain  
  
    Public Sub Main()  
  
        Dim remotePrivateQueue As MessageQueue  
        Dim messageText As String  
  
        remotePrivateQueue = _  
            DirectCast(Dts.Connections("Message Queue Connection Manager").AcquireConnection(Dts.Transaction), _  
            MessageQueue)  
        messageText = DirectCast(Dts.Variables("MessageText").Value, String)  
        remotePrivateQueue.Send(messageText)  
  
        Dts.TaskResult = ScriptResults.Success  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Messaging;  
  
public class ScriptMain  
{  
  
    public void Main()  
        {  
  
            MessageQueue remotePrivateQueue = new MessageQueue();  
            string messageText;  
  
            remotePrivateQueue = (MessageQueue)(Dts.Connections["Message Queue Connection Manager"].AcquireConnection(Dts.Transaction) as MessageQueue);  
            messageText = (string)(Dts.Variables["MessageText"].Value);  
            remotePrivateQueue.Send(messageText);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [消息队列任务](../../integration-services/control-flow/message-queue-task.md)  
  
  
