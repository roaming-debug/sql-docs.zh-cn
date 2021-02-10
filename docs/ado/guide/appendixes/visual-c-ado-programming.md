---
description: Visual C++ ADO 编程
title: Visual C++ ADO 编程 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO]
ms.assetid: 11233b96-e05c-4221-9aed-5f20944b0f1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 420bfbba2eaa3946ce9a373c4ac31a88b540f7d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028702"
---
# <a name="visual-c-ado-programming"></a>Visual C++ ADO 编程
ADO API 参考介绍了 ADO 应用程序编程接口的功能 (API) 使用类似于 Microsoft Visual Basic 的语法。 尽管目标受众是所有用户，但 ADO 程序员使用各种语言（如 Visual Basic）、Visual C++ (，但不使用 **#import** 指令) ，而 Visual j + + (使用 ADO/WFC 类包) 。  

> [!NOTE]
> Microsoft 在2004中结束了对 Visual j + + 的支持。

 为了适应这种多样性， [Visual C++ 语法索引的 ADO](./using-ado-with-microsoft-visual-c.md) 提供了 Visual C++ 语言特定的语法，其中包含指向 API 参考中的功能、参数、异常行为等的常用说明的链接。  
  
 ADO 是通过 COM (组件对象模型) 接口实现的。 但是，程序员在某些编程语言中使用 COM 比使用其他语言更容易。 例如，几乎所有使用 COM 的细节都是为 Visual Basic 程序员隐式处理的，而 Visual C++ 程序员必须亲自参与这些细节。  
  
 以下部分汇总了使用 ADO 和 **#import** 指令的 c 和 c + + 程序员的详细信息。 它侧重于特定于 COM (**Variant**、 **BSTR** 和 **SafeArray**) 的数据类型，以及 (_com_error) 的错误处理。  
  
## <a name="using-the-import-compiler-directive"></a>使用 #import 编译器指令  
 **#Import** Visual C++ 编译器指令可简化使用 ADO 方法和属性。 指令采用包含类型库的文件的名称，如 ( # A0) ，并生成包含 typedef 声明、接口的智能指针和枚举常量的头文件。 每个接口都在类中封装或包装。  
  
 对于类中的每个操作 (即) 方法或属性调用，有一种声明可直接调用该操作 (即，操作) 的 "原始" 格式，以及用于调用原始操作并在操作失败时引发 COM 错误的声明。 如果该操作是一个属性，则通常会有一个编译器指令，该指令为操作创建了一个替代语法，该语法具有 Visual Basic 的语法。  
  
 检索属性值的操作具有格式为 **Get**_属性_ 的名称。 设置属性的值的操作具有 form、 **Put**_属性_ 的名称。 将属性的值设置为指向 ADO 对象的指针的操作具有 **PutRef**_属性_ 的名称。  
  
 您可以使用以下形式的调用获取或设置属性：  
  
```cpp
variable = objectPtr->GetProperty(); // get property value   
objectPtr->PutProperty(value);       // set property value  
objectPtr->PutRefProperty(&value);   // set property with object pointer  
```
  
## <a name="using-property-directives"></a>使用属性指令  
 **__Declspec (属性 ... )** 编译器指令是一种特定于 Microsoft 的 C 语言扩展，它声明一个函数，该函数用作属性以具有替代语法。 因此，你可以通过类似于 Visual Basic 的方式来设置或获取属性的值。 例如，可以通过以下方式设置和获取属性：  
  
```cpp
objectPtr->property = value;        // set property value  
variable = objectPtr->property;     // get property value  
```
  
 请注意，您不必编码：  
  
```cpp
objectPtr->PutProperty(value);      // set property value  
variable = objectPtr->GetProperty;  // get property value  
```
  
 编译器将根据 _-_ 声明的替代语法以及是否读取或写入属性，生成相应的 Get、 **Put** 或 **PutRef**_属性_ 调用。  
  
 **__Declspec (属性 ... )** 编译器指令只能声明 **get**、 **put** 或 **get** 和 **put** 函数的替代语法。 只读操作仅具有 **get** 声明;只写操作只有 **put** 声明;读取和写入操作都具有 **get** 和 **put** 声明。  
  
 此指令只能有两个声明;但是，每个属性可以有三个属性函数： **Get**_property_、 **Put**_属性_ 和 **PutRef**_属性_。 在这种情况下，只有两种形式的属性具有替代语法。  
  
 例如，使用 **Get**_ActiveConnection_ 和 **PutRef**_ActiveConnection_ 的替代语法声明 **命令** 对象 **ActiveConnection** 属性。 **PutRef** 语法是一个不错的选择，因为在实践中，您通常需要将打开的 **连接** 对象 (即，此属性中) **连接** 对象指针。 另一方面， **Recordset** 对象具有 **Get**、 **Put** 和 **PutRef**_ActiveConnection_ 操作，但没有其他语法。  
  
## <a name="collections-the-getitem-method-and-the-item-property"></a>集合、GetItem 方法和 Item 属性  

 ADO 定义多个集合，包括 **字段**、 **参数**、 **属性** 和 **错误**。 在 Visual C++ 中， **GetItem (_index_)** 方法返回集合的成员。 *Index* 是一个 **变量**，其值是集合中成员的数字索引，或是包含成员名称的字符串。  
  
 **__Declspec (属性 ... )** 编译器指令将 **Item** 属性声明为每个集合的基本 **GetItem ( # B3** 方法的替代语法。 替代语法使用方括号，类似于数组引用。 通常，这两种形式如下所示：  
  
```cpp
  
      collectionPtr->GetItem(index);  
collectionPtr->Item[index];  
```
  
 例如，将一个值分配给 **记录集** 对象（名为 **_rs_**）的字段，该字段是从 **pubs** 数据库的 **authors** 表派生的。 使用 **Item ( # B1** 属性访问 **Recordset** 对象 **字段** 集合的第三个 **字段** (集合从零开始索引;假定第三个字段名为 **_au \_ fname_**) 。 然后调用 **字段** 对象上 **( # B1** 方法的值来分配字符串值。  
  
 这可以通过以下四种方式表示， (最后两种形式对于 Visual Basic 是唯一的 Visual Basic;其他语言没有等效) ：  
  
```cpp
rs.Fields.Item(2).Value = "value"  
rs.Fields.Item("au_fname").Value = "value"  
rs(2) = "value"  
rs!au_fname = "value"  
```
  
 与上述前两个窗体 Visual C++ 的等效项是：  
  
```cpp
rs->Fields->GetItem(long(2))->PutValue("value");   
rs->Fields->GetItem("au_fname")->PutValue("value");  
```
  
 -或- (还显示了 **Value** 属性的替代语法)   
  
```cpp
rs->Fields->Item[long(2)]->Value = "value";  
rs->Fields->Item["au_fname"]->Value = "value";  
```
  
 有关循环访问集合的示例，请参阅 "ADO 参考" 的 "ADO 集合" 一节。  
  
## <a name="com-specific-data-types"></a>COM-Specific 数据类型  
 通常，在 ADO API 引用中找到的任何 Visual Basic 数据类型都具有 Visual C++ 等效的。 其中包括标准数据类型（例如 Visual Basic **字节** 的 **无符号字符**）、**整数** 的 **短** 整型 **和长整型。**  查找语法 Indexesto，查看给定方法或属性的操作数所需的确切内容。  
  
 此规则的例外情况是特定于 COM 的数据类型： **Variant**、 **BSTR** 和 **SafeArray**。  
  
### <a name="variant"></a>变量  
 **变体** 是一种结构化的数据类型，它包含值成员和数据类型成员。 **变量** 可以包含各种其他数据类型，包括其他变体、BSTR、Boolean、IDispatch 或 IUnknown 指针、货币、日期等。 COM 还提供了一些方法，使您可以轻松地将一种数据类型转换为另一种数据类型。  
  
 **_Variant_t** 类封装并管理 **variant** 数据类型。  
  
 当 ADO API 引用显示某个方法或属性操作数采用一个值时，这通常意味着在 **_variant_t** 中传递该值。  
  
 当 ADO API 参考中的 " **参数** " 部分显示 "操作数" 是 **变量** 时，此规则将显式为 true。 一种例外情况是，在文档明确指出操作数采用标准数据类型时，例如 **Long** 或 **Byte**，或枚举。 另一种例外情况是操作数采用 **字符串**。  
  
### <a name="bstr"></a>BSTR  
 **BSTR** (**B** asic **STR**) 是一种结构化数据类型，它包含字符串和字符串长度。 COM 提供分配、操作和释放 **BSTR** 的方法。  
  
 **_Bstr_t** 类封装并管理 **bstr** 数据类型。  
  
 当 ADO API 引用显示某个方法或属性采用 **字符串** 值时，表示该值的形式为 **_bstr_t**。  
  
### <a name="casting-_variant_t-and-_bstr_t-classes"></a>强制转换 _variant_t 和 _bstr_t 类  
 通常不需要在操作的参数中显式编写 **_variant_t** 或 **_bstr_t** 。 如果 **_variant_t** 或 **_bstr_t** 类具有与参数的数据类型匹配的构造函数，则编译器将生成相应的 **_variant_t** 或 **_bstr_t**。  
  
 但是，如果参数不明确，即参数的数据类型匹配多个构造函数，则必须用适当的数据类型强制转换自变量，才能调用正确的构造函数。  
  
 例如， **Recordset：： Open** 方法的声明为：  
  
```cpp
    HRESULT Open (  
        const _variant_t & Source,  
        const _variant_t & ActiveConnection,  
        enum CursorTypeEnum CursorType,  
        enum LockTypeEnum LockType,  
        long Options );  
```
  
 `ActiveConnection`参数采用对 **_variant_t** 的引用，你可以将其编码为连接字符串或指向打开的 **连接** 对象的指针。  
  
 如果传递字符串（如 " `DSN=pubs;uid=MyUserName;pwd=MyPassword;` "）或指针（如 ""），则将隐式构造正确的 _variant_t `(IDispatch *) pConn` 。  
  
> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。  
  
 或者，您可以对包含指针（例如 ""）的 **_variant_t** 进行显式编码 `_variant_t((IDispatch *) pConn, true)` 。 强制转换 `(IDispatch *)` 使用采用指向 IUnknown 接口的指针的另一个构造函数解析多义性。  
  
 这是一种很重要的情况，虽然很少提到这一事实，但 ADO 却是一个 IDispatch 接口。 只要指向 ADO 对象的指针必须作为 **变量** 传递，则必须将该指针强制转换为指向 IDispatch 接口的指针。  
  
 最后一种情况下，将构造函数的第二个布尔参数（其可选的默认值）显式编码 `true` 。 此参数会导致 **变体** 构造函数调用其 **AddRef** ( # A1 方法，该方法可在 ado 方法或属性调用完成时，补偿 ado 自动调用 **_variant_t：： Release** ( # A3 方法。  
  
### <a name="safearray"></a>SafeArray  
 **SafeArray** 是一种结构化的数据类型，它包含其他数据类型的数组。 **SafeArray** 称为 *safe* ，因为它包含每个数组维度的边界相关信息，并且限制对这些边界内数组元素的访问。  
  
 当 ADO API 引用显示某个方法或属性采用或返回一个数组时，这意味着该方法或属性采用或返回一个 **SafeArray**，而不是本机 C/c + + 数组。  
  
 例如， **连接** 对象 **OpenSchema** 方法的第二个参数需要 **可变** 值的数组。 这些 **变量** 值必须作为 **SafeArray** 的元素进行传递，而该 **safearray** 必须设置为另一种 **变体** 的值。 这是另一个作为 **OpenSchema** 的第二个参数传递的 **变体**。  
  
 作为进一步的示例， **Find** 方法的第一个参数是一个值为一维 **SafeArray** 的 **变量**;**AddNew** 的每个可选的第一个和第二个自变量都是一维的 **SafeArray**;**GetRows** 方法的返回值是一个 **变量**，其值为二维 **SafeArray**。  
  
## <a name="missing-and-default-parameters"></a>缺少参数和默认参数  
 Visual Basic 允许方法中缺少参数。 例如， **Recordset** 对象 **Open** 方法具有五个参数，但您可以跳过中间参数并保留尾部参数。 根据缺少的操作数的数据类型，将替换默认 **BSTR** 或 **变体** 。  
  
 在 C/c + + 中，必须指定所有操作数。 如果要指定一个缺少的参数，该参数的数据类型为字符串，请指定包含空字符串的 **_bstr_t** 。 如果要指定缺少其数据类型为 **Variant** 的参数，请指定值为 DISP_E_PARAMNOTFOUND 和 VT_ERROR 类型的 **_variant_t** 。 或者，指定 **#import** 指令提供的等效 **_variant_t** 常量 **vtMissing**。  
  
 三种方法是 **vtMissing** 典型用法的例外情况。 这些是 **连接** 和 **命令** 对象的 **执行** 方法，以及 **Recordset** 对象的 **NextRecordset** 方法。 下面是它们的签名：  
  
```cpp
_RecordsetPtr <A HREF="mdmthcnnexecute.htm">Execute</A>( _bstr_t CommandText, VARIANT * RecordsAffected,   
        long Options );  // Connection  
_RecordsetPtr <A HREF="mdmthcmdexecute.htm">Execute</A>( VARIANT * RecordsAffected, VARIANT * Parameters,   
        long Options );  // Command  
_RecordsetPtr <A HREF="mdmthnextrec.htm">NextRecordset</A>( VARIANT * RecordsAffected );  // Recordset  
```
  
 参数 *RecordsAffected* 和 *参数* 是指向 **变量** 的指针。 *参数* 是一个输入参数，该参数指定包含单个参数的 **变量** 的地址或参数数组，这些参数将修改所执行的命令。 *RecordsAffected* 是一个输出参数，它指定变量的地址，该 **变量** 的值由该方法所影响的行数。  
  
 在 **Command** object **Execute** 方法中，通过将 *参数* 设置为 `&vtMissing` (建议的) 或 null 指针 (为 Null 或零 (0) # A5 **，指示** 未指定任何参数。 如果将 *参数* 设置为 null 指针，则该方法在内部替换等效的 **vtMissing**，然后完成该操作。  
  
 在所有方法中，通过将 *RecordsAffected* 设置为 null 指针，指示不应返回受影响的记录数。 在这种情况下，null 指针不太多，因此缺少参数，这是因为方法应丢弃受影响的记录数。  
  
 因此，对于这三个方法，其编码方式如下：  
  
```cpp
pConnection->Execute("commandText", NULL, adCmdText);   
pCommand->Execute(NULL, NULL, adCmdText);  
pRecordset->NextRecordset(NULL);  
```
  
## <a name="error-handling"></a>错误处理  
 在 COM 中，大多数操作都返回一个 HRESULT 返回代码，该代码指示函数是否已成功完成。 **#Import** 指令围绕每个 "原始" 方法或属性生成包装代码，并检查返回的 HRESULT。 如果 HRESULT 指示失败，则包装代码会调用 _com_issue_errorex ( # A1，并使用 HRESULT 返回代码作为参数，从而引发 COM 错误。 COM 错误对象可以在 **try** - **catch** 块中捕获。  (为提高效率，请捕获对 **_com_error** 对象的引用 )   
  
 请记住，这些是 ADO 错误：它们是由于 ADO 操作失败导致的。 基础提供程序返回的错误显示为 **连接** 对象 **错误** 集合中的 **错误** 对象。  
  
 **#Import** 指令只为在 ADO 中声明的方法和属性创建错误处理例程。 但是，您可以通过编写自己的错误检查宏或内联函数来利用这一相同的错误处理机制。 有关示例，请参阅主题 [Visual C++ 扩展](./visual-c-extensions-for-ado.md)或以下部分中的代码。  
  
## <a name="visual-c-equivalents-of-visual-basic-conventions"></a>Visual Basic 约定 Visual C++ 等效项  
 下面概述了 ADO 文档中的多个约定，在 Visual Basic 中编码，以及它们在 Visual C++ 中的等效项。  
  
### <a name="declaring-an-ado-object"></a>声明 ADO 对象  
 在 Visual Basic 中，在这种情况下，将为 **记录集** 对象 (ADO 对象变量) 声明如下：  
  
```vb
Dim rst As ADODB.Recordset  
```
  
 子句 " `ADODB.Recordset` " 是在注册表中定义的 **Recordset** 对象的 ProgID。 **记录** 对象的新实例声明如下：  
  
```vb
Dim rst As New ADODB.Recordset  
```
  
 或  
  
```vb
Dim rst As ADODB.Recordset  
Set rst = New ADODB.Recordset  
```
  
 在 Visual C++ 中， **#import** 指令生成所有 ADO 对象的智能指针类型声明。 例如，指向 **_Recordset** 对象的变量为类型 **_RecordsetPtr**，并按如下所示声明：  
  
```cpp
_RecordsetPtr  rs;  
```
  
 指向 **_Recordset** 对象的新实例的变量声明如下：  
  
```cpp
_RecordsetPtr  rs("ADODB.Recordset");  
```
  
 - 或 -  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance("ADODB.Recordset");  
```
  
 - 或 -  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance(__uuidof(_Recordset));  
```
  
 调用 **CreateInstance** 方法后，可以按如下所示使用变量：  
  
```cpp
rs->Open(...);  
```
  
 请注意，在一种情况下，将 `.` 使用 "" 运算符，就像变量是类的实例 (`rs.CreateInstance`) ，而在另一种情况下， `->` 使用 "" 运算符，就像变量是指向接口 () 的指针一样 `rs->Open` 。  
  
 可以通过两种方式使用一个变量，因为 " `->` " 运算符会重载，以允许类的实例的行为类似于指向接口的指针。 实例变量的私有类成员包含指向 **_Recordset** 接口的指针;" `->` " 运算符返回该指针; 返回的指针访问 **_Recordset** 对象的成员。  
  
### <a name="coding-a-missing-parameter---string"></a>编码缺少的参数字符串  
 如果需要在 Visual Basic 中编写缺少的 **字符串** 操作数的代码，则只需省略操作数。 必须在 Visual C++ 中指定操作数。 将包含空字符串的 **_bstr_t** 编码为值。  
  
```cpp
_bstr_t strMissing(L"");  
```
  
### <a name="coding-a-missing-parameter---variant"></a>编码缺少参数变量  
 如果需要在 Visual Basic 中对缺少的 **变体** 操作数进行编码，只需忽略操作数。 您必须指定 Visual C++ 中的所有操作数。 使用将 **_variant_t** 设置为特殊值、DISP_E_PARAMNOTFOUND 和类型 VT_ERROR 来编写缺少的 **变体** 参数。 或者，指定 **vtMissing**，它是由 **#import** 指令提供的等效预定义常量。  
  
```cpp
_variant_t  vtMissingYours(DISP_E_PARAMNOTFOUND, VT_ERROR);   
```
  
 -或使用-  
  
```cpp
...vtMissing...;  
```
  
### <a name="declaring-a-variant"></a>声明变量  
 在 Visual Basic 中，用 **Dim** 语句声明 **变量**，如下所示：  
  
```vb
Dim VariableName As Variant  
```
  
 在 Visual C++ 中，将变量声明为 **_variant_t** 类型。 下面显示了几个示意 **_variant_t** 声明。  
  
> [!NOTE]
>  这些声明只是大致了解你在自己的程序中编写代码的内容。 有关详细信息，请参阅下面的示例和 Visual C + + 文档。  
  
```cpp
_variant_t  VariableName(value);  
_variant_t  VariableName((data type cast) value);  
_variant_t  VariableName(value, VT_DATATYPE);  
_variant_t  VariableName(interface * value, bool fAddRef = true);  
```
  
### <a name="using-arrays-of-variants"></a>使用变体数组  
 在 Visual Basic 中，可以使用 **Dim** 语句对 **变量** 数组进行编码，也可以使用 **Array** 函数，如下面的示例代码所示：  
  
```vb
Public Sub ArrayOfVariants  
Dim cn As ADODB.Connection  
Dim rs As ADODB.Recordset  
Dim fld As ADODB.Field  
  
    cn.Open "DSN=pubs"  
    rs = cn.OpenSchema(adSchemaColumns, _  
        Array(Empty, Empty, "authors", Empty))  
    For Each fld in rs.Fields  
        Debug.Print "Name = "; fld.Name  
    Next fld  
    rs.Close  
    cn.Close  
End Sub  
```
  
 下面的 Visual C++ 示例演示如何使用用于 **_variant_t** 的 **SafeArray** 。  
  
#### <a name="notes"></a>注释  
 以下说明对应于代码示例中的注释部分。  
  
1.  同样，TESTHR ( # A1 内联函数被定义为利用现有的错误处理机制。  
  
2.  你只需要一维数组，因此你可以使用 **SafeArrayCreateVector**，而不是常规用途 **SAFEARRAYBOUND** 声明和 **SafeArrayCreate** 函数。 下面是使用 **SafeArrayCreate** 时代码的外观：  
  
    ```cpp
       SAFEARRAYBOUND   sabound[1];  
       sabound[0].lLbound = 0;  
       sabound[0].cElements = 4;  
       pSa = SafeArrayCreate(VT_VARIANT, 1, sabound);  
    ```
  
3.  枚举常量 **adSchemaColumns** 标识的架构与四个约束列关联： TABLE_CATALOG、TABLE_SCHEMA、TABLE_NAME 和 COLUMN_NAME。 因此，将创建一个具有四个元素的 **变量** 值的数组。 然后，指定与第三列 TABLE_NAME 对应的约束值。  
  
     返回的 **记录集** 由多个列组成，其中的一个子集是约束列。 每个返回行的约束列的值必须与相应的约束值相同。  
  
4.  熟悉 **safearray** 的人可能会惊讶的是，在退出前不会调用 **SafeArrayDestroy** ( # A1。 事实上，在这种情况下，调用 **SafeArrayDestroy** ( # A1 将导致运行时异常。 原因是 `vtCriteria` 当 **_variant_t** 超出范围时，的析构函数将调用 **VariantClear** ( # A1，这将释放 **SafeArray**。 调用 **SafeArrayDestroy** 时，如果不手动清除 **_variant_t**，将导致析构函数尝试清除无效的 **SafeArray** 指针。  
  
     如果调用了 **SafeArrayDestroy** ，则代码将如下所示：  
  
    ```cpp
          TESTHR(SafeArrayDestroy(pSa));  
       vtCriteria.vt = VT_EMPTY;  
          vtCriteria.parray = NULL;  
    ```
  
     但是，让 **_variant_t** 管理 **SafeArray** 要简单得多。  
  
```cpp
// Visual_CPP_ADO_Prog_1.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Note 1  
inline void TESTHR( HRESULT _hr ) {   
   if FAILED(_hr)   
      _com_issue_error(_hr);   
}  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _RecordsetPtr pRs("ADODB.Recordset");  
      _ConnectionPtr pCn("ADODB.Connection");  
      _variant_t vtTableName("authors"), vtCriteria;  
      long ix[1];  
      SAFEARRAY *pSa = NULL;  
  
      pCn->Provider = "sqloledb";  
      pCn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
      // Note 2, Note 3  
      pSa = SafeArrayCreateVector(VT_VARIANT, 1, 4);  
      if (!pSa)   
         _com_issue_error(E_OUTOFMEMORY);  
  
      // Specify TABLE_NAME in the third array element (index of 2).   
      ix[0] = 2;        
      TESTHR(SafeArrayPutElement(pSa, ix, &vtTableName));  
  
      // There is no Variant constructor for a SafeArray, so manually set the   
      // type (SafeArray of Variant) and value (pointer to a SafeArray).  
  
      vtCriteria.vt = VT_ARRAY | VT_VARIANT;  
      vtCriteria.parray = pSa;  
  
      pRs = pCn->OpenSchema(adSchemaColumns, vtCriteria, vtMissing);  
  
      long limit = pRs->GetFields()->Count;  
      for ( long x = 0 ; x < limit ; x++ )  
         printf( "%d: %s\n", x + 1, ((char*) pRs->GetFields()->Item[x]->Name) );  
      // Note 4  
      pRs->Close();  
      pCn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Error:\n");  
      printf("Code = %08lx\n", e.Error());  
      printf("Code meaning = %s\n", (char*) e.ErrorMessage());  
      printf("Source = %s\n", (char*) e.Source());  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   CoUninitialize();  
}  
```
  
### <a name="using-property-getputputref"></a>使用属性 Get/Put/PutRef  
 在 Visual Basic 中，如果属性的名称被检索、赋值或分配了引用，则不会对其进行限定。  
  
```vb
Public Sub GetPutPutRef  
Dim rs As New ADODB.Recordset  
Dim cn As New ADODB.Connection  
Dim sz as Integer  
cn.Open "Provider=sqloledb;Data Source=yourserver;" & _  
         "Initial Catalog=pubs;Integrated Security=SSPI;"  
rs.PageSize = 10  
sz = rs.PageSize  
rs.ActiveConnection = cn  
rs.Open "authors",,adOpenStatic  
' ...  
rs.Close  
cn.Close  
End Sub  
```
  
 此 Visual C++ 示例演示了 **Get** / **Put** / **PutRef**_属性_。  
  
#### <a name="notes"></a>注释  
 以下说明对应于代码示例中的注释部分。  
  
1.  此示例使用两种形式的缺少字符串参数：显式常量、 **strMissing** 和字符串，编译器将使用该字符串来创建在 **Open** 方法范围内将存在的临时 **_bstr_t** 。  
  
2.  不需要将的操作数转换 `rs->PutRefActiveConnection(cn)` 为， `(IDispatch *)` 因为操作数的类型已经存在 `(IDispatch *)` 。  
  
```cpp
// Visual_CPP_ado_prog_2.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _bstr_t strMissing(L"");  
      long oldPgSz = 0, newPgSz = 5;  
  
      // Note 1  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", strMissing, "", adConnectUnspecified);  
  
      oldPgSz = rs->GetPageSize();  
      // -or-  
      // oldPgSz = rs->PageSize;  
  
      rs->PutPageSize(newPgSz);  
      // -or-  
      // rs->PageSize = newPgSz;  
  
      // Note 2  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockReadOnly, adCmdTable);  
      printf("Original pagesize = %d, new pagesize = %d\n", oldPgSz, rs->GetPageSize());  
      rs->Close();  
      cn->Close();  
  
   }  
   catch (_com_error &e) {  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="using-getitemx-and-itemx"></a>使用 GetItem (x) 和 Item [x]  
 此 Visual Basic 示例演示了 **Item** ( # A1 的标准和替代语法。  
  
```vb
Public Sub GetItemItem  
Dim rs As New ADODB.Recordset  
Dim name as String  
rs = rs.Open "authors", "DSN=pubs;", adOpenDynamic, _  
         adLockBatchOptimistic, adTable  
name = rs(0)  
' -or-  
name = rs.Fields.Item(0)  
rs(0) = "Test"  
rs.UpdateBatch  
' Restore name  
rs(0) = name  
rs.UpdateBatch  
rs.Close  
End Sub  
```
  
 此 Visual C++ 示例演示 **项**。  
  
> [!NOTE]
>  以下注释对应于代码示例中的注释部分：当使用 **Item** 访问集合时，必须将索引 **2** 转换为 **long** ，以便调用适当的构造函数。  
  
```cpp
// Visual_CPP_ado_prog_3.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
void main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _variant_t vtFirstName;  
  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", "", "", adConnectUnspecified);  
  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockOptimistic, adCmdTable);  
      rs->MoveFirst();  
  
      // Note 1. Get a field.  
      vtFirstName = rs->Fields->GetItem((long)2)->GetValue();  
      // -or-  
      vtFirstName = rs->Fields->Item[(long)2]->Value;  
  
      printf( "First name = '%s'\n", (char*)( (_bstr_t)vtFirstName) );  
  
      rs->Fields->GetItem((long)2)->Value = L"TEST";  
      rs->Update(vtMissing, vtMissing);  
  
      // Restore name  
      rs->Fields->GetItem((long)2)->PutValue(vtFirstName);  
      // -or-  
      rs->Fields->GetItem((long)2)->Value = vtFirstName;  
      rs->Update(vtMissing, vtMissing);  
      rs->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="casting-ado-object-pointers-with-idispatch-"></a>将 ADO 对象指针转换 (IDispatch * )   
 下面的 Visual C++ 示例演示如何使用 (IDispatch * ) 来强制转换 ADO 对象指针。  
  
#### <a name="notes"></a>注释  
 以下说明对应于代码示例中的注释部分。  
  
1.  在显式编码的 **变量** 中指定打开的 **连接** 对象。 将其转换为 (IDispatch \*) ，以便调用正确的构造函数。 此外，请将第二个 **_variant_t** 参数显式设置为默认值 **true**，以便在 **Recordset：： Open** 操作结束时，对象引用计数将是正确的。  
  
2.  表达式不是 `(_bstr_t)` 强制转换，而是一个 **_variant_t** 运算符，用于从 **值** 返回的 **变量** 中提取 **_bstr_t** 字符串。  
  
 表达式不是 `(char*)` 强制转换，而是一个 **_bstr_t** 运算符，它提取指向 **_bstr_t** 对象中封装的字符串的指针。  
  
 此代码段演示 **_variant_t** 和 **_bstr_t** 运算符的一些有用行为。  
  
```cpp
// Visual_CPP_ado_prog_4.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr pConn("ADODB.Connection");  
      _RecordsetPtr pRst("ADODB.Recordset");  
  
      pConn->Provider = "sqloledb";  
      pConn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
  
      // Note 1.  
      pRst->Open("authors", _variant_t((IDispatch *) pConn, true), adOpenStatic, adLockReadOnly, adCmdTable);  
      pRst->MoveLast();  
  
      // Note 2.  
      printf("Last name is '%s %s'\n",   
         (char*) ((_bstr_t) pRst->GetFields()->GetItem("au_fname")->GetValue()),  
         (char*) ((_bstr_t) pRst->Fields->Item["au_lname"]->Value));  
  
      pRst->Close();  
      pConn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }     
   ::CoUninitialize();  
}  
```