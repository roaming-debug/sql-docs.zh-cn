---
description: 步骤 2：初始化主列表框
title: 步骤2：初始化主列表框 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: a1454493-1c86-46c2-ada8-d3c6fcdaf3c1
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b1daa60ce9cb93f46ccdc852052f61ec5b58e6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036967"
---
# <a name="step-2-initialize-the-main-list-box"></a>步骤 2：初始化主列表框
若要声明全局记录和记录集对象，请将以下代码插入到 Form1 的 (常规)  () 声明中：  
  
```  
Option Explicit  
Dim grec As Record  
Dim grs As Recordset  
```  
  
 此代码为记录和记录集对象声明全局对象引用，稍后将在此方案中使用。  
  
## <a name="to-connect-to-a-url-and-populate-lstmain"></a>连接到 URL 并填充 lstMain  
 在 Form1 的窗体加载事件处理程序中插入以下代码：  
  
```  
Private Sub Form_Load()  
    Set grec = New Record  
    Set grs = New Recordset  
    grec.Open "", "URL=https://servername/foldername/", , _  
        adOpenIfExists Or adCreateCollection  
    Set grs = grec.GetChildren  
    While Not grs.EOF  
        lstMain.AddItem grs(0)  
        grs.MoveNext  
    Wend  
End Sub  
```  
  
 此代码将实例化全局记录和记录集对象。 `grec`使用指定为 ActiveConnection 的 URL 打开记录对象。 如果该 URL 存在，则将其打开;如果它尚不存在，则创建它。 请注意，应将替换 `https://servername/foldername/` 为您的环境中的有效 URL。  
  
 记录集对象 `grs` 在记录的子级上打开 `grec` 。 然后， `lstMain` 将用发布到 URL 的资源的文件名填充。  
  
## <a name="see-also"></a>另请参阅  
 [Internet 发布方案](../../../ado/guide/data/internet-publishing-scenario.md)   
 [步骤1：设置 Visual Basic 项目](../../../ado/guide/data/step-1-set-up-the-visual-basic-project.md)   
 [步骤 3：填充字段列表框](../../../ado/guide/data/step-3-populate-the-fields-list-box.md)
