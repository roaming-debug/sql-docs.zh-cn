---
description: Version 属性示例 (VC++)
title: " (VC + +) 的版本属性示例 |Microsoft Docs"
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
- Version property [ADO], VC++ example
ms.assetid: 2440b6ff-2536-497c-a5f4-41db0cf1945e
author: rothja
ms.author: jroth
ms.openlocfilehash: b73a1b7e68e6e020109163d5f385e62cddb5d061
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056142"
---
# <a name="version-property-example-vc"></a>Version 属性示例 (VC++)
此示例使用[连接](./connection-object-ado.md)对象的[Version](./version-property-ado.md)属性显示当前 ADO 版本。 它还使用几个动态属性来显示：  
  
-   当前 DBMS 名称和版本。  
  
-   OLE DB 版本。  
  
-   提供程序名称和版本。  
  
-   ODBC 版本。  
  
-   ODBC 驱动程序名称和版本。  
  
> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。  
  
```  
// BeginVersionCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void VersionX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
    if (FAILED(::CoInitialize(NULL)))  
        return -1;  
  
    VersionX();  
  
    ::CoUninitialize();  
}  
  
void VersionX() {  
    HRESULT hr = S_OK;  
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
    // Define ADO object pointers.  Initialize pointers on define.  
    // These are in the ADODB::  namespace.  
    _ConnectionPtr pConnection = NULL;  
  
    try {  
        // Open connection.  
        TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
        pConnection->Open (strCnn, "", "", adConnectUnspecified);  
  
        printf("ADO Version   : %s\n\n",(LPCSTR) pConnection->Version);  
        printf("DBMS Name   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("DBMS Name")->Value);  
        printf("DBMS Version   : %s\n\n",(LPCSTR) (_bstr_t)  
            pConnection->Properties->GetItem("DBMS Version")->Value);  
        printf("OLE DB Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("OLE DB Version")->Value);  
        printf("Provider Name   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Provider Name")->Value);  
        printf("Provider Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Provider Version")->Value);  
        printf("Driver Name   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Driver Name")->Value);  
        printf("Driver Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Driver Version")->Value);  
        printf("Driver ODBC Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Driver ODBC Version")->Value);  
  
    }  
  
    catch (_com_error &e) {  
        // Notify the user of errors if any.  
        PrintProviderError(pConnection);  
        PrintComError(e);  
    }  
  
    if (pConnection)  
        if (pConnection->State == adStateOpen)  
            pConnection->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
    // Print Provider Errors from Connection object.  
    // pErr is a record object in the Connection's Error collection.  
    ErrorPtr pErr = NULL;  
  
    if ( (pConnection->Errors->Count) > 0) {  
        long nCount = pConnection->Errors->Count;  
        // Collection ranges from 0 to nCount -1.  
        for ( long i = 0 ; i < nCount ; i++) {  
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
  
## <a name="see-also"></a>另请参阅  
 [ADO) 的连接对象 (](./connection-object-ado.md)   
 [Version 属性 (ADO)](./version-property-ado.md)