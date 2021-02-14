---
title: 初始化自定义程序集对象 | Microsoft Docs
ms.date: 03/03/2017
description: 了解使用从报表的全局对象集合中提供给你的值来初始化自定义类。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- initializing custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], initializing
- OnInit method
ms.assetid: 26fd74dc-d02f-40f7-aeb3-50ce05e9e6b9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 24009d6b4e7d1fbfe107d16b828f16454dff7bc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064522"
---
# <a name="initializing-custom-assembly-objects"></a>初始化自定义程序集对象
  在某些情况下，您可能需要在实例化自定义程序集类中的属性值和字段值时初始化它们。 您最可能需要使用从报表的全局对象集合中提供给您的值来初始化自定义类。 为此，需要覆盖报表的 Code 对象的 OnInit 方法   。 若要访问 OnInit，请使用报表定义的 Code 元素   。 可使用两种方法，在你计划在报表中使用的自定义程序集中初始化类的属性或字段值：可以使用 OnInit 声明和创建类的新实例，也可以使用 OnInit 调用公开可用的方法   。  
  
## <a name="global-object-collections-and-initialization"></a>全局对象集合和初始化  
 若干集合可用于初始化您的自定义类变量。 可以使用 Globals 和 User 集合   。 在调用 OnInit 方法时，Parameters、Fields 和 ReportItems 集合在报表生命周期中都不可用     。 若要使用共享集合（Globals 或 User），需要包括 Report 对象引用    。 例如，若要基于访问报表的用户的当前语言初始化自定义类，则 Code 元素可能如下  ：  
  
```  
<Code>  
   Dim m_myClass As MyClass  
  
   Protected Overrides Sub OnInit()  
      m_myClass = new MyClass(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 初始化上述类的属性值和字段值的一个方法就是声明您的类并通过调用某一覆盖的构造函数创建其新实例。  
  
 初始化自定义程序集中类的属性值和字段值的另一个方法是调用从 OnInit 方法定义的可以公共使用的方法  。 您首先需要在报表定义文件中为您的类添加一个实例名称。 一旦添加了相应的程序集引用和实例名称后，可以调用您的初始化方法以初始化类的属性值和字段值。 OnInit 方法可能如下  ：  
  
```  
<Code>  
   Protected Overrides Sub OnInit()  
      m_myClass.MyInitializationMethod(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 有关为自定义类添加程序集引用和实例名称的详细信息，请参阅[向报表添加程序集引用 (SSRS)](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md)。  
  
 有关全局对象集合的详细信息，请参阅[表达式中的内置集合（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md)。  
  
## <a name="see-also"></a>另请参阅  
 [将自定义程序集用于报表](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
