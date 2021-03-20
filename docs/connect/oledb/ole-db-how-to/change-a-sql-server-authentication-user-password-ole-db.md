---
title: 更改 SQL Server 身份验证用户密码 (OLE DB) | Microsoft Docs
description: 通过此示例了解如何使用 OLE DB 更改使用 SQL Server 身份验证的用户帐户的密码。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2a64426fa3d1285f66be90dae8cfd2e028b4d62
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741727"
---
# <a name="change-a-sql-server-authentication-user-password-ole-db"></a>更改 SQL Server 身份验证用户密码 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  此示例显示如何使用 OLE DB 更改 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证下的用户帐户密码。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 crypto API](/windows/win32/seccrypto/cryptography-reference)（Win32 加密 API）加密它们。  
  
## <a name="example"></a>示例  
 在生成示例前，请更新 C++ 代码以指定用户 ID、旧密码和新密码。  
  
 此应用程序连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 在某些 Windows 操作系统上，您需要将 (localhost) 或 (local) 更改为您的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。 若要连接到命名实例，请将连接字符串从 L"(local)" 更改为 L"(local)\\\name"，其中 name 是命名实例。 默认情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Express 安装在命名实例中。 请确保 INCLUDE 环境变量包括含有 msoledbsql.h 的目录。  
  
 使用 ole32.lib 和 oleaut32.lib 进行编译。  
  
 若要生成此示例，您将需要一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证用户帐户并了解帐户密码。 若要允许在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证下登录，请打开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio，在对象资源管理器中右键单击“服务器”节点，然后选择“属性”。 选择“安全性”并启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 Windows 身份验证模式。 若要在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证下添加用户帐户，请在对象资源管理器中右键单击“安全性”节点，然后选择“添加”。  
  
 将用于运行此示例的服务器必须至少针对一个登录名启用了 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证。 此外，该服务器还必须能够允许使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证登录名。  
  
