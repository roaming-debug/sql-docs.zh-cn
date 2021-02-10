---
description: Microsoft OLE DB 简单提供程序概述
title: Microsoft OLE DB 简单提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- simple provider [ADO]
- providers [ADO], OLE DB simple provider
- OLE DB simple provider [ADO]
ms.assetid: 1e7dc6f0-482c-4103-8187-f890865e40fc
author: rothja
ms.author: jroth
ms.openlocfilehash: a07224d352f059b8aad3709c6d4f9605c16696b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029231"
---
# <a name="microsoft-ole-db-simple-provider-overview"></a>Microsoft OLE DB 简单提供程序概述
Microsoft OLE DB 简单提供程序 (OSP) 允许 ADO 访问使用 [OLE DB 简单提供程序 (OSP) 工具包](/previous-versions/windows/desktop/ms715822(v=vs.85))编写的提供程序的任何数据。 简单提供程序用于访问只需要基本 OLE DB 支持的数据源，例如内存中数组或 XML 文档。

## <a name="connection-string-parameters"></a>连接字符串参数
 若要连接到 OLE DB 简单提供程序 DLL，请将 *提供程序* 参数设置为 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) 属性，以：

```vb
MSDAOSP
```

 还可以使用 [Provider](../../reference/ado-api/provider-property-ado.md) 属性设置或读取此值。

 您可以使用已注册的提供程序名称（由提供程序编写器确定），连接到已注册为完全 OLE DB 提供程序的简单提供程序。

## <a name="typical-connection-string"></a>典型连接字符串
 此提供程序的典型连接字符串是：

```vb
"Provider=MSDAOSP;Data Source=serverName"
```

 字符串包含以下关键字：

|关键字|说明|
|-------------|-----------------|
|**提供程序**|指定 SQL Server 的 OLE DB 提供程序。|
|**数据源**|指定服务器的名称。|

