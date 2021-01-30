---
description: Type 属性示例（字段）(VC++)
title: Type 属性示例 (字段)  (VC + +) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- C++
helpviewer_keywords:
- Type property [field] [ADO], VC++ example
ms.assetid: 440dbdb1-16fc-4cfe-9451-59a153852537
author: rothja
ms.author: jroth
ms.openlocfilehash: d3f433434833ee07e4ebbf999e2ef2ca122f44fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170067"
---
# <a name="type-property-example-field-vc"></a>Type 属性示例（字段）(VC++)
此示例通过显示与 "*_Employees_*" 表中所有 [Field](./field-object.md)对象的 **Type** 属性值相对应的常量的名称，演示了 [type](./type-property-ado.md)属性。 运行此过程需要 FieldType 函数。  
  
## <a name="example"></a>示例  
  
```  
// BeginTypeFieldCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
  
void TypeX();  
_bstr_t FieldType(int intType);   
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   TypeX();  
   ::CoUninitialize();  
}  
  
void TypeX() {  
   // Define string variables.  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='(local)'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   // Define ADO object pointers, initialize pointers.  These are in the ADODB:: namespace.  
   _RecordsetPtr pRstEmployees = NULL;  
   FieldsPtr pFldLoop = NULL;  
  
   try {    
      // Open recordset with data from Employee table.  
      TESTHR(pRstEmployees.CreateInstance(__uuidof(Recordset)));  
      pRstEmployees->Open ("employee", strCnn, adOpenForwardOnly, adLockReadOnly, adCmdTable);  
  
      printf("Fields in Employee Table:\n\n");  
  
      // Enumerate the Fields collection of the Employees table.  
      pFldLoop = pRstEmployees->GetFields();  
      for (short int intFields = 0 ; intFields < (int)pFldLoop->GetCount() ; intFields++) {  
         _variant_t Index;  
         Index.vt = VT_I2;  
         Index.iVal = intFields;  
         printf("  Name: %s\n" , (LPCSTR) pFldLoop->GetItem(Index)->GetName());  
         printf("  Type: %s\n\n", (LPCTSTR)FieldType(pFldLoop->GetItem(Index)->Type));  
      }  
   }  
   catch (_com_error &e) {  
      // Display errors, if any.  Pass connection pointer accessed from the Recordset.  
      _variant_t vtConnect = pRstEmployees->GetActiveConnection();  
  
      // GetActiveConnection returns connect string if connection  
      // is not open, else returns Connection object.  
      switch(vtConnect.vt) {  
      case VT_BSTR:  
         PrintComError(e);  
         break;  
      case VT_DISPATCH:  
         PrintProviderError(vtConnect);  
         break;  
      default:  
         printf("Errors occured.");  
         break;  
      }  
   }  
  
   // Clean up objects before exit.  
   if (pRstEmployees)  
      if (pRstEmployees->State == adStateOpen)  
         pRstEmployees->Close();  
}  
  
_bstr_t FieldType(int intType) {  
   _bstr_t strType;   
   switch(intType) {  
   case adChar:  
      strType = "adChar";  
      break;  
   case adVarChar:  
      strType = "adVarChar";  
      break;  
   case adSmallInt:  
      strType = "adSmallInt";  
      break;  
   case adUnsignedTinyInt:  
      strType = "adUnsignedTinyInt";  
      break;  
   case adDBTimeStamp:  
      strType = "adDBTimeStamp";  
      break;  
   default:  
      break;  
   }  
   return strType;  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0 ) {  
      long nCount = pConnection->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("Error number: %x\t%s\n", pErr->Number, (LPCSTR) pErr->Description);  
      }  
   }  
}  
  
void PrintComError(_com_error &e) {  
   _bstr_t bstrSource(e.Source());  
   _bstr_t bstrDescription(e.Description());  
  
   // Print Com errors.    
   printf("Error\n");  
   printf("\tCode = %08lx\n", e.Error());  
   printf("\tCode meaning = %s\n", e.ErrorMessage());  
   printf("\tSource = %s\n", (LPCSTR) bstrSource);  
   printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
```  
  
 _ *员工表中的字段：**  
 **名称： emp_id**  
 **类型： adChar**  
 **名称： fname**  
 **类型： adVarChar**  
 **名称： minit**  
 **类型： adChar**  
 **名称： lname**  
 **类型： adVarChar**  
 **名称： job_id**  
 **类型： adSmallInt**  
 **名称： job_lvl**  
 **类型： adUnsignedTinyInt**  
 **名称： pub_id**  
 **类型： adChar**  
 **名称： hire_date**  
 **类型： adDBTimeStamp**   
## <a name="see-also"></a>另请参阅  
 [Field 对象](./field-object.md)   
 [Type 属性 (ADO)](./type-property-ado.md)