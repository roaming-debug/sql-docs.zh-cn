---
title: IBCPSession::BCPDone（OLE DB 驱动程序）| Microsoft Docs
description: 了解 IBCPSession::BCPDone 方法如何在 OLE DB Driver for SQL Server 中提交要发送到 SQL Server 的剩余行。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPDone (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPDone method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1a2f7b39618c9cab40952e6c9fe3c4916a540d60
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748687"
---
# <a name="ibcpsessionbcpdone-ole-db"></a>IBCPSession::BCPDone (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  提交要发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的剩余行。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
HRESULT BCPDone(void);  
```  
  
## <a name="remarks"></a>备注  
 在调用 BCPDone 方法之后，不能对 [IBCPSession](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md) 接口调用其他操作  。 唯一的可能性是调用 [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) 方法以启动新的大容量复制操作。 这类似于调用 [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md) 方法。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，在调用该方法之前，未调用 BCPInit 方法  。  
  
## <a name="example"></a>示例  
 该示例演示如何使用 IBCPSession 接口  。  
  
 在运行此示例之前，必须执行以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)]：  
  
```sql  
create table fltest(col1 int, col2 int, col3 image)  
insert into fltest values (1, 1, 0x0FF)  
insert into fltest values (2, 2, 0xF00)  
insert into fltest values (3, 3, 0xBAD)  
insert into fltest values (4, 4, 0xFAD)  
```  
  
 此示例连接到 master 数据库。  
  
 当示例运行时，它将在项目目录中创建 outfile.dat。 outfile.dat 包含在本机（二进制）模式下从表中复制的数据。  
  
 可以通过以下命令，使用 BCP 将此数据添加回表中：  
  
 bcp master..fltest in outfile.dat -n -T -S  server   
  
```cpp  
#define DBINITCONSTANTS   // Defined to initialize constants in oledb.h  
#define INITGUID  
#define MAX_ROWS  100  
  
#include <windows.h>  
#include <sqlext.h>  
#include <msoledbsql.h>  
#include <oledberr.h>  
#include <stdio.h>  
  
#define SAFE_RELEASE(p) { \  
   if (p) { \  
   (p)->Release(); \  
   (p)=NULL; \  
   } \  
}  
  