## <a name="xml-document-example"></a>XML 文档示例
 MDAC 2.7 或更高版本中的 OLE DB 简单提供程序 (OSP) ，并增强了 windows DAC) 的 Windows 数据访问 (组件，以支持通过任意 XML 文件打开分层 ADO **记录集** 。 这些 XML 文件可能包含 ADO XML 持久性架构，但不是必需的。 这已经通过将 OSP 连接到 **MSXML2.DLL** 来实现;因此需要 **MSXML2.DLL** 或更高版本。

 下面的示例中使用的 **portfolio.xml** 文件包含以下树：

```console
Portfolio
   Stock
      Shares
      Symbol
      Price
      Info
         Company Name
         WebSite
```

 XML DSO 使用内置试探法将 XML 树中的节点转换为分层 **记录集中** 的章节。

 使用这些内置试探法，XML 树将转换为以下形式的两级分层 **记录集** ：

```console
Parent Recordset
Shares, Symbol, Price, $Text
   Child Recordset
      Company Name, WebSite, $Text
```

 请注意，"项目组合" 和 "信息" 标记不在分层 **记录集中** 表示。 有关 XML DSO 如何将 XML 树转换为分层 **记录集** 的说明，请参阅以下规则。 下一节将讨论 $Text 列。

## <a name="rules-for-assigning-xml-elements-and-attributes-to-columns-and-rows"></a>将 XML 元素和属性分配给列和行的规则
 XML DSO 遵循在数据绑定应用程序中将元素和属性分配给列和行的过程。 XML 作为树建模，其中一个标记包含整个层次结构。 例如，书籍的 XML 说明可以包含章节标记、图标记和节标记。 最高级别为书籍标记，其中包含子元素章节、图和节。 当 XML DSO 将 XML 元素映射到行和列时，将转换子元素，而不是顶级元素。

 XML DSO 使用此过程来转换子元素：

-   每个子元素和属性都对应于层次结构中某些 **记录集中** 的列。

-   列的名称与子元素或属性的名称相同，除非父元素具有具有相同名称的属性和子元素，在这种情况下，子元素的列名前面会出现 "！"。

-   每个列都是一个 *简单* 列，其中包含标量值 (通常为字符串) 或包含子 **记录集** 的 **记录集** 列。

-   与属性相对应的列总是简单的。

-   如果子元素具有其自己的子元素或属性 (或两者均) ，或子元素的父项具有子元素的多个实例，则与子元素对应的列是 **记录集** 列。 否则列很简单。

-   如果有多个子元素的实例 (在不同的父) 下，则如果有 *任何* 实例表示 **记录集** 列，则其列是 **记录集** 列;仅当 *所有实例都* 指简单列时，其列才简单。

-   所有 **记录集** 都有一个名为 $Text 的附加列。

 构造 **记录集** 所需的代码如下所示：

```vb
Dim adoConn as ADODB.Connection
Dim adoRS as ADODB.Recordset

Set adoRS = New ADODB.Connection
Set adoRS = New ADODB.Recordset

adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
adoRS.Open "https://WebServer/VRoot/portfolio.xml, adoConn
```

> [!NOTE]
>  可以使用四个不同的命名约定来指定数据文件的路径。

```vb
'HTTP://
adoRS.Open "https://WebServer/VRoot/portfolio.xml", adoConn
'FILE://
adoRS.Open "file:/// C:\\Directory\\portfolio.xml", adoConn
'UNC Path
adoRS.Open "\\ComputerName\ShareName\portfolio.xml", adoConn
'Full DOS Path
adoRS.Open "C:\Directory\portfolio.xml", adoConn
```

 一旦打开该 **记录集** ，就可以使用常用的 ADO **记录集** 导航命令。

 OSP 生成的 **记录集** 有一些限制：

-   不支持 (**adUseClient**) 的客户端游标。

-   无法使用 Recordset 保存通过任意 XML 创建的分层 **记录集** **。保存**。

-   用 OSP 创建的 **记录集** 是只读的。

-   XMLDSO 向层次结构中的每个 **记录集** 添加一个额外的数据列 ($Text) 。

 有关 OLE DB 简单提供程序的详细信息，请参阅 [构建简单的提供程序](/previous-versions/windows/desktop/ms721067(v=vs.85))。

## <a name="code-example"></a>代码示例
 下面的 Visual Basic 代码演示如何打开任意 XML 文件、构造分层 **记录集**，以及以递归方式将每个 **记录集** 的每个记录写入 "调试" 窗口。

 下面是包含股票行情的简单 XML 文件。 下面的代码使用此文件来构造两级分层 **记录集**。

```xml
<portfolio>
   <stock>
      <shares>100</shares>
      <symbol>MSFT</symbol>
      <price>$70.00</price>
      <info>
         <companyname>Microsoft Corporation</companyname>
         <website>https://www.microsoft.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>AAPL</symbol>
      <price>$107.00</price>
      <info>
         <companyname>Apple Computer, Inc.</companyname>
         <website>https://www.apple.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>DELL</symbol>
      <price>$50.00</price>
      <info>
         <companyname>Dell Corporation</companyname>
         <website>https://www.dell.com</website>
      </info>
    </stock>
    <stock>
       <shares>100</shares>
       <symbol>INTC</symbol>
       <price>$115.00</price>
       <info>
          <companyname>Intel Corporation</companyname>
          <website>https://www.intel.com</website>
       </info>
   </stock>
</portfolio>
```

 下面是两个 Visual Basic sub 过程。 第一个创建该 **记录集** 并将其传递给 *WalkHier* sub 过程，该过程将以递归方式遍历层次结构，将每个记录 **集中** 每个记录的每个 **字段** 写入调试窗口。

```vb
Private Sub BrowseHierRecordset()
' Add ADO 2.7 or later to Project/References
' No need to add MSXML2, ADO just passes the ProgID through to the OSP.

    Dim adoConn As ADODB.Connection
    Dim adoRS As ADODB.Recordset
    Dim adoChildRS As ADODB.Recordset

    Set adoConn = New ADODB.Connection
    Set adoRS = New ADODB.Recordset
    Set adoChildRS = ADODB.Recordset

    adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
    adoRS.Open "https://bwillett3/Kowalski/portfolio.xml", adoConn

    Dim iLevel As Integer
    iLevel = 0
    WalkHier iLevel, adoRS

End Sub

Sub WalkHier(ByVal iLevel As Integer, ByVal adoRS As ADODB.Recordset)
    iLevel = iLevel + 1
    PriorLevel = iLevel
    While Not adoRS.EOF
        For ndx = 0 To adoRS.Fields.Count - 1
            If adoRS.Fields(ndx).Name <> "$Text" Then
                If adoRS.Fields(ndx).Type = adChapter Then
                    Set adoChildRS = adoRS.Fields(ndx).Value
                    WalkHier iLevel, adoChildRS
                Else
                    Debug.Print iLevel & ": adoRS.Fields(" & ndx & _
                       ") = " & adoRS.Fields(ndx).Name & " = " & _
                       adoRS.Fields(ndx).Value
                End If
            End If
        Next ndx
        adoRS.MoveNext
    Wend
    iLevel = PriorLevel
End Sub
```