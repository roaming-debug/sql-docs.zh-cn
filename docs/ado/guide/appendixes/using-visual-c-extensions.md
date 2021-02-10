---
description: 使用 Visual C++ 扩展
title: 使用 Visual C++ 扩展 |Microsoft Docs
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
- Visual C++ [ADO], using VC++ extensions
- ADO, Visual C++
ms.assetid: ff759185-df41-4507-8d12-0921894ffbd9
author: rothja
ms.author: jroth
ms.openlocfilehash: b5acd1b06bbaad8476ac48261ba49a6ca90cb0b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028652"
---
# <a name="visual-c-extensions"></a>Visual C++ 扩展
## <a name="the-iadorecordbinding-interface"></a>IADORecordBinding 接口
 ADO 的 Microsoft Visual C++ 扩展将 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象的字段关联或绑定到 c/c + + 变量。 只要绑定 **记录集** 的当前行发生更改， **记录集中** 的所有绑定字段就会复制到 c/c + + 变量。 如有必要，将复制的数据转换为 C/c + + 变量的已声明数据类型。

 **IADORecordBinding** 接口的 **BindToRecordset** 方法将字段绑定到 c/c + + 变量。 **AddNew** 方法将一个新行添加到绑定 **记录集**。 **Update** 方法用 C/c + + 变量的值，在 **记录集** 的新行中填充字段，或者更新现有行中的字段。

 **IADORecordBinding** 接口由 **Recordset** 对象实现。 不会自行为实现编码。

## <a name="binding-entries"></a>绑定项
 ADO 的 Visual C++ 扩展将 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象的字段映射到 c/c + + 变量。 字段与变量之间的映射定义称为 *绑定项*。 宏为数值、固定长度和可变长度数据提供绑定项。 绑定项和 C/c + + 变量是在派生自 Visual C++ Extension 类 **CADORecordBinding** 的类中声明的。 **CADORecordBinding** 类由绑定项宏在内部定义。

 ADO 将这些宏中的参数映射到 OLE DB **DBBINDING** 结构，并创建 OLE DB **访问器** 对象，以管理字段与变量之间的数据移动和转换。 OLE DB 定义由三部分组成的数据：存储数据的 *缓冲区* ;指示字段是否已成功存储在缓冲区中的 *状态* ，或者如何将变量还原到字段;数据的 *长度* 。 有关详细信息，请参阅在 OLE DB 程序员参考中 [获取并设置数据 (OLE DB) ](/previous-versions/windows/desktop/ms713700(v=vs.85)) (。 ) 

## <a name="header-file"></a>标头文件
 在应用程序中包含以下文件，以便使用 ADO 的 Visual C++ 扩展：

```cpp
#include <icrsint.h>
```

## <a name="binding-recordset-fields"></a>绑定记录集字段

#### <a name="to-bind-recordset-fields-to-cc-variables"></a>将记录集字段绑定到 C/c + + 变量

1.  创建一个派生自 **CADORecordBinding** 类的类。

2.  在派生类中指定绑定项和相应的 C/c + + 变量。 将绑定项括在 **BEGIN_ADO_BINDING** 和 **END_ADO_BINDING** 宏之间。 不要用逗号或分号终止宏。 每个宏自动指定相应的分隔符。

     为每个要映射到 C/c + + 变量的字段指定一个绑定项。 使用 **ADO_FIXED_LENGTH_ENTRY**、 **ADO_NUMERIC_ENTRY** 或 **ADO_VARIABLE_LENGTH_ENTRY** 的宏系列中的相应成员。

3.  在应用程序中，创建从 **CADORecordBinding** 派生的类的实例。 获取 **记录集** 的 **IADORecordBinding** 接口。 然后调用 **BindToRecordset** 方法，将 **记录集** 字段绑定到 c/c + + 变量。

 有关详细信息，请参阅 [Visual C++ 扩展示例](./visual-c-extensions-example.md)。

## <a name="interface-methods"></a>接口方法
 **IADORecordBinding** 接口有三种方法： **BindToRecordset**、 **AddNew** 和 **Update**。 每个方法的唯一参数是指向派生自 **CADORecordBinding** 的类的实例的指针。 因此， **AddNew** 和 **Update** 方法不能指定其 ADO 方法 namesakes 的任何参数。

## <a name="syntax"></a>语法
 **BindToRecordset** 方法将 **记录集** 字段与 c/c + + 变量关联。

```cpp
BindToRecordset(CADORecordBinding *binding)
```

 **AddNew** 方法会调用其 NAMESAKE （ADO [AddNew](../../reference/ado-api/addnew-method-ado.md)方法）向 **记录集** 添加新行。

```cpp
AddNew(CADORecordBinding *binding)
```

 **Update** 方法会调用其 NAMESAKE （ADO [Update](../../reference/ado-api/update-method.md)方法）来更新 **记录集**。

```cpp
Update(CADORecordBinding *binding)
```