// DumpErrorInfo queries error interfaces, retrieving available status or error information.  
void DumpErrorInfo ( IUnknown* pObjectWithError, REFIID IID_InterfaceWithError ) {  
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
  
   // Hard-code a US English locale for the example.  
   DWORD MYLOCALEID = 0x0409;  
  
   // Only ask for error information if the interface supports it.  
   if (FAILED(pObjectWithError->QueryInterface(IID_ISupportErrorInfo, (void**) &pISupportErrorInfo))) {  
      wprintf(L"SupportErrorErrorInfo interface not supported\r\n");  
      return;  
   }  
  
   if (FAILED(pISupportErrorInfo->InterfaceSupportsErrorInfo(IID_InterfaceWithError))) {  
      wprintf(L"InterfaceWithError interface not supported\r\n");  
      return;  
   }  
  
   // Do not test the return of GetErrorInfo. It can succeed and return  
   // a NULL pointer in pIErrorInfoAll. Simply test the pointer.  
   GetErrorInfo(0, &pIErrorInfoAll);  
  
   if (pIErrorInfoAll != NULL) {  
      // Test to see if it's a valid OLE DB IErrorInfo interface exposing a list of records.  
      if (SUCCEEDED(pIErrorInfoAll->QueryInterface(IID_IErrorRecords, (void**) &pIErrorRecords))) {  
         pIErrorRecords->GetRecordCount(&nRecs);  
  
         // Within each record, retrieve information from each of the defined interfaces.  
         for (nRec = 0; nRec < nRecs; nRec++) {  
            // From IErrorRecords, get the HRESULT and a reference to the ISQLErrorInfo interface.  
            pIErrorRecords->GetBasicErrorInfo(nRec, &errorinfo);  
            pIErrorRecords->GetCustomErrorObject(nRec,IID_ISQLErrorInfo, (IUnknown**) &pISQLErrorInfo);  
  
            // Display the HRESULT, then use the ISQLErrorInfo.  
            wprintf(L"HRESULT:\t%#X\r\n", errorinfo.hrError);  
  
            if (pISQLErrorInfo != NULL) {  
               pISQLErrorInfo->GetSQLInfo(&bstrSQLSTATE, &lNativeError);  
  
               // Display the SQLSTATE and native error values.  
               wprintf(L"SQLSTATE:\t%s\r\nNative Error:\t%ld\r\n", bstrSQLSTATE, lNativeError);  
  
               // SysFree BSTR references.  
               SysFreeString(bstrSQLSTATE);  
  
               // Get the ISQLServerErrorInfo interface from ISQLErrorInfo before releasing the reference.  
               pISQLErrorInfo->QueryInterface( IID_ISQLServerErrorInfo, (void**) &pISQLServerErrorInfo);   
  
               pISQLErrorInfo->Release();  
            }  
  
            // Test to ensure the reference is valid, then get error information from ISQLServerErrorInfo.  
            if (pISQLServerErrorInfo != NULL) {  
               pISQLServerErrorInfo->GetErrorInfo(&pSSErrorInfo, &pSSErrorStrings);  
  
               // ISQLServerErrorInfo::GetErrorInfo succeeds even when it has nothing to return.   
               // Test the pointers before using.  
               if (pSSErrorInfo) {  
                  // Display the state and severity from the returned information.   
                  // The error message comes from IErrorInfo::GetDescription.  
                  wprintf(L"Error state:\t%d\r\nSeverity:\t%d\r\n", pSSErrorInfo->bState, pSSErrorInfo->bClass);  
  
                  // IMalloc::Free needed to release references on returned values.  
                  CoTaskMemFree(pSSErrorStrings);  
                  CoTaskMemFree(pSSErrorInfo);  
               }  
  
               pISQLServerErrorInfo->Release();  
            }  
  
            if (SUCCEEDED(pIErrorRecords->GetErrorInfo(nRec, MYLOCALEID, &pIErrorInfoRecord))) {  
               // Get the source and description (error message) from the record's IErrorInfo.  
               pIErrorInfoRecord->GetSource(&bstrSource);  
               pIErrorInfoRecord->GetDescription(&bstrDescription);  
  
               if (bstrSource != NULL) {  
                  wprintf(L"Source:\t\t%s\r\n", bstrSource);  
                  SysFreeString(bstrSource);  
               }  
  
               if (bstrDescription != NULL) {  
                  wprintf(L"Error message:\t%s\r\n", bstrDescription);  
                  SysFreeString(bstrDescription);  
               }  
  
               pIErrorInfoRecord->Release();  
            }  
         }  
         pIErrorRecords->Release();  
      }  
      else {  
         // IErrorInfo is valid; get the source and description to see what it is.  
         pIErrorInfoAll->GetSource(&bstrSource);  
         pIErrorInfoAll->GetDescription(&bstrDescription);  
  
         if (bstrSource != NULL) {  
            wprintf(L"Source:\t\t%s\r\n", bstrSource);  
            SysFreeString(bstrSource);  
         }  
  
         if (bstrDescription != NULL) {  
            wprintf(L"Error message:\t%s\r\n", bstrDescription);  
            SysFreeString(bstrDescription);  
         }  
      }  
  
      pIErrorInfoAll->Release();  
   }  
   else   
      wprintf(L"GetErrorInfo has not returned ErrorInfo.\r\n");  
  
   pISupportErrorInfo->Release();  
   return;  
}  
  
