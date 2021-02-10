---
description: VBScript ADO 编程
title: VBScript ADO 编程 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, VBScript
- VBScript [ADO]
ms.assetid: 6aaaf6d0-1376-4473-bea6-b81f2645a9ac
author: rothja
ms.author: jroth
ms.openlocfilehash: 31e6c98d12c6c6f472053634d8b3aea314ccbab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037867"
---
# <a name="vbscript-ado-programming"></a>VBScript ADO 编程
## <a name="creating-an-ado-project"></a>创建 ADO 项目  
 Microsoft Visual Basic 脚本编写版不支持类型库，因此您不需要在项目中引用 ADO。 因此，不支持任何相关功能，如命令行完成。 此外，默认情况下，VBScript 中未定义 ADO 枚举的常量。  
  
 但是，ADO 提供包含以下定义的两个包含文件用于 VBScript：  
  
-   对于服务器端脚本使用 Adovbs，默认情况下，该文件安装在 c:\Program Files\Common Files\System\ado\ 文件夹中。  
  
-   对于客户端脚本使用 Adcvbs，默认情况下，该文件安装在 c:\Program Files\Common Files\System\msdac\ 文件夹中。  
  
 您可以将常数定义从这些文件复制并粘贴到您的 ASP 页中，或者，如果您正在执行服务器端脚本编写，请将 Adovbs 文件复制到您的网站上的一个文件夹中，然后从您的 ASP 页引用它，如下所示：  
  
```vb
<!--#include File="adovbs.inc"-->  
```  
  
## <a name="creating-ado-objects-in-vbscript"></a>在 VBScript 中创建 ADO 对象  
 不能使用 **Dim** 语句将对象分配到 VBScript 中的特定类型。 另外，VBScript 不支持 Visual Basic for Applications 中 **Dim** 语句使用的 **新** 语法。 您必须改用 **CreateObject** 函数调用：  
  
```vb
Dim Rs1  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
```  
  
## <a name="vbscript-examples"></a>VBScript 示例  
 下面的代码是 (ASP) 文件的 Active Server 页面中 VBScript 服务器端编程的一般示例：  
  
```vb
<%  @LANGUAGE="VBSCRIPT" %>  
<%  Option Explicit %>  
<!--#include File="adovbs.inc"-->  
<HTML>  
    <BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
  
    <!-- Your ASP Code goes here -->  
<%  
Dim Source  
Dim Connect  
Dim Rs1  
  
Source = "SELECT * FROM Authors"  
Connect = "Provider=sqloledb;Data Source=srv;" & _  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
Rs1.Open Source, Connect, adOpenForwardOnly  
Response.Write("Success!")  
%>  
    </BODY>  
</HTML>  
```  
  
 ADO 文档附带了更为具体的 VBScript 示例。 有关详细信息，请参阅 [Microsoft Visual Basic Scripting Edition 中的 ADO 代码示例](../../reference/ado-api/ado-code-examples-vbscript.md)。  
  
## <a name="differences-between-vbscript-and-visual-basic"></a>VBScript 与 Visual Basic 之间的差异  
 使用 ADO 和 VBScript 类似于将 ADO 与 Visual Basic 一起使用，其中包括如何使用语法。 但存在一些重要的差异：  
  
-   VBScript 仅支持 Variant 数据类型，该类型可以保存不同类型的数据。 您可以在变量数据类型中存储所需的数据，数据将正常运行，因为 VBScript 执行了强制转换。 它识别 ADO 所需的类型，并相应地转换变量中的值。  
  
-   不能在 VBScript 中 **的 \<label> 错误 goto 上** 使用。  
  
-   VBScript 支持某些内置 Visual Basic 函数，例如 **Msgbox**、 **Date** 和 **IsNumeric**。 但是，由于 VBScript 是 Visual Basic 的子集，因此并非所有内置函数都受支持。 例如，VBScript 不支持 **Format** 函数和文件 i/o 函数。