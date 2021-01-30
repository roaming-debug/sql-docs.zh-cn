---
description: GetObjectOwner 和 SetObjectOwner 方法示例 (VC++)
title: GetObjectOwner 和 SetObjectOwner 方法示例 (VC + +) |Microsoft Docs
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
- SetObjectOwner method [ADOX], VC++ example
- GetObjectOwner method [ADOX], VC++ example
ms.assetid: f5f2aa4b-d790-458f-9e70-1643e3e203b2
author: rothja
ms.author: jroth
ms.openlocfilehash: fb68ca4eb7cd8c1591ef52b399649dac7614b308
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169405"
---
# <a name="getobjectowner-and-setobjectowner-methods-example-vc"></a>GetObjectOwner 和 SetObjectOwner 方法示例 (VC++)
此示例演示 [GetObjectOwner](./getobjectowner-method-adox.md) 和 [SetObjectOwner](./setobjectowner-method.md) 方法。 此代码假定存在组记帐 (查看 [组和用户追加，ChangePassword 方法示例 (VC + +) ](./groups-and-users-append-changepassword-methods-example-vc.md) 查看如何将此组添加到系统) 。 "类别" 表的所有者设置为 "记帐"。  
  
```  
// BeginOwnersCpp.cpp  
// compile with: /EHsc  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   HRESULT hr = S_OK;  
  
   // Define and initialize ADOX object pointers. These are in the ADODB namespace.  
   _TablePtr m_pTable = NULL;  
   _CatalogPtr m_pCatalog = NULL;  
  
   try {  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
      TESTHR(hr = m_pTable.CreateInstance(__uuidof(Table)));  
  
      // Open the Catalog.  
      m_pCatalog->PutActiveConnection("Provider='Microsoft.JET.OLEDB.4.0';data source='c:\\Northwind.mdb';jet oledb:system database='c:\\system.mdw'");  
  
      // Print the original owner of Categories  
      _bstr_t strOwner = m_pCatalog->GetObjectOwner("Categories", adPermObjTable);  
      cout << "Owner of Categories: " << strOwner << "\n" << endl;  
  
      //Create and append new group with a string.  
      m_pCatalog->Groups->Append("Accounting");  
  
      //Set the owner of Categories to Accounting.  
      m_pCatalog->SetObjectOwner("Categories", adPermObjTable, "Accounting");  
  
      _variant_t vIndex;  
      // List the owners of all tables and columns in the catalog.  
      for ( long iIndex = 0 ; iIndex < m_pCatalog->Tables->Count ; iIndex++ ) {  
         vIndex = iIndex;  
         m_pTable = m_pCatalog->Tables->GetItem(vIndex);  
         cout << "Table: " << m_pTable->Name << endl;  
         cout << "   Owner: " << m_pCatalog->GetObjectOwner(m_pTable->Name, adPermObjTable) << endl;  
      }  
  
      // Restore the original owner of Categories  
      m_pCatalog->SetObjectOwner("Categories", adPermObjTable, strOwner);  
  
      // Delete Accounting  
      m_pCatalog->Groups->Delete("Accounting");  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
  
   catch(...) {  
      cout << "Error occurred in include files...." << endl;  
   }  
   ::CoUninitialize();  
}  
```