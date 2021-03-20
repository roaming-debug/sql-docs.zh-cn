---
description: '执行 SQL Server Native Client User-Defined 函数并处理返回代码 (OLE DB) '
title: 'User-Defined 函数，返回代码 (OLE DB) '
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- user-defined functions [OLE DB]
ms.assetid: d96c33fd-ed17-4713-8921-bf2dc3347f78
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 77f44a74c695aac1ddd6b67abc2c1c7b82e5e71c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751987"
---
# <a name="execute-a-sql-server-native-client-user-defined-function-and-process-return-code-ole-db"></a>执行 SQL Server Native Client User-Defined 函数并处理返回代码 (OLE DB) 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在此示例中，将执行一个用户定义函数并输出返回代码。 IA64 平台不支持此示例。  
  
 此示例要求使用 AdventureWorks 示例数据库，其可从 [Microsoft SQL Server 示例和社区项目](https://go.microsoft.com/fwlink/?LinkID=85384)主页下载。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 crypto API](/windows/win32/seccrypto/cryptography-reference)（Win32 加密 API）加密它们。  
  
## <a name="example"></a>示例  
 执行第一个 ([!INCLUDE[tsql](../../../includes/tsql-md.md)]) 代码列表，以创建该应用程序要使用的存储过程。  
  
 使用 ole32.lib 和 oleaut32.lib 编译并执行第二个 (C++) 代码列表。 此应用程序连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 在某些 Windows 操作系统上，您需要将 (localhost) 或 (local) 更改为您的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。 若要连接到命名实例，请将连接字符串从 L"(local)" 更改为 L"(local)\\\name"，其中 name 是命名实例。 默认情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Express 安装在命名实例中。 请确保 INCLUDE 环境变量包含包含 sqlncli.msi 的目录。  
  
 执行第三个 ([!INCLUDE[tsql](../../../includes/tsql-md.md)]) 代码列表，以删除该应用程序使用的存储过程。  
  
```sql
USE AdventureWorks  
if exists (SELECT * FROM sys.objects WHERE object_id = OBJECT_ID(N'[fn_RectangleArea]'))  
   drop function fn_RectangleArea  
go  
  
CREATE FUNCTION fn_RectangleArea  
   (@Width int,   
@Height int )  
RETURNS int  
AS  
BEGIN  
  
   RETURN ( @Width * @Height )  
END  
GO  
```  
  
```cpp
// compile with: ole32.lib oleaut32.lib  
void InitializeAndEstablishConnection();  
#define UNICODE  
#define DBINITCONSTANTS  
#define INITGUID  
#define OLEDBVER 0x0250   // to include correct interfaces  
  
#include <windows.h>  
#include <stdio.h>  
#include <stddef.h>  
#include <iostream>  
#include <oledb.h>  
#include <oledberr.h>  
#include <sqlncli.h>  
using namespace std;  
  
IDBInitialize* pIDBInitialize = NULL;      
IDBCreateSession* pIDBCreateSession = NULL;  
IDBCreateCommand* pIDBCreateCommand = NULL;  
ICommandText* pICommandText = NULL;  
IRowset* pIRowset = NULL;  
ICommandWithParameters* pICommandWithParams = NULL;  
IAccessor* pIAccessor = NULL;  
IDBProperties* pIDBProperties = NULL;  
WCHAR* pStringsBuffer;  
DBBINDING* pBindings;  
const ULONG nInitProps = 4;  
DBPROP InitProperties[nInitProps];      
const ULONG nPropSet = 1;  
DBPROPSET rgInitPropSet[nPropSet];    
HRESULT hr;  
HACCESSOR hAccessor;  
const ULONG nParams = 3;   // No. of parameters in the command  
DBPARAMBINDINFO ParamBindInfo[nParams];  
ULONG i;  
ULONG cbColOffset = 0;  
  
DB_UPARAMS ParamOrdinals[nParams];  
DBROWCOUNT cNumRows = 0;  
DBPARAMS Params;  
  
// Declare array of DBBINDING structures, one for each parameter in the command  
DBBINDING acDBBinding[nParams];  
DBBINDSTATUS acDBBindStatus[nParams];  
  
// The following buffer is used to store parameter values.  
typedef struct tagSPROCPARAMS {  
   long lReturnValue;  
   long inParam1;  
   long inParam2;  
} SPROCPARAMS;  
  
int main() {  
   // The command to execute.  
   WCHAR* wCmdString = L"{? = CALL fn_RectangleArea(?, ?) }";  
   // WCHAR* wCmdString = L"EXEC ? = fn_RectangleVolume(?, ?)";  
  
   SPROCPARAMS sprocparams = {0,5,10};  
  
   // All the initialization stuff in a separate function.  
   InitializeAndEstablishConnection();  
  
   // Let us create a new session from the data source object.  
   if (FAILED(pIDBInitialize->QueryInterface( IID_IDBCreateSession,  
                                              (void**) &pIDBCreateSession))) {  
      cout << "Failed to access IDBCreateSession interface\n";  
      goto EXIT;  
   }  
  
   if ( FAILED(pIDBCreateSession->CreateSession( NULL,   
                                                 IID_IDBCreateCommand,   
                                                 (IUnknown**) &pIDBCreateCommand))) {  
      cout << "pIDBCreateSession->CreateSession failed\n";  
      goto EXIT;  
   }  
  
   // Create a Command  
   if (FAILED(pIDBCreateCommand->CreateCommand( NULL,   
                                                IID_ICommandText,   
                                                (IUnknown**) &pICommandText))) {  
      cout << "Failed to access ICommand interface\n";  
      goto EXIT;  
   }  
  
   // Set the command text.  
   if (FAILED(pICommandText->SetCommandText(DBGUID_DBSQL, wCmdString))) {  
      cout << "failed to set command text\n";  
      goto EXIT;  
   }  
  
   // Describe the command parameters (parameter name, provider specific name   
   // of the parameter's data type etc.) in an array of DBPARAMBINDINFO   
   // structures.  This information is then used by SetParameterInfo().  
   ParamBindInfo[0].pwszDataSourceType = L"DBTYPE_I4";  
   ParamBindInfo[0].pwszName = NULL;  
   ParamBindInfo[0].ulParamSize = sizeof(long);  
   ParamBindInfo[0].dwFlags = DBPARAMFLAGS_ISOUTPUT;  
   ParamBindInfo[0].bPrecision = 11;  
   ParamBindInfo[0].bScale = 0;  
   ParamOrdinals[0] = 1;  
  
   ParamBindInfo[1].pwszDataSourceType = L"DBTYPE_I4";  
   ParamBindInfo[1].pwszName = NULL;   // L"@inparam1";                 
   ParamBindInfo[1].ulParamSize = sizeof(long);  
   ParamBindInfo[1].dwFlags = DBPARAMFLAGS_ISINPUT;  
   ParamBindInfo[1].bPrecision = 11;  
   ParamBindInfo[1].bScale = 0;  
   ParamOrdinals[1] = 2;  
  
   ParamBindInfo[2].pwszDataSourceType = L"DBTYPE_I4";  
   ParamBindInfo[2].pwszName = NULL;   // L"@inparam2";   
   ParamBindInfo[2].ulParamSize = sizeof(long);  
   ParamBindInfo[2].dwFlags = DBPARAMFLAGS_ISINPUT;  
   ParamBindInfo[2].bPrecision = 11;  
   ParamBindInfo[2].bScale = 0;  
   ParamOrdinals[2] = 3;  
  
   // Set the parameters information.  
   if (FAILED(pICommandText->QueryInterface( IID_ICommandWithParameters,  
                                             (void**)&pICommandWithParams))) {  
      cout << "failed to obtain ICommandWithParameters\n";  
      goto EXIT;  
   }  
   if (FAILED(pICommandWithParams->SetParameterInfo( nParams,   
                                                     ParamOrdinals,   
                                                     ParamBindInfo))) {  
      cout << "failed in setting parameter info.(SetParameterInfo)\n";  
      goto EXIT;  
   }  
  
   // Describe the consumer buffer; initialize the array of DBBINDING structures.    
   // Each binding associates a single parameter to the consumer's buffer.  
   for ( i = 0 ; i < nParams ; i++ ) {  
      acDBBinding[i].obLength = 0;  
      acDBBinding[i].obStatus = 0;  
      acDBBinding[i].pTypeInfo = NULL;  
      acDBBinding[i].pObject = NULL;  
      acDBBinding[i].pBindExt = NULL;  
      acDBBinding[i].dwPart = DBPART_VALUE;  
      acDBBinding[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      acDBBinding[i].dwFlags = 0;  
      acDBBinding[i].bScale = 0;  
   }   // for  
  
   acDBBinding[0].iOrdinal = 1;  
   acDBBinding[0].obValue = offsetof(SPROCPARAMS, lReturnValue);  
   acDBBinding[0].eParamIO = DBPARAMIO_OUTPUT;  
   acDBBinding[0].cbMaxLen = sizeof(long);  
   acDBBinding[0].wType = DBTYPE_I4;  
   acDBBinding[0].bPrecision = 11;  
  
   acDBBinding[1].iOrdinal = 2;  
   acDBBinding[1].obValue = offsetof(SPROCPARAMS, inParam1);  
   acDBBinding[1].eParamIO = DBPARAMIO_INPUT;  
   acDBBinding[1].cbMaxLen = sizeof(long);  
   acDBBinding[1].wType = DBTYPE_I4;  
   acDBBinding[1].bPrecision = 11;  
  
   acDBBinding[2].iOrdinal = 3;  
   acDBBinding[2].obValue = offsetof(SPROCPARAMS, inParam2);  
   acDBBinding[2].eParamIO = DBPARAMIO_INPUT;  
   acDBBinding[2].cbMaxLen = sizeof(long);  
   acDBBinding[2].wType = DBTYPE_I4;  
   acDBBinding[2].bPrecision = 11;  
  
   // Let us create an accessor from the above set of bindings.  
   hr = pICommandWithParams->QueryInterface( IID_IAccessor, (void**)&pIAccessor);  
   if (FAILED(hr))  
      cout << "Failed to get IAccessor interface\n";  
  
   hr = pIAccessor->CreateAccessor( DBACCESSOR_PARAMETERDATA,  
                                    nParams,   
                                    acDBBinding,   
                                    sizeof(SPROCPARAMS),   
                                    &hAccessor,  
                                    acDBBindStatus);  
   if (FAILED(hr))  
      cout << "failed to create accessor for the defined parameters\n";  
  
   // Initialize DBPARAMS structure for command execution. DBPARAMS specifies the  
   // parameter values in the command.  DBPARAMS is then passed to Execute.  
   Params.pData = &sprocparams;  
   Params.cParamSets = 1;  
   Params.hAccessor = hAccessor;  
  
   // Execute the command.  
   if (FAILED(hr = pICommandText->Execute( NULL,   
                                           IID_IRowset,   
                                           &Params,   
                                           &cNumRows,   
                                           (IUnknown **) &pIRowset))) {  
      cout << "failed to execute command\n";  
      goto EXIT;  
   }  
  
   printf("  Return value = %d\n", sprocparams.lReturnValue);  
  
   // Release result set without processing.  
   if (pIRowset != NULL)  
      pIRowset->Release();  
  
   // Release memory.  
   pIAccessor->ReleaseAccessor(hAccessor, NULL);  
   pIAccessor->Release();  
   pICommandWithParams->Release();  
   pICommandText->Release();  
   pIDBCreateCommand->Release();  
   pIDBCreateSession->Release();      
  
   if (FAILED(pIDBInitialize->Uninitialize()))  
      // Uninitialize is not required, but it fails if an interface  
      // has not been released.  This can be used for debugging.  
      cout << "Problem uninitializing\n";  
  
   pIDBInitialize->Release();  
  
   CoUninitialize();  
   return 0;  
  
EXIT:  
   if (pIAccessor != NULL)  
      pIAccessor->Release();  
   if (pICommandWithParams != NULL)  
      pICommandWithParams->Release();  
   if (pICommandText != NULL)  
      pICommandText->Release();  
   if (pIDBCreateCommand != NULL)  
      pIDBCreateCommand->Release();  
   if (pIDBCreateSession != NULL)  
      pIDBCreateSession->Release();  
   if (pIDBInitialize != NULL)  
      if (FAILED(pIDBInitialize->Uninitialize()))  
         // Uninitialize is not required, but it fails if an interface has   
         // not been released.  This can be used for debugging.  
         cout << "problem in uninitializing\n";  
      pIDBInitialize->Release();  
  
   CoUninitialize();  
}  
  
void InitializeAndEstablishConnection() {      
   // Initialize the COM library.  
   CoInitialize(NULL);  
  
   // Obtain access to the SQL Server native Client OLE DB provider.      
   hr = CoCreateInstance( CLSID_SQLNCLI11,   
                          NULL,   
                          CLSCTX_INPROC_SERVER,  
                          IID_IDBInitialize,   
                          (void **) &pIDBInitialize);  
  
   if (FAILED(hr))  
      cout << "Failed in CoCreateInstance()\n";  
  
   // Initialize the property values needed to establish the connection.  
   for ( i = 0 ; i < nInitProps ; i++ )  
      VariantInit(&InitProperties[i].vValue);  
  
   // Specify server name.  
   InitProperties[0].dwPropertyID = DBPROP_INIT_DATASOURCE;  
   InitProperties[0].vValue.vt = VT_BSTR;  
  
   // Replace "MySqlServer" with proper value.  
   InitProperties[0].vValue.bstrVal = SysAllocString(L"(local)");  
   InitProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid = DB_NULLID;  
  
   // Specify database name.  
   InitProperties[1].dwPropertyID = DBPROP_INIT_CATALOG;  
   InitProperties[1].vValue.vt = VT_BSTR;  
   InitProperties[1].vValue.bstrVal = SysAllocString(L"AdventureWorks");  
   InitProperties[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid = DB_NULLID;  
  
   InitProperties[2].dwPropertyID = DBPROP_AUTH_INTEGRATED;  
   InitProperties[2].vValue.vt = VT_BSTR;  
   InitProperties[2].vValue.bstrVal = SysAllocString(L"SSPI");  
   InitProperties[2].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid = DB_NULLID;  
  
   // Now that properties are set, construct the DBPROPSET structure  
   // (rgInitPropSet).  The DBPROPSET structure is used to pass an array  
   // of DBPROP structures (InitProperties) to SetProperties method.  
   rgInitPropSet[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties = 4;  
   rgInitPropSet[0].rgProperties = InitProperties;  
  
   // Set initialization properties.  
   hr = pIDBInitialize->QueryInterface( IID_IDBProperties, (void **)&pIDBProperties);  
   if (FAILED(hr))  
      cout << "Failed to obtain IDBProperties interface.\n";  
  
   hr = pIDBProperties->SetProperties( nPropSet, rgInitPropSet);  
   if (FAILED(hr))  
      cout << "Failed to set initialization properties\n";  
  
   pIDBProperties->Release();  
  
   // Now we establish connection to the data source.  
   if (FAILED(pIDBInitialize->Initialize()))  
      cout << "Problem in initializing\n";  
}  
```  
  
```sql
USE AdventureWorks  
drop function fn_RectangleArea  
go  
```  
  
## <a name="see-also"></a>另请参阅  
 [处理结果操作指南主题 (OLE DB)](../../../relational-databases/native-client-ole-db-how-to/results/processing-results-how-to-topics-ole-db.md)  
  