IDBCreateSession *Connect () {  
   // constant definitions  
   static LPCWSTR pwszProviderString = L"server=(local)\\dschwart3;Database=master;Trusted_Connection=yes;";  
   static LPCWSTR pwszDataSource     = NULL;  
   static LPCWSTR pwszUserID         = NULL;  
   static LPCWSTR pwszPassword       = NULL;  
  
   IDBInitialize    * pIDBInitialize      = NULL;  
   IDBProperties    * pIDBProperties      = NULL;  
   IDBCreateSession * pIDBCreateSession   = NULL;  
  
   HRESULT hr;  
  
   // Obtain the provider's clsid  
   CLSID clsidProv = CLSID_MSOLEDBSQL;  
  
   // Initialize COM  
   CoInitialize(NULL);  
  
   hr = CoCreateInstance(clsidProv, NULL, CLSCTX_ALL, IID_IDBInitialize,(void **)&pIDBInitialize);  
   if (!SUCCEEDED(hr))   
      return NULL;  
  
   hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);  
   if (!SUCCEEDED(hr))   
      return NULL;  
  
   // Set Init properties  
   {  
      DBPROPSET rgPropSets[1];  
      ULONG     cPropSets   = 0;  
      DBPROP    rgInitProperties[10];  
  
      // Initialize Data source properties (connection info)  
      {  
         ULONG cProperties = 0;  
  
         // DBPROP_INIT_DATASOURCE  
         if (pwszDataSource) {  
            rgInitProperties[cProperties].dwPropertyID    = DBPROP_INIT_DATASOURCE;  
            rgInitProperties[cProperties].dwOptions       = DBPROPOPTIONS_REQUIRED;  
            rgInitProperties[cProperties].dwStatus        = DBPROPSTATUS_OK;  
            rgInitProperties[cProperties].colid           = DB_NULLID;  
            rgInitProperties[cProperties].vValue.vt       = VT_BSTR;  
            V_BSTR(&rgInitProperties[cProperties].vValue) = SysAllocString(pwszDataSource);                 
            cProperties++;  
         }  
  
         // DBPROP_AUTH_USERID  
         if (pwszUserID) {  
            rgInitProperties[cProperties].dwPropertyID    = DBPROP_AUTH_USERID;  
            rgInitProperties[cProperties].dwOptions       = DBPROPOPTIONS_REQUIRED;  
            rgInitProperties[cProperties].dwStatus        = DBPROPSTATUS_OK;  
            rgInitProperties[cProperties].colid           = DB_NULLID;  
            rgInitProperties[cProperties].vValue.vt       = VT_BSTR;  
            V_BSTR(&rgInitProperties[cProperties].vValue) = SysAllocString(pwszUserID);  
            cProperties++;  
         }  
  
         // DBPROP_AUTH_PASSWORD  
         if (pwszPassword) {  
            rgInitProperties[cProperties].dwPropertyID    = DBPROP_AUTH_PASSWORD;  
            rgInitProperties[cProperties].dwOptions       = DBPROPOPTIONS_REQUIRED;  
            rgInitProperties[cProperties].dwStatus        = DBPROPSTATUS_OK;  
            rgInitProperties[cProperties].colid           = DB_NULLID;  
            rgInitProperties[cProperties].vValue.vt       = VT_BSTR;  
            V_BSTR(&rgInitProperties[cProperties].vValue) = SysAllocString(pwszPassword);  
            cProperties++;  
         }  
  
         // DBPROP_INIT_PROVIDERSTRING  
         if (pwszProviderString) {  
            rgInitProperties[cProperties].dwPropertyID    = DBPROP_INIT_PROVIDERSTRING;  
            rgInitProperties[cProperties].dwOptions       = DBPROPOPTIONS_REQUIRED;  
            rgInitProperties[cProperties].dwStatus        = DBPROPSTATUS_OK;  
            rgInitProperties[cProperties].colid           = DB_NULLID;  
            rgInitProperties[cProperties].vValue.vt       = VT_BSTR;  
            V_BSTR(&rgInitProperties[cProperties].vValue) = SysAllocString(pwszProviderString);  
            cProperties++;  
         }  
  
         if (cProperties) {  
            rgPropSets[cPropSets].cProperties = cProperties;  
            rgPropSets[cPropSets].rgProperties = rgInitProperties;  
            rgPropSets[cPropSets].guidPropertySet = DBPROPSET_DBINIT;  
            cPropSets++;  
         }  
      }  
  
      // Initialize  
      hr = pIDBProperties->SetProperties(cPropSets, rgPropSets);  
      if (!SUCCEEDED(hr)) {  
         DumpErrorInfo(pIDBProperties, IID_IDBProperties);  
         return NULL;  
      }  
   }  
  
   SAFE_RELEASE(pIDBProperties);  
  
   hr = pIDBInitialize->Initialize();  
   if (!SUCCEEDED(hr)) {  
      DumpErrorInfo(pIDBInitialize, IID_IDBInitialize);  
      return NULL;  
   }  
  
   hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);  
   if (!SUCCEEDED(hr)) {   
      DumpErrorInfo(pIDBInitialize, IID_IDBInitialize);  
      return NULL;  
   }  
  
   // Set SQL Server Specific properties  
   {  
      DBPROPSET rgPropSets[1];  
      ULONG cPropSets = 0;  
      DBPROP rgSqlProperties[10];  
  
      // Initialize the DBProps for fastload and BCP  
      {  
         ULONG cProperties = 0;  
  
         // SSPROP_ENABLEFASTLOAD  
         {  
            rgSqlProperties[cProperties].dwPropertyID   = SSPROP_ENABLEFASTLOAD;  
            rgSqlProperties[cProperties].dwOptions      = DBPROPOPTIONS_REQUIRED;  
            rgSqlProperties[cProperties].dwStatus       = DBPROPSTATUS_OK;  
            rgSqlProperties[cProperties].colid          = DB_NULLID;  
            rgSqlProperties[cProperties].vValue.vt      = VT_BOOL;  
            V_BOOL(&rgSqlProperties[cProperties].vValue)= VARIANT_TRUE;  
            cProperties++;  
         }  
  
         // SSPROP_ENABLEBULKCOPY  
         {  
            rgSqlProperties[cProperties].dwPropertyID   = SSPROP_ENABLEBULKCOPY;  
            rgSqlProperties[cProperties].dwOptions      = DBPROPOPTIONS_REQUIRED;  
            rgSqlProperties[cProperties].dwStatus       = DBPROPSTATUS_OK;  
            rgSqlProperties[cProperties].colid          = DB_NULLID;  
            rgSqlProperties[cProperties].vValue.vt      = VT_BOOL;  
            V_BOOL(&rgSqlProperties[cProperties].vValue)= VARIANT_TRUE;  
            cProperties++;  
         }  
  
         rgPropSets[cPropSets].cProperties = cProperties;  
         rgPropSets[cPropSets].rgProperties = rgSqlProperties;  
         rgPropSets[cPropSets].guidPropertySet = DBPROPSET_SQLSERVERDATASOURCE;  
         cPropSets++;  
      }  
  
      hr = pIDBProperties->SetProperties(cPropSets, rgPropSets);  
      if (!SUCCEEDED(hr)) {   
         DumpErrorInfo(pIDBProperties, IID_IDBProperties);  
         return NULL;  
      }  
   }  
  
   SAFE_RELEASE(pIDBProperties);  
  
   hr = pIDBInitialize->QueryInterface(IID_IDBCreateSession, (void **)&pIDBCreateSession);  
   if (!SUCCEEDED(hr)) {   
      DumpErrorInfo(pIDBInitialize, IID_IDBInitialize);  
      return NULL;  
   }  
  
   SAFE_RELEASE(pIDBInitialize);  
  
   return pIDBCreateSession;  
}  
  