## <a name="binding-entry-macros"></a>绑定项宏
 绑定项宏定义 **记录集** 字段和变量的关联。 开始和结束宏分隔绑定项集。

 为固定长度的数据（如 **adDate** 或 **adBoolean**）提供了宏家族。数值数据，如 **adTinyInt**、 **adInteger** 或 **adDouble**;和可变长度数据，如 **adChar**、 **adVarChar** 或 **adVarBinary**。 除 **adVarNumeric** 以外的所有数字类型也是固定长度的类型。 每个系列都有不同的参数集，这样您就可以排除不感兴趣的绑定信息。

 有关详细信息，请参阅 [附录 A：数据类型](/previous-versions/windows/desktop/ms723969(v=vs.85))，OLE DB 程序员参考。

### <a name="begin-binding-entries"></a>开始绑定项
  (*类* **BEGIN_ADO_BINDING**) 

### <a name="fixed-length-data"></a>Fixed-Length 数据
 **ADO_FIXED_LENGTH_ENTRY** (*序号、数据类型、缓冲区、状态、修改*) 

 **ADO_FIXED_LENGTH_ENTRY2** (*序号、数据类型、缓冲区、修改*) 

### <a name="numeric-data"></a>数值数据
 **ADO_NUMERIC_ENTRY** (*序号、数据类型、缓冲区、精度、小数位数、状态、修改*) 

 **ADO_NUMERIC_ENTRY2** (*序号、数据类型、缓冲区、精度、小数位数、修改*) 

### <a name="variable-length-data"></a>Variable-Length 数据
 **ADO_VARIABLE_LENGTH_ENTRY** (*序号、数据类型、缓冲区、大小、状态、长度、修改*) 

 **ADO_VARIABLE_LENGTH_ENTRY2** (*序号、数据类型、缓冲区、大小、状态、修改*) 

 **ADO_VARIABLE_LENGTH_ENTRY3** (*序号、数据类型、缓冲区、大小、长度、修改*) 

 **ADO_VARIABLE_LENGTH_ENTRY4** (*序号、数据类型、缓冲区、大小、修改*) 

### <a name="end-binding-entries"></a>结束绑定项
 **END_ADO_BINDING** ( # A1

|参数|说明|
|---------------|-----------------|
|*类*|定义绑定项和 C/c + + 变量的类。|
|*序号*|对应于 C/c + + 变量的 **记录集** 字段的序号（从1开始）。|
|*DataType*|C/c + + 变量的等效 ADO 数据类型 (有关) 的有效数据类型的列表，请参阅 [DataTypeEnum](../../reference/ado-api/datatypeenum.md) 。 如有必要， **记录集** 字段的值将转换为该数据类型。|
|*Buffer*|将存储 **记录集** 字段的 C/c + + 变量的名称。|
|*大小*|*缓冲区* 的最大大小（以字节为单位）。 如果 *Buffer* 包含可变长度字符串，则允许用于终止零的空间。|
|*Status*|变量的名称，该变量指示 *缓冲区* 的内容是否有效，以及字段是否成功转换为 *数据类型* 。<br /><br /> 此变量的两个最重要的值是 **adFldOK**，这意味着转换成功;和 **adFldNull**，这意味着该字段的值将是 VT_NULL 类型的变体，而不只是空的。<br /><br /> 下表 "Status Values" 中列出了可能的 *状态* 值。|
|*修改*|布尔标志;如果为 TRUE，则表示允许 ADO 使用 *Buffer* 中包含的值更新相应的 **记录集** 字段。<br /><br /> 将 "布尔 *修改* " 参数设置为 "TRUE" 以启用 ADO 以更新绑定字段，如果要检查字段而不进行更改，则设置为 FALSE。|
|*精度*|数值变量中可表示的位数。|
|缩放 |数值变量中的小数位数。|
|*时长*|包含 *缓冲区* 中数据的实际长度的四字节变量的名称。|

## <a name="status-values"></a>状态值
 *状态* 变量的值指示是否已成功将字段复制到变量。

 设置数据时，可以将 *Status* 设置为 **adFldNull** ，以指示 **记录集** 字段应设置为 null。

|返回的常量|Value|说明|
|--------------|-----------|-----------------|
|**adFldOK**|0|返回的字段值为非 null。|
|**adFldBadAccessor**|1|绑定无效。|
|**adFldCantConvertValue**|2|由于除符号不匹配或数据溢出之外的其他原因，无法转换值。|
|**adFldNull**|3|获取字段时，表示返回了 null 值。<br /><br /> 设置字段时，指示在字段不能将 **null** 本身编码 (例如，字符数组或) 的整数时，应将该字段设置为 **NULL** 。|
|**adFldTruncated**|4|长度可变的数据或数字已被截断。|
|**adFldSignMismatch**|5|值已签名，并且变量数据类型无符号。|
|**adFldDataOverFlow**|6|值大于可以存储在变量数据类型中。|
|**adFldCantCreate**|7|未知的列类型和字段已打开。|
|**adFldUnavailable**|8|无法确定字段值-例如，在没有默认值的新的、未分配的字段上。|
|**adFldPermissionDenied**|9|更新时，没有写入数据的权限。|
|**adFldIntegrityViolation**|10|更新时，字段值将违反列的完整性。|
|**adFldSchemaViolation**|11|更新时，字段值将违反列架构。|
|**adFldBadStatus**|12|更新时，状态参数无效。|
|**adFldDefault**|13|更新时，将使用默认值。|

## <a name="see-also"></a>另请参阅
 [Visual C++ 扩展示例](./visual-c-extensions-example.md) [Visual C++ extension 标头](./visual-c-extensions-header.md)