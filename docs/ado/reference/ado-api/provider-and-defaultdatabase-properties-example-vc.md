---
description: Provider 和 DefaultDatabase 属性示例 (VC++)
title: 提供程序和 DefaultDatabase 属性示例 (VC + +) |Microsoft Docs
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
- provider property [ADO], VC++ example
- DefaultDatabase property [ADO], VC++ example
ms.assetid: d9868c99-425a-4b10-af67-1929ed513fda
author: rothja
ms.author: jroth
ms.openlocfilehash: 13071d81285ec75afb0713e1d24a452778e5bebb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040837"
---
# <a name="provider-and-defaultdatabase-properties-example-vc"></a>Provider 和 DefaultDatabase 属性示例 (VC++)
此示例通过使用不同的提供程序打开三个[连接](./connection-object-ado.md)对象来演示[提供程序](./provider-property-ado.md)的属性。 它还使用 [DefaultDatabase](./defaultdatabase-property.md) 属性设置 Microsoft ODBC 提供程序的默认数据库。  
  
```  
// Provider_and_DefaultDatabase_Properties.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void ProviderX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if (FAILED(::CoInitialize(NULL)))  
      return -1;  
  
   ProviderX();  
   ::CoUninitialize();  
}  
  
void ProviderX() {  
   HRESULT hr = S_OK;  
  
   // Define ADO object pointers.  Initialize pointers on define. These are in the ADODB::  namespace.  
   _ConnectionPtr pConnection1 = NULL;  
   _ConnectionPtr pConnection2 = NULL;  
   _ConnectionPtr pConnection3 = NULL;  
  
   try {  
      // Open a Connection using the Microsoft ODBC provider.  
      TESTHR(pConnection1.CreateInstance(__uuidof(Connection)));  
      pConnection1->ConnectionString = "DSN=Data;user id='MyUserId';password='MyPassword';";  
      pConnection1->Open("", "", "", adConnectUnspecified);  
      pConnection1->DefaultDatabase = "pubs";  
      // Display the provider  
      printf("\n\nConnection1 provider: %s \n\n", (LPCSTR)pConnection1->Provider);  
  
      // Open a connection using the OLE DB Provider for Microsoft Jet.  
      TESTHR(pConnection2.CreateInstance(__uuidof(Connection)));  
      pConnection2->Provider = "Microsoft.Jet.OLEDB.4.0";  
  
      char *sConn = "c:\\Northwind.mdb";  
      pConnection2->Open(sConn, "admin", "", NULL);  
      // Display the provider  
      printf("Connection2 provider: %s \n\n",(LPCSTR)pConnection2->Provider);  
  
      // Requires SQL Server authentication  
      // Open a Connection using the Microsoft SQL Server provider.  
      TESTHR(pConnection3.CreateInstance(__uuidof(Connection)));  
      pConnection3->Provider = "sqloledb";  
      pConnection3->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", NULL);  
      // Display the provider.  
      printf("Connection3 provider: %s\n\n", (LPCSTR)pConnection3->Provider);  
   }  
  
   catch (_com_error &e) {  
      // Notify the user of errors if any.  
      PrintProviderError(pConnection1);  
      if (pConnection2)  
         PrintProviderError(pConnection2);  
  
      if (pConnection3)   
         PrintProviderError(pConnection3);  
  
      PrintComError(e);  
   }  
  
   if (pConnection1)  
      if (pConnection1->State == adStateOpen)  
         pConnection1->Close();  
  
   if (pConnection2)  
      if (pConnection2->State == adStateOpen)  
         pConnection2->Close();  
  
   if (pConnection3)  
      if (pConnection3->State == adStateOpen)  
         pConnection3->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0) {  
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
  
   // Print COM errors.   
   printf("Error\n");  
   printf("\tCode = %08lx\n", e.Error());  
   printf("\tCode meaning = %s\n", e.ErrorMessage());  
   printf("\tSource = %s\n", (LPCSTR) bstrSource);  
   printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [ADO) 的连接对象 (](./connection-object-ado.md)   
 [DefaultDatabase 属性](./defaultdatabase-property.md)   
 [Provider 属性 (ADO)](./provider-property-ado.md)