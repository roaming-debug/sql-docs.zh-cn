---
description: Visual C++ 扩展示例
title: Visual C++ 扩展示例 |Microsoft Docs
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
- Visual C++ [ADO], VC++ extensions example
ms.assetid: 9739c278-582c-402b-a158-7f68a1b2c293
author: rothja
ms.author: jroth
ms.openlocfilehash: 2c87889a1d5812bb9ede48d7cf19e6479cc018cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028642"
---
# <a name="visual-c-extensions-example"></a>Visual C++ 扩展示例
此程序说明如何从字段检索值并将其转换为 C/c + + 变量。  
  
 此示例还利用 "智能指针"，它自动处理 `QueryInterface` **IADORecordBinding** 接口调用和引用计数的特定于 COM 的详细信息。  
  
 如果不包含智能指针，可以编写代码：  
  
```cpp
IADORecordBinding   *picRs = NULL;  
...  
TESTHR(pRs->QueryInterface(  
          __uuidof(IADORecordBinding), (LPVOID*)&picRs));  
...  
if (picRs) picRs->Release();  
```  
  
 通过智能指针，可以通过 `IADORecordBindingPtr` `IADORecordBinding` 以下语句从接口派生类型：  
  
```cpp
_COM_SMARTPTR_TYPEDEF(IADORecordBinding, __uuidof(IADORecordBinding));  
```  
  
 并实例化指针，如下所示：  
  
```cpp
IADORecordBindingPtr picRs(pRs);  
```  
  
 由于 Visual C++ 扩展由 **Recordset** 对象实现，智能指针的构造函数 `picRs` 采用 _ `RecordsetPtr` 指针 `pRs` 。 构造函数 `QueryInterface` 通过调用 `pRs` 来查找 `IADORecordBinding` 接口。  
  
```cpp
// Visual_Cpp_Extensions_Example.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <icrsint.h>  
_COM_SMARTPTR_TYPEDEF(IADORecordBinding, __uuidof(IADORecordBinding));  
  
inline void TESTHR(HRESULT _hr) { if FAILED(_hr) _com_issue_error(_hr); }  
  
class CCustomRs : public CADORecordBinding {  
   BEGIN_ADO_BINDING(CCustomRs)  
      ADO_VARIABLE_LENGTH_ENTRY2(2, adVarChar, m_ch_fname, sizeof(m_ch_fname), m_ul_fnameStatus, false)  
      ADO_VARIABLE_LENGTH_ENTRY2(4, adVarChar, m_ch_lname, sizeof(m_ch_lname), m_ul_lnameStatus, false)  
   END_ADO_BINDING()  
  
public:  
   CHAR m_ch_fname[22];  
   CHAR m_ch_lname[32];  
   ULONG m_ul_fnameStatus;  
   ULONG m_ul_lnameStatus;  
};  
  
int main() {  
   ::CoInitialize(NULL);  
   try {  
      _RecordsetPtr pRs("ADODB.Recordset");  
      CCustomRs rs;  
      IADORecordBindingPtr picRs(pRs);  
  
      pRs->Open(L"SELECT * FROM Employee ORDER BY lname", L"dsn=DataPubs;Trusted_Connection=yes;", adOpenStatic, adLockOptimistic, adCmdText);  
  
      TESTHR(picRs->BindToRecordset(&rs));  
  
      while (!pRs->EndOfFile) {  
         // Process data in the CCustomRs C++ instance variables.  
         printf("Name = %s %s\n",  
            (rs.m_ul_fnameStatus == adFldOK ? rs.m_ch_fname: "<Error>"),   
            (rs.m_ul_lnameStatus == adFldOK ? rs.m_ch_lname: "<Error>") );  
  
         // Move to the next row of the Recordset.   Fields in the new row will   
         // automatically be placed in the CCustomRs C++ instance variables.  
  
         pRs->MoveNext();  
      }  
   }  
   catch (_com_error &e ) {  
      printf("Error:\n");  
      printf("Code = %08lx\n", e.Error());  
      printf("Meaning = %s\n", e.ErrorMessage());  
      printf("Source = %s\n", (LPCSTR) e.Source());  
      printf("Description = %s\n", (LPCSTR) e.Description());  
   }  
   ::CoUninitialize();  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 Visual C++ 扩展](./using-visual-c-extensions.md)   
 [Visual C++ 扩展标头](./visual-c-extensions-header.md)