---
description: 在 Visual Basic 6 应用程序中引用 ADO 库
title: 在 Visual Basic 6 应用程序中引用 ADO 库 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- libraries [ADO]
- referencing libraries in a Visual Basic application[ADO]
- ADO, libraries
ms.assetid: cfd37a82-aad2-41cd-8d13-1566c43d95f0
author: rothja
ms.author: jroth
ms.openlocfilehash: 431a58609ff26ace443b012bc2835e5ba89736db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036607"
---
# <a name="referencing-the-ado-libraries-in-a-visual-basic-6-application"></a>在 Visual Basic 6 应用程序中引用 ADO 库
若要将 ADO 库导入 Microsoft Visual Basic 6 应用程序，必须在 Visual Basic 项目中设置引用。  
  
### <a name="to-set-a-reference-to-the-ado-libraries-in-a-visual-basic-project"></a>设置对 Visual Basic 项目中 ADO 库的引用  
  
1.  创建新的或打开现有的 Visual Basic 项目。  
  
2.  单击 " **项目** " 菜单项，然后从下拉菜单面板中选择 " **引用 ...** "。  
  
3.  在 "**可用引用**" 中，选中 " **Microsoft ActiveX 数据对象 *n. n* " 库** 的框，其中，" **_n_"*表示最新的版本号。下面的 _* Location** 字段应将你的选择标识为 *$installDir\msado15.dll*，其中 *$installDir* 表示已安装 ADO 库的目录的路径。  
  
4.  如果要使用 ADO MD，请重复步骤3以选择 " **Microsoft ActiveX 数据对象 (多维)**" n "。 " **位置** " 字段应将此选项标识为 *$installDir\msadomd.dll*。  
  
5.  如果要使用 ADOX，请重复步骤3以选择 **用于 DDL 和安全性的 Microsoft ADO Ext.** n. n。 " **位置** " 字段应将此选项标识为 *$installDir\msadox.dll*。  
  
6.  单击 **"确定"** 完成设置引用。  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 安装 ADO 还会复制早期版本的以下类型库：  
  
-   *msado27*，ADO 2.7 类型库  
  
-   *msado26*，ADO 2.6 类型库  
  
-   *msado25*，ADO 2.5 类型库  
  
-   *msado21*，ADO 2.1 类型库  
  
-   *msado20*，ADO 2.0 类型库  
  
 如果你的应用程序必须出于向后兼容的原因而使用这些 ADO 库中的任何一个，则需要导入相应的类型库版本。 为此，请按照上一部分中的步骤操作，将 *msado15.dll* 替换 *MsadoXX*，其中 *XX* 表示需要导入的版本号。
