---
description: Handler 属性示例 (VC++)
title: " (VC + +) 的处理程序属性示例 |Microsoft Docs"
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
- Handler property [ADO], VC++ example
ms.assetid: d046d89c-622b-48bc-9d30-f454c3e13595
author: rothja
ms.author: jroth
ms.openlocfilehash: b42f254b54380b9b240f7231cf6bacf86510662b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049377"
---
# <a name="handler-property-example-vc"></a>Handler 属性示例 (VC++)
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 此示例演示了 [RDS DataControl](./datacontrol-object-rds.md) 对象 [处理程序](./handler-property-rds.md) 属性。 有关详细信息，请参阅 [DataFactory 自定义](../../guide/remote-data-service/datafactory-customization.md) 。 )  (  
  
 假定位于服务器上 Msdfmap.ini 的参数文件中的以下部分：  
  
```  
[connect AuthorDataBase]  
Access=ReadWrite  
Connect="DSN=Pubs"  
[sql AuthorById]  
SQL="SELECT * FROM Authors WHERE au_id = ?"  
```  
  
 你的代码如下所示。 分配给 [SQL](./sql-property.md) 属性的命令将与 ***AuthorById** _ 标识符匹配，并将为作者 Michael O'Leary 检索行。 尽管代码中的 [Connect](./connect-property-rds.md) 属性指定 Northwind 数据源，但该数据源将被 Msdfmap.ini _connect * 节覆盖。 将 **DataControl** 对象 [记录集](./recordset-sourcerecordset-properties-rds.md) 属性分配给完全作为编码便利的断开连接的 [记录集](../ado-api/recordset-object-ado.md) 对象。  
  
```  
// BeginHandlerCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
#import "msadco.dll"  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void HandlerX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   HRESULT hr = S_OK;  
   hr = ::CoInitialize(NULL);  
   if (SUCCEEDED(hr)) {  
      HandlerX();  
      ::CoUninitialize();  
   }  
}  
  
void HandlerX() {  
   HRESULT  hr = S_OK;  
   // Define and initialize ADO object pointers, in the ADODB namespace.     
   _RecordsetPtr pRst = NULL;  
  
   // Define RDS object pointers.  
   RDS::IBindMgrPtr dc;  
  
   try {  
      TESTHR(hr = dc.CreateInstance(__uuidof(RDS::DataControl)));  
      dc->Handler = "MSDFMAP.Handler";  
      dc->ExecuteOptions = 1;  
      dc->FetchOptions = 1;  
      dc->Server = "https://MyServer";  
      dc->Connect = "Data Source=AuthorDatabase";  
      dc->SQL = "AuthorById('267-41-2394')";  
  
      // Retrieve the record.  
      dc->Refresh();  
  
      // Use another Recordset as a convenience.  
      pRst = dc->GetRecordset();  
      printf("Author is %s %s",   
         (LPSTR)(_bstr_t) pRst->Fields->GetItem("au_fname")->Value,   
         (LPSTR)(_bstr_t) pRst->Fields->GetItem("au_lname")->Value);  
      pRst->Close();  
  
   }  // End Try statement.  
   catch (_com_error &e) {  
      PrintProviderError(pRst->GetActiveConnection());  
      PrintComError(e);  
   }  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
   long nCount = 0;  
   long i = 0;  
  
   if ( (pConnection->Errors->Count) > 0) {  
      nCount = pConnection->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for ( i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("\t Error number: %x\t%s", pErr->Number, pErr->Description);  
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
 [DataControl 对象 (RDS) ](./datacontrol-object-rds.md)   
 [Handler 属性 (RDS)](./handler-property-rds.md)