---
description: 步骤 1：设置 Visual Basic 项目
title: 步骤1：设置 Visual Basic 项目 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 77d3bfa5-fc9f-4a72-93b4-790c7d227988
author: rothja
ms.author: jroth
ms.openlocfilehash: b80f44160e3542147158f0bece7c87adb3ba7e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032439"
---
# <a name="step-1-set-up-the-visual-basic-project"></a>步骤 1：设置 Visual Basic 项目
在此方案中，假定你已在系统上安装了 Microsoft Visual Basic 6.0、ADO 2.5 或更高版本，以及用于 Internet 发布的 Microsoft OLE DB 提供程序。 您将首先创建一个新项目，然后将某些控件添加到项目中的默认窗体。  
  
### <a name="to-create-an-ado-project"></a>创建 ADO 项目：  
  
1.  在 Microsoft Visual Basic 中，创建一个新的标准 EXE 项目。  
  
2.  从 "项目" 菜单中选择 "引用"。  
  
3.  选择 "Microsoft ActiveX 数据对象2.5 库"，然后单击 "确定"。  
  
### <a name="to-insert-controls-on-the-main-form"></a>若要在主窗体上插入控件：  
  
1.  向 Form1 添加 ListBox 控件。 将其 Name 属性设置为 **lstMain**。  
  
2.  向 Form1 添加另一个 ListBox 控件。 将其 Name 属性设置为 **lstDetails**。  
  
3.  向 Form1 添加 TextBox 控件。 将其 Name 属性设置为 **txtDetails**。  
  
## <a name="see-also"></a>另请参阅  
 [Internet 发布方案](../../../ado/guide/data/internet-publishing-scenario.md)   
 [步骤 2：初始化主列表框](../../../ado/guide/data/step-2-initialize-the-main-list-box.md)
