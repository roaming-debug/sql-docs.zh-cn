---
description: 连接的 Close 方法、表 Type 属性示例 (VC++)
title: 连接关闭方法，表类型属性示例 (VC + +) |Microsoft Docs
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
- Type property [ADOX], VC++ example
- Close method [ADOX], VC++ example
ms.assetid: d0e250aa-fc57-4fd3-9610-d64f50c5507f
author: rothja
ms.author: jroth
ms.openlocfilehash: 13e0f9831b89e69498868f02db32ed4a73c25159
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169446"
---
# <a name="connection-close-method-table-type-property-example-vc"></a>连接的 Close 方法、表 Type 属性示例 (VC++)
如果将 [ActiveConnection](./activeconnection-property-adox.md) 属性设置为 " **无** "，则应 "关闭" 目录。 关联的集合将为空。 从目录中的架构对象创建的任何对象都将是孤立对象。 那些已缓存的对象上的所有属性仍将可用，但尝试读取需要调用提供程序的属性会失败。  
  
```  
// BeginCloseConnectionCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" rename("EOF", "EndOfFile")  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void CloseConnectionByNothingX();  
void CloseConnectionX();  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL) ) )  
      return -1;  
  
   CloseConnectionByNothingX();  
   CloseConnectionX();  
  
   ::CoUninitialize();  
}  
  
void CloseConnectionByNothingX() {  
   HRESULT hr = S_OK;  
  
   // Define and initialize ADOX object pointers, in ADODB namespace.  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
  
   // Define ADODB object pointers  
   ADODB::_ConnectionPtr m_pCnn = NULL;  
  
   // Define other variables  
   _variant_t vIndex = (short) 0;  
  
   try {  
      TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
  
      m_pCnn->Open("Provider='Microsoft.JET.OLEDB.4.0';Data Source= 'c:\\Northwind.mdb';", "", "", NULL);  
      m_pCatalog->PutActiveConnection(_variant_t((IDispatch *)m_pCnn));  
      m_pTable = m_pCatalog->Tables->GetItem(vIndex);  
  
      // Cache m_pTable.Type info  
      cout << m_pTable->Type << endl;  
  
      _variant_t vCnn;  
      vCnn.vt = VT_DISPATCH;  
      vCnn.pdispVal = NULL;  
      m_pCatalog->PutActiveConnection(vCnn);  
  
      // m_pTable is orphaned, will succeed if this was cached  
      cout << m_pTable->Type << endl;  
  
      cout << m_pTable->Columns->GetItem(vIndex)->DefinedSize << endl;  
      // Previous line will fail if this info has not been cached  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\nError\n\tSource :  %s \n\tdescription : %s \n", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occurred in CloseConnectionByNothingX...." << endl;  
   }  
}  
  
void CloseConnectionX() {  
   HRESULT hr = S_OK;  
  
   // Define ADOX object pointers, initialize pointers. These are in ADODB  namespace.  
  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
  
   // Define ADODB object pointers  
   ADODB::_ConnectionPtr m_pCnn = NULL;  
  
   // Define other variables  
   _variant_t vIndex = (short) 0;  
   try {  
      TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
      m_pCnn->Open("Provider='Microsoft.JET.OLEDB.4.0';Data Source= 'c:\\Northwind.mdb';", "", "", NULL);  
  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
      m_pCatalog->PutActiveConnection(_variant_t((IDispatch *)m_pCnn));  
  
      m_pTable = m_pCatalog->Tables->GetItem(vIndex);  
  
      // Cache m_pTable.Type info  
      cout << m_pTable->Type << endl;  
  
      m_pCnn->Close();  
  
      // m_pTable is orphaned, will succeed if this was cached  
      cout << m_pTable->Type << endl;  
  
      cout << m_pTable->Columns->GetItem(vIndex)->DefinedSize << endl;  
      // Previous line will fail if this info has not been cached  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\nError\n\tSource :  %s \n\tdescription : %s \n", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occurred in CloseConnectionX...." << endl;  
   }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [ActiveConnection 属性 (ADOX)](./activeconnection-property-adox.md)