void wmain() {  
   static LPCWSTR pwszTableName   = L"fltest";  
   static LPCWSTR pwszOutFileName = L"outfile.dat";  
   static LPCWSTR pwszErrFileName = L"errfile.txt";  
  
   DBROWCOUNT icRowsCopied = 0;  
  
   IDBCreateSession * pIDBCreateSession = NULL;  
   IBCPSession * pIBCPSession = NULL;  
  
   HRESULT hr;  
  
   pIDBCreateSession = Connect();  
   if (!pIDBCreateSession) {   
      printf("Failed to connect\r\n");  
      exit(1);  
   }  
  
   // get an IBCPSession interface  
   hr = pIDBCreateSession->CreateSession(NULL, IID_IBCPSession, (IUnknown**) &pIBCPSession);  
  
   if (FAILED(hr)) {  
      printf("Failed to obtain an IBCPSession interface\r\n");  
      DumpErrorInfo(pIDBCreateSession, IID_IDBCreateSession);  
      exit(1);  
   }  
  
   SAFE_RELEASE(pIDBCreateSession);  
  
   // Initialize BCP  
  
   // Here we create the mapping between file and table, identifying  
   // which is the source and which is the destination through the final parameter  
   // (here we are copying out of the table and into the file).  
   hr = pIBCPSession->BCPInit(pwszTableName, pwszOutFileName, pwszErrFileName, BCP_DIRECTION_OUT);  
  
   if (FAILED(hr)) {  
      printf("BCPInit failed.\r\n");  
      DumpErrorInfo(pIBCPSession, IID_IBCPSession);  
      exit(1);  
   }  
  
   // You can set SSPROP_ASYNCH_BULKCOPY to TRUE to improve client latency here,  
   // at which point BCPExec can return DB_S_ASYNCHRONOUS.  
   do {  
      hr = pIBCPSession->BCPExec(&icRowsCopied);  
   } while (hr == DB_S_ASYNCHRONOUS);  
  
   if (FAILED(hr)) {  
      printf("BCPExec failed.\r\n");  
      DumpErrorInfo(pIBCPSession, IID_IBCPSession);  
      exit(1);  
   }  
  
   // flush the rest of the rows and finalize the transaction -- the process is complete.  
   hr = pIBCPSession->BCPDone();  
   if (FAILED(hr)) {  
      printf("BCPDone failed.\r\n");  
      DumpErrorInfo(pIBCPSession, IID_IBCPSession);  
      exit(1);  
   }  
  
   SAFE_RELEASE(pIBCPSession);  
  
   CoUninitialize();  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [执行大容量复制操作](../../oledb/features/performing-bulk-copy-operations.md)  
  
  
