---
description: 使用 Java 适用的 Microsoft SDK
title: 使用 Microsoft SDK for Java |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Java (Microsoft SDK for)
- Microsoft SDK for Java [ADO]
ms.assetid: 2d7cb5b5-8307-49dd-b07e-c07069bb1626
author: rothja
ms.author: jroth
ms.openlocfilehash: 13a518a3cbf7c4d9f83da5810e340406147f86f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028827"
---
# <a name="using-the-microsoft-sdk-for-java"></a>使用 Java 适用的 Microsoft SDK

> [!IMPORTANT]
> Microsoft 在2004年1月支持 Visual j + +。

Microsoft SDK for Java 是适用于 Microsoft Internet Explorer 环境的开发人员工具包。 提供工具、信息和示例以帮助你基于 JDK 1.1 和 Microsoft Win32 虚拟机 (Microsoft VM) 来开发 Java 程序和 applet。 Microsoft SDK for Java 未绑定到 Microsoft Visual j + +。 若要下载此 SDK，请单击此处。  
  
 Jactivex.exe 实用工具将从类型库生成类，但只能在命令行上调用。 此功能未与 Visual j + + 开发环境集成。 与 Java 类型库向导生成的类不同，可以单步执行由 SDK 创建的类包装。 这适用于调试代码使用 ADO 包装类的方式。  
  
 此机制读取 ADO 类型库，并生成可在应用程序中实例化的类。 它在以下位置生成这些类： \\<windows 目录 \> \Java\trustlib\msado15。  
  
 使用 Microsoft SDK for Java 在 Java 中创建 ADO 应用程序与使用 Java 类型库向导在本质上完全相同。 有关示例代码，请参阅 [ADO Java 类包装](./ado-java-class-wrappers.md)。 唯一的区别在于首先如何生成包装类，如以下步骤中所示。  
  
### <a name="to-create-an-ado-project-with-the-microsoft-sdk-for-java"></a>使用 Microsoft SDK for Java 创建 ADO 项目  
  
1.  在命令提示符处运行以下命令。 必须将路径设置为包括 Microsoft SDK for Java Bin 目录，或从该位置运行该命令。 通常，适用于 Java 的 Microsoft SDK 安装在与 Visual Studio 相同的位置。 这是一个命令语句。  
  
    ```java
    \<path to DevStudio>\<path to Java SDK>\bin\JactiveX.exe /javatlb "C:\program files\common files\system\ado\msado15.dll"  
    ```  
  
2.  运行以下命令以编译生成的类。 /G： t 开关会启用调试符号的生成，以便可以跟踪到。Java 符号。 对于发布版本，请将其删除。  
  
    ```java
    jvc /g:t c:\<windows>\Java\trustlib\msado15\*.Java  
    ```  
  
3.  若要使用这些文件，请在 Visual j + + 中打开项目。 从 " **项目** " 菜单中，选择 " **添加到项目**"。 选择 " **文件**"，并添加所有。在项目的 trustlib\msado15 目录中生成的 JAVA 文件。  
  
## <a name="see-also"></a>另请参阅  
 [ADO Java 类包装器](./ado-java-class-wrappers.md)