```  
// compile with: ole32.lib oleaut32.lib  
void InitializeAndEstablishConnection();  
  
#define STATUSDUMP(status)  case status : wprintf(L"status"); break;  
  
#define UNICODE  
#define DBINITCONSTANTS  
#define INITGUID  
  
#include <assert.h>  
#include <windows.h>  
#include <stdio.h>  
#include <stddef.h>  
#include <IOSTREAM>  
#include <cguid.h>  
#include <oledb.h>  
#include <oledberr.h>  
  
#include <msoledbsql.h>  
  
LPMALLOC pMalloc = NULL;  
IDBInitialize * pIDBInitialize = NULL;  
HRESULT hr;  
  
void DumpErrorInfo (IUnknown* pObjectWithError, REFIID IID_InterfaceWithError);  
void DumpErrorInfo (IUnknown* pObjectWithError, REFIID IID_InterfaceWithError, BOOL &has_sql_errors);  
  
int main() {  
   // All the initialization activities in a separate function.  
   InitializeAndEstablishConnection();  
  
   if ( FAILED( pIDBInitialize->Uninitialize() ) )  
      // Uninitialize is not required, but fails if an interface was not released.  
      printf("Problem uninitializing.\n");  
  
   pIDBInitialize->Release();  
   CoUninitialize();  
};  
  
void InitializeAndEstablishConnection() {  
   IDBProperties * pIDBProperties = NULL;  
   const ULONG nInitProps = 4;  
   DBPROP InitProperties[nInitProps];  
   const ULONG nSSInitProps = 1;  
   DBPROP SSInitProperties[nSSInitProps];  
  
   const ULONG nPropSet = 2;  
   DBPROPSET rgInitPropSet[nPropSet];  
  
   // Initialize the COM library.  
   CoInitialize(NULL);  
   CoGetMalloc(1, &pMalloc);  
  
   CLSID clsid;  
   CLSIDFromProgID(L"MSOLEDBSQL", &clsid);  
  
   // Obtain access to the SQLOLEDB provider.  
   hr = CoCreateInstance( clsid, NULL, CLSCTX_INPROC_SERVER, IID_IDBInitialize, (void **) &pIDBInitialize);  
   if (FAILED(hr))  
      printf("Failed in CoCreateInstance().\n");  
  
   // Initialize the property values needed to establish the connection.  
   for (int i = 0; i < nInitProps; i++)  
      VariantInit(&InitProperties[i].vValue);  
  
   // Specify database name.  
   InitProperties[0].dwPropertyID = DBPROP_INIT_CATALOG;  
   InitProperties[0].vValue.vt = VT_BSTR;  
   // Change the following line to use any database on your server  
   InitProperties[0].vValue.bstrVal = SysAllocString(L"master");  
   InitProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid = DB_NULLID;  
  
   InitProperties[1].dwPropertyID = DBPROP_AUTH_USERID;  
   InitProperties[1].vValue.vt = VT_BSTR;  
   // Modify the following line to add the user ID of a SQL Server Authentication account on your server  
   InitProperties[1].vValue.bstrVal = SysAllocString(L"");  
   InitProperties[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid = DB_NULLID;  
  
   InitProperties[2].dwPropertyID = DBPROP_AUTH_PASSWORD;  
   InitProperties[2].vValue.vt = VT_BSTR;  
   // Modify the following line to add the new SQL Server Authentication password  
   InitProperties[2].vValue.bstrVal = SysAllocString(L"");  
   InitProperties[2].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid = DB_NULLID;  
  
   InitProperties[3].dwPropertyID = DBPROP_INIT_DATASOURCE;  
   InitProperties[3].vValue.vt = VT_BSTR;  
   InitProperties[3].vValue.bstrVal = SysAllocString(L"(local)");  
   InitProperties[3].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[3].colid = DB_NULLID;  
  
   SSInitProperties[0].dwPropertyID = SSPROP_AUTH_OLD_PASSWORD;  
   SSInitProperties[0].vValue.vt = VT_BSTR;  
   // Modify the following line to specify the existing SQL Server Authentication password  
   SSInitProperties[0].vValue.bstrVal = SysAllocString(L"");  
   SSInitProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   SSInitProperties[0].colid = DB_NULLID;  
  
   rgInitPropSet[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties = nInitProps;  
   rgInitPropSet[0].rgProperties = InitProperties;  
  
   rgInitPropSet[1].guidPropertySet = DBPROPSET_SQLSERVERDBINIT;  
   rgInitPropSet[1].cProperties = nSSInitProps;  
   rgInitPropSet[1].rgProperties = SSInitProperties;  
  
   // Set initialization properties.  
   hr = pIDBInitialize->QueryInterface( IID_IDBProperties, (void **)&pIDBProperties);  
  
   if (FAILED(hr))  
      printf("Failed to obtain IDBProperties interface.\n");  
  
   hr = pIDBProperties->SetProperties( nPropSet, rgInitPropSet);  
  
   if (FAILED(hr))  
      printf("Failed to set initialization properties.\n");  
  
   pIDBProperties->Release();  
  
   // establish connection to the data source.  
   DWORD now = GetTickCount();  
  
   if ( FAILED(pIDBInitialize->Initialize()) ) {  
      printf("Problem in initializing.\n");  
      DumpErrorInfo(pIDBInitialize, IID_IDBInitialize);  
   }  
  
   DWORD tookms = GetTickCount() - now;  
   printf("Connection took %d ms.\n", tookms);  
}  
  
// DumpErrorInfo queries SQLOLEDB error interfaces, retrieving available status  
// or error information. This version is called when SQL Server errors are not expected.  
void DumpErrorInfo (IUnknown* pObjectWithError, REFIID IID_InterfaceWithError) {  
   BOOL has_sql_errors;  
   DumpErrorInfo (pObjectWithError, IID_InterfaceWithError, has_sql_errors);  
}  
  
// DumpErrorInfo queries SQLOLEDB error interfaces, retrieving available status   
// or error information. This version is called when an SQL Server error could occur.  
void DumpErrorInfo (IUnknown* pObjectWithError, REFIID IID_InterfaceWithError, BOOL &has_sql_errors ) {  
   // Interfaces used in the example.  
   IErrorInfo * pIErrorInfoAll = NULL;  
   IErrorInfo * pIErrorInfoRecord = NULL;  
   IErrorRecords * pIErrorRecords = NULL;  
   ISupportErrorInfo * pISupportErrorInfo = NULL;  
   ISQLErrorInfo * pISQLErrorInfo = NULL;  
   ISQLServerErrorInfo * pISQLServerErrorInfo = NULL;  
  
   // Number of error records.  
   ULONG nRecs;  
   ULONG nRec;  
  
   // Basic error information from GetBasicErrorInfo.  
   ERRORINFO errorinfo;  
  
   // IErrorInfo values.  
   BSTR bstrDescription;  
   BSTR bstrSource;  
  
   // ISQLErrorInfo parameters.  
   BSTR bstrSQLSTATE;  
   LONG lNativeError;  
  
   // ISQLServerErrorInfo parameter pointers.  
   SSERRORINFO * pSSErrorInfo = NULL;  
   OLECHAR * pSSErrorStrings = NULL;  
  
   // Hard-code an English (United States) locale for the example.  
   DWORD MYLOCALEID = 0x0409;  
  
   has_sql_errors = 0;  
  
   // Only ask for error information if the interface supports it.  
   if (FAILED(pObjectWithError->QueryInterface(IID_ISupportErrorInfo, (void**) &pISupportErrorInfo)))     
      wprintf(L"SupportErrorErrorInfo interface not supported\n");  
   else if (FAILED(pISupportErrorInfo->InterfaceSupportsErrorInfo(IID_InterfaceWithError)))     
      wprintf(L"InterfaceWithError interface not supported\n");  
  
   // Do not test the return of GetErrorInfo. It can succeed and return  
   // a NULL pointer in pIErrorInfoAll. Simply test the pointer.  
   GetErrorInfo(0, &pIErrorInfoAll);  
  
   // Test to see if it's a valid OLE DB IErrorInfo interface exposing a list of records.  
   if (pIErrorInfoAll != NULL)  {  
      if (SUCCEEDED(pIErrorInfoAll->QueryInterface(IID_IErrorRecords, (void**) &pIErrorRecords))) {  
         pIErrorRecords->GetRecordCount(&nRecs);  
  
         // Within each record, retrieve information from each of the defined interfaces.  
         for (nRec = 0; nRec < nRecs; nRec++) {  
            ULONG errorno = nRecs - nRec - 1;  
            // From IErrorRecords, get the HRESULT and a reference to the ISQLErrorInfo interface.  
            pIErrorRecords->GetBasicErrorInfo(errorno, &errorinfo);  
            pIErrorRecords->GetCustomErrorObject(errorno,IID_ISQLErrorInfo, (IUnknown**) &pISQLErrorInfo);  
  
            // Display the HRESULT, then use the ISQLErrorInfo.  
            wprintf(L"HRESULT:\t%#X\n", errorinfo.hrError);  
  
            if (pISQLErrorInfo != NULL) {  
               pISQLErrorInfo->GetSQLInfo(&bstrSQLSTATE, &lNativeError);  
               // Display SQLSTATE and native error values.  
               wprintf(L"SQLSTATE:\t%s\nNative Error:\t%ld\n", bstrSQLSTATE, lNativeError);  
  
               // SysFree BSTR references.  
               SysFreeString(bstrSQLSTATE);  
  
               // Get the ISQLServerErrorInfo interface from  
               // ISQLErrorInfo before releasing the reference.  
               pISQLErrorInfo->QueryInterface(IID_ISQLServerErrorInfo, (void**) &pISQLServerErrorInfo);  
  
               pISQLErrorInfo->Release();  
            }  
  
            // Test to ensure the reference is valid, then get error information from ISQLServerErrorInfo.  
            if (pISQLServerErrorInfo != NULL) {  
               pISQLServerErrorInfo->GetErrorInfo(&pSSErrorInfo, &pSSErrorStrings);  
  
               // ISQLServerErrorInfo::GetErrorInfo succeeds even when it  
               // has nothing to return. Test the pointers before using.  
               if (pSSErrorInfo) {  
                  // Display the state and severity from the returned  
                  // information. The error message comes from  
                  // IErrorInfo::GetDescription.  
                  wprintf(L"Error state:\t%d\nSeverity:\t%d\n", pSSErrorInfo->bState, pSSErrorInfo->bClass);  
                  wprintf(L"Procedure:\t%s\nLine:\t%d\n", pSSErrorInfo->pwszProcedure, pSSErrorInfo->wLineNumber);  
  
                  has_sql_errors++;  
  
                  // IMalloc::Free needed to release references on returned values.  
                  pMalloc->Free(pSSErrorStrings);  
                  pMalloc->Free(pSSErrorInfo);  
               }  
  
               pISQLServerErrorInfo->Release();  
            }  
  
            if (SUCCEEDED(pIErrorRecords->GetErrorInfo(errno, MYLOCALEID, &pIErrorInfoRecord))) {  
               // Get the source and description (error message) from the record's IErrorInfo.  
               pIErrorInfoRecord->GetSource(&bstrSource);  
               pIErrorInfoRecord->GetDescription(&bstrDescription);  
  
               if (bstrSource != NULL) {  
                  wprintf(L"Source:\t\t%s\n", bstrSource);  
                  SysFreeString(bstrSource);  
               }  
  
               if (bstrDescription != NULL) {  
                  wprintf(L"Error message:\t%s\n", bstrDescription);  
                  SysFreeString(bstrDescription);  
               }  
  
               pIErrorInfoRecord->Release();  
            }  
         }  
  
         pIErrorRecords->Release();  
      }  
      else {  
         // IErrorInfo is valid; get the source and  
         // description to see what it is.  
         pIErrorInfoAll->GetSource(&bstrSource);  
         pIErrorInfoAll->GetDescription(&bstrDescription);  
  
         if (bstrSource != NULL) {  
            wprintf(L"Source:\t\t%s\n", bstrSource);  
            SysFreeString(bstrSource);  
         }  
  
         if (bstrDescription != NULL) {  
            wprintf(L"Error message:\t%s\n", bstrDescription);  
            SysFreeString(bstrDescription);  
         }  
      }  
  
      pIErrorInfoAll->Release();  
   }  
   else  
      wprintf(L"GetErrorInfo failed.\n");  
  
   if (pISupportErrorInfo != NULL)  
      pISupportErrorInfo->Release();  
}  
```  
  
