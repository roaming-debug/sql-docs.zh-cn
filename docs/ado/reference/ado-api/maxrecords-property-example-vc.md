---
description: MaxRecords 属性示例 (VC++)
title: MaxRecords 属性示例 (VC + +) |Microsoft Docs
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
- MaxRecords property [ADO], VC++ example
ms.assetid: af6b399b-e546-4de5-9cd1-5a6e0ec7ddc7
author: rothja
ms.author: jroth
ms.openlocfilehash: 8f74ae0e10c13dd4cd972284139623ad865c6dee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043957"
---
# <a name="maxrecords-property-example-vc"></a>MaxRecords 属性示例 (VC++)
此示例使用 [MaxRecords](./maxrecords-property-ado.md) 属性打开一个 [记录集](./recordset-object-ado.md) ，其中包含 ***标题*** 表中的10个最昂贵的标题。  
  
## <a name="example"></a>示例  
  
```  
// MaxRecords_Property_Example.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF","EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
#include "icrsint.h"  
  
// This class extracts titles and type from the titles table  
class CTitleRs : public CADORecordBinding {  
   BEGIN_ADO_BINDING(CTitleRs)  
  
      // Column title is the 1st field in the Recordset  
      ADO_VARIABLE_LENGTH_ENTRY2(1, adVarChar,m_szau_Title,  
      sizeof(m_szau_Title), lau_TitleStatus, FALSE)  
  
      // Column price is the 2nd field in the Recordset  
      ADO_VARIABLE_LENGTH_ENTRY2(2, adDouble, m_szau_Price,  
      sizeof(m_szau_Price), lau_PriceStatus, FALSE)  
  
   END_ADO_BINDING()  
  
public:  
   CHAR m_szau_Title[81];  
   ULONG lau_TitleStatus;  
   DOUBLE m_szau_Price;  
   ULONG lau_PriceStatus;  
};  
  
// Function Declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void MaxRecordsX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   MaxRecordsX();  
   ::CoUninitialize();  
}  
  
void  MaxRecordsX() {  
   // Define ADO ObjectPointers.  Initialize Pointers on define  
   // These are in the ADODB :: namespace  
   _RecordsetPtr pRstTemp = NULL;  
  
   // Define Other Variables  
   IADORecordBinding *picRs = NULL;   // Interface Pointer Declared    
   CTitleRs titlers;   // C++ Class Object  
  
   try {  
      // Assign Connection String to Variable  
      _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
      // Open Recordset containing the 10 most expensive titles in the Titles table.  
      TESTHR(pRstTemp.CreateInstance(__uuidof(Recordset)));  
  
      pRstTemp->MaxRecords = 10;  
  
      pRstTemp->Open("SELECT title,price FROM Titles ORDER BY Price DESC",  
         strCnn, adOpenForwardOnly, adLockReadOnly, adCmdText);  
  
      // Open IADORecordBinding interface pointer for binding Recordset to a class  
      TESTHR(pRstTemp->QueryInterface(__uuidof(IADORecordBinding), (LPVOID*)&picRs));  
  
      // Bind the Recordset to a C++ class here  
      TESTHR(picRs->BindToRecordset(&titlers));  
  
      // Display the contents of the Recordset  
      printf("Top Ten Titles by Price:\n\n");  
  
      while ( !(pRstTemp->EndOfFile) ) {  
         printf("%s ---  %6.2lf\n", titlers.lau_TitleStatus == adFldOK ?   
            titlers.m_szau_Title : "<NULL>", titlers.lau_PriceStatus == adFldOK ?   
            titlers.m_szau_Price : 0.00);  
         pRstTemp->MoveNext();  
      }  
   }  
   catch(_com_error &e) {  
      // Display errors, if any.  Pass connection pointer accessed from the Recordset.  
      _variant_t vtConnect = pRstTemp->GetActiveConnection();  
  
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
  
   // Clean up objects before exit. Release the IADORecordset Interface here     
   if (picRs)  
      picRs->Release();  
  
   if (pRstTemp)  
      if (pRstTemp->State == adStateOpen)  
         pRstTemp->Close();  
};  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object  
   // pErr is a record object in the Connection's Error collection  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count)>0 ) {  
      long nCount = pConnection->Errors->Count;  
  
      // Collection ranges from 0 to nCount-1  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("\t Error Number :%x \t %s", pErr->Number, pErr->Description);  
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
  
 **按价格排列的十大标题：**  
**但用户是否友好？---22.95**  
**计算机善始善终和非善始善终个体：行为变化形式---21.59**  
**Onions、Leeks 和大蒜：地中海的密码---20.95**  
**硅低谷的机密---20.00**  
**繁忙的 Executive 数据库指南---19.99**  
**直接谈论---19.99 的计算机**  
**硅谷 Gastronomic---19.99**  
**长时间数据 Deprivation：四个案例研究---19.99**  
**寿司，任何人？---14.99**  
**50年 Buckingham Palace Kitchens---11.95**   
## <a name="see-also"></a>另请参阅  
 [MaxRecords 属性 (ADO) ](./maxrecords-property-ado.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)