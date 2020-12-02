---
description: 为自定义 ForEach 枚举器开发用户界面
title: 为自定义 ForEach 枚举器开发用户界面 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom user interface [Integration Services], custom foreach enumerators
- custom foreach enumerators [Integration Services], developing custom user interface
ms.assetid: 8aa4aa80-c9ba-42b3-ba87-ae5ea5d3cac3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 55e03f664f2c0b9c5664c6bcb308cad21d628f84
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123108"
---
# <a name="developing-a-user-interface-for-a-custom-foreach-enumerator"></a>为自定义 ForEach 枚举器开发用户界面

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  重写基类的属性和方法的实现以提供自定义功能后，您可能需要为 Foreach 枚举器创建自定义用户界面。 如果未创建自定义用户界面，用户只能使用“属性”窗口来配置新的自定义 Foreach 枚举器。  
  
 在自定义用户界面项目或程序集中，可以创建一个实现 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI> 的类。 此类派生自通常用于创建复合控件以承载其他 Windows 窗体控件的 System.Windows.Forms.UserControl。 你创建的控件显示在“Foreach 循环编辑器”的“集合”选项卡的“枚举器配置”区域中。  
  
> [!IMPORTANT]  
>  在按照[生成、部署并调试自定义对象](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)中的说明生成自定义用户界面并签名而且在全局程序集缓存中安装后，还要在<xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> 的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A> 属性中提供此类的完全限定名称。  
  
## <a name="coding-the-user-interface-control-class"></a>编写用户界面控件类代码  
  
### <a name="initializing-the-user-interface"></a>初始化用户界面  
 可以重写 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI.Initialize%2A> 方法以缓存对主机对象以及包中定义的连接管理器集合和变量集合的引用。  
  
### <a name="setting-properties-on-the-user-interface-control"></a>设置用户界面控件的属性  
 从中派生用户界面类的 UserControl 类用作承载其他 Windows 窗体控件的复合控件。 由于此类承载其他控件，因此您可以像在任何 Windows 窗体应用程序中一样，通过拖放控件、排列控件、设置控件属性以及在运行时响应控件的事件来设计您自己的自定义用户界面。  
  
### <a name="saving-settings"></a>保存设置  
 可以重写 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI.SaveSettings%2A> 方法，以在用户关闭编辑器时，将用户选择的值从控件复制到枚举器的属性。  
  
## <a name="see-also"></a>另请参阅  
 [创建自定义 Foreach 枚举器](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)   
 [编写自定义 Foreach 枚举器代码](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)  
  
  
