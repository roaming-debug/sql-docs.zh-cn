---
title: 使用大值类型
description: 了解 OLE DB Driver for SQL Server 如何支持带有 varchar(max)、varbinary(max) 和 nvarchar(max) 类型的大值数据类型。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- large value data types
- MSOLEDBSQL, large value types
- OLE DB Driver for SQL Server, large value types
- data access [OLE DB Driver for SQL Server], large value types
- OLE DB Driver for SQL Server, large value data types
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cf390ad99ea840a2f464402e18a7660d6fab0e4d
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793096"
---
# <a name="using-large-value-types"></a>使用大值类型

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前，若要使用大值数据类型，必须进行特殊的处理。 大值数据类型是超过了 8 KB 最大行大小的类型。 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 为 varchar、nvarchar 和 varbinary 数据类型引入了一个 max 说明符，以允许存储最长可达 2^31 -1 个字节的值     。 表列和 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 变量可以指定 varchar(max)、nvarchar(max) 或 varbinary(max) 数据类型    。

> [!NOTE]
> 大值数据类型的最大大小可以介于 1 KB 到 8 KB 之间，也可以指定为不限制其大小。

以前，只有 text、ntext 和 image 之类的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型可以达到此大小。 varchar、nvarchar 和 varbinary 的 max 说明符使这些数据类型也可以达到这样的长度     。 但是，由于仍然提供长数据类型，因而大多数 OLE DB 数据访问组件的接口将保持不变。 为了实现与旧版本的向后兼容性，OLE DB Driver for SQL Server 中的 DBCOLUMNFLAGS_ISLONG 标志仍在使用中。 针对 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本编写的访问接口和驱动程序可以继续使用这些字词将新类型设置为最大长度不受限制。

> [!NOTE]
> 还可以将 varchar(max)、nvarchar(max) 和 varbinary(max) 数据类型指定为存储过程的输入和输出参数类型、函数返回类型或者用在 [CAST 和 CONVERT](../../../t-sql/functions/cast-and-convert-transact-sql.md) 函数中  。

> [!NOTE]
> 如果复制数据，可能需要将 [max text repl size 服务器配置选项](../../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md)设置为 -1。

## <a name="ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序

适用于 SQL Server 的 OLE DB 驱动程序将 varchar(max)、varbinary(max) 和 nvarchar(max) 类型分别公开为 DBTYPE_STR、DBTYPE_BYTES 和 DBTYPE_WSTR    。

如果列中的 varchar(max)、varbinary(max) 和 nvarchar(max) 数据类型的 max 大小设置为不受限制，则这些数据类型会通过返回列数据类型的核心 OLE DB 架构行集和接口表示为 ISLONG   。

命令对象的 IAccessor  实现已更改为允许绑定为 DBTYPE_IUNKNOWN。 如果使用者指定 DBTYPE_IUNKNOWN 并将 pObject 设置为 Null，则提供程序将向使用者返回 ISequentialStream 接口，以便使用者可以对输出变量之外的 varchar(max)、nvarchar(max) 或 varbinary(max) 数据进行流式处理      。

将在所有结果行之后返回经过流式处理的输出参数值。 如果应用程序尝试通过调用 IMultipleResults::GetResult 移动到下一个结果集并且没有使用返回的所有输出参数值，则将返回 DB_E_OBJECTOPEN  。

为了支持流式处理，适用于 SQL Server 的 OLE DB 驱动程序要求按顺序访问可变长度参数。 此要求意味着，只要 varchar(max)、nvarchchar(max) 或 varbinary(max) 列或输出参数绑定到 DBTYPE_IUNKNOWN，DBPROP_ACCESSORDER 就必须设置为 DBPROPVAL_AO_SEQUENTIALSTORAGEOBJECTS 或 DBPROPVAL_AO_SEQUENTIAL  。 如果不遵守此访问顺序限制，对 IRowset::GetData 的调用将失败，并出现 DBSTATUS_E_UNAVAILABLE  。 如果不存在使用 DBTYPE_IUNKNOWN 的任何输出绑定，则不会应用此限制。

适用于 SQL Server 的 OLE DB 驱动程序还支持针对大值数据类型将输出参数绑定为 DBTYPE_IUNKNOWN，以利于以下方案：存储过程的返回值为大值类型并且作为 DBTYPE_IUNKNOWN 向客户端公开。

若要使用这些类型，应用程序可以使用以下选项：

- 绑定为支持与列的基类型进行绑定的类型（例如，对于 nvarchar(max)，绑定为可绑定到 nvarchar 的类型）。 如果缓冲区不够大，将发生截断，以便与基类型完全相同，虽然现在可以使用更大的值。
- 绑定为支持与列的基类型进行转换的类型，同时还指定 DBTYPE_BYREF。
- 绑定为 DBTYPE_IUNKNOWN 并使用流处理。

在报告列的最大大小时，OLE DB Driver for SQL Server 会报告：

- 定义的最大大小，例如，对于 varchar(2000) 列将会报告 2000，或者  
- “unlimited”值，对于 varchar(max) 列而言此值等于 ~0。 此值是为 DBCOLUMN_COLUMNSIZE 元数据属性设置的。

将向 varchar(max) 列应用的标准转换规则，也就是说，对于 varchar(2000) 列有效的任何转换对于 varchar(max) 列也有效     。 这也适用于 nvarchar(max) 和 varbinary(max) 列   。

在检索大值类型时，最有效的方法是绑定为 DBTYPE_IUNKNOWN 并将行集属性 DBPROP_ACCESSORDER 设置为 DBPROPVAL_AO_SEQUENTIALSTORAGEOBJECTS。 此设置会导致该值直接从网络上进行流式处理，而不进行中间缓冲处理，如下例所示：

```cpp
#define UNICODE
#define _UNICODE
#define DBINITCONSTANTS
#define INITGUID
#define OLEDBVER 0x0250  // To include the correct interfaces.

#include <stdio.h>
#include <tchar.h>
#include <stddef.h>
#include <iostream>

using std::cout;
using std::endl;

#include <windows.h>

#include <oledb.h>
#include "msoledbsql.h"
#include <oledberr.h>

#define CHKHR_GOTO(hr, errMsg, Label) \
   if (FAILED(hr)) \
   { \
      cout << errMsg << endl; \
      goto Label; \
   }

#define MAX_COL_SIZE 8000

// ROUNDUP on all platforms pointers must be aligned properly.
#define ROUNDUP_AMOUNT 8
#define ROUNDUP_(size,amount) (((ULONG)(size)+((amount)-1))&~((amount)-1))
#define ROUNDUP(size) ROUNDUP_(size, ROUNDUP_AMOUNT)

HRESULT InitializeAndEstablishConnection(IDBInitialize** ppIDBInitialize);
void UnInitializeConnection(IDBInitialize* pIDBInitialize);
HRESULT CreateAndSetCommand(IDBInitialize* pIDBInitialize, ICommandText** ppICommandText);
HRESULT ProcessResultSet(IRowset* pIRowset);

void DisplayTime()
{
   SYSTEMTIME st;
   GetSystemTime(&st);
   cout<< st.wHour << ":" << st.wMinute << ":" << st.wSecond << "." << st.wMilliseconds << endl;
}

void main()
{
   HRESULT hr;
   IDBInitialize* pIDBInitialize = NULL;
   ICommandText* pICommandText = NULL;
   IMultipleResults* pIMultipleResults = NULL;
   IRowset* pIRowset = NULL;

   hr = InitializeAndEstablishConnection(&pIDBInitialize);
   CHKHR_GOTO(hr, L"Failed to establish connection.", _ExitMain);

   hr = CreateAndSetCommand(pIDBInitialize, &pICommandText);
   CHKHR_GOTO(hr, L"Failed to set up command object.", _ExitMain);

   DisplayTime();

   hr = pICommandText->Execute(NULL,
      IID_IMultipleResults,
      NULL,
      NULL,
     (IUnknown **) &pIMultipleResults);

   CHKHR_GOTO(hr, L"Failed to execute command.", _ExitMain);

   while (1)
   {
      hr = pIMultipleResults->GetResult(
         NULL,
         DBRESULTFLAG_DEFAULT,
         IID_IRowset,
         NULL,
         (IUnknown**)&pIRowset);

   CHKHR_GOTO(hr, L"Failed to obtain a results from MR object.", _ExitMain);

   if (hr == DB_S_NORESULT)
      break;

      if (pIRowset)
      {
         hr = ProcessResultSet(pIRowset);
         CHKHR_GOTO(hr, L"Failed to process the current Rowset.", _ExitMain);

         pIRowset->Release();
         pIRowset = NULL;
      }
   }

   DisplayTime();

_ExitMain:

   if (pIRowset)
   {
      pIRowset->Release();
      pIRowset = NULL;
   }

   if (pIMultipleResults)
   {
      pIMultipleResults->Release();
      pIMultipleResults = NULL;
   }

   if (pICommandText)
   {
      pICommandText->Release();
      pICommandText = NULL;
   }

   UnInitializeConnection(pIDBInitialize);
   return;
};

HRESULT InitializeAndEstablishConnection(IDBInitialize** ppIDBInitialize)
{
   HRESULT hr;
   IDBInitialize* pIDBInitialize = NULL;
   IDBProperties* pIDBProperties = NULL;

   const int NUM_DBINIT_PROPS = 3;
   const wchar_t* const g_wszServer = L".";
   const wchar_t* const g_wszCatalog = L"AdventureWorks";
   const wchar_t* const g_wszSecurity = L"SSPI";

   DBPROPSET rgdbPropSetInit[1];
   DBPROP rgdbPropInit [NUM_DBINIT_PROPS];

   *ppIDBInitialize = NULL;
   hr = CoInitialize(NULL);
   CHKHR_GOTO(hr, L"Failed to initialize COM.", _ExitInitialize);

   hr = CoCreateInstance(CLSID_MSOLEDBSQL,
      NULL,
      CLSCTX_INPROC_SERVER,
      IID_IDBInitialize,
      (void**)&pIDBInitialize);

   CHKHR_GOTO(hr, L"Failed to create MSOLEDBSQL DataSource object.", _ExitInitialize);

   for(int idxProp = 0; idxProp < NUM_DBINIT_PROPS; idxProp++)
   {
      VariantInit(&rgdbPropInit[idxProp].vValue);
   }

   rgdbPropInit[0].dwPropertyID = DBPROP_INIT_DATASOURCE;
   rgdbPropInit[0].vValue.vt = VT_BSTR;
   rgdbPropInit[0].vValue.bstrVal= SysAllocString(g_wszServer);
   rgdbPropInit[0].dwOptions = DBPROPOPTIONS_REQUIRED;
   rgdbPropInit[0].colid = DB_NULLID;

   if (rgdbPropInit[0].vValue.bstrVal == NULL)
   {
      hr = E_OUTOFMEMORY;
      goto _ExitInitialize;
   }

   rgdbPropInit[1].dwPropertyID = DBPROP_INIT_CATALOG;
   rgdbPropInit[1].vValue.vt = VT_BSTR;
   rgdbPropInit[1].vValue.bstrVal= SysAllocString(g_wszCatalog);
   rgdbPropInit[1].dwOptions = DBPROPOPTIONS_REQUIRED;
   rgdbPropInit[1].colid = DB_NULLID;

   if (rgdbPropInit[1].vValue.bstrVal == NULL)
   {
      hr = E_OUTOFMEMORY;
      goto _ExitInitialize;
   }

   rgdbPropInit[2].dwPropertyID = DBPROP_AUTH_INTEGRATED;
   rgdbPropInit[2].vValue.vt = VT_BSTR;
   rgdbPropInit[2].vValue.bstrVal= SysAllocString(g_wszSecurity);
   rgdbPropInit[2].dwOptions = DBPROPOPTIONS_REQUIRED;
   rgdbPropInit[2].colid = DB_NULLID;

   if (rgdbPropInit[2].vValue.bstrVal == NULL)
   {
      hr = E_OUTOFMEMORY;
      goto _ExitInitialize;
   }

   rgdbPropSetInit[0].guidPropertySet = DBPROPSET_DBINIT;
   rgdbPropSetInit[0].cProperties = NUM_DBINIT_PROPS;
   rgdbPropSetInit[0].rgProperties = rgdbPropInit;

   hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
   CHKHR_GOTO(hr, L"Failed to QI DataSource object for IDBProperties.", _ExitInitialize);

   hr = pIDBProperties->SetProperties(1, rgdbPropSetInit);
   CHKHR_GOTO(hr, L"Failed to set DataSource object Properties.", _ExitInitialize);

   pIDBProperties->Release();
   pIDBProperties = NULL;

   hr = pIDBInitialize->Initialize();
   CHKHR_GOTO(hr, L"Failed to establish connection with the server.", _ExitInitialize);

_ExitInitialize:

   if (pIDBProperties)
   {
      pIDBProperties->Release();
      pIDBProperties = NULL;
   }

   if (FAILED(hr))
   {
      if (pIDBInitialize)
      {
         pIDBInitialize->Release();
         pIDBInitialize = NULL;
      }
   }

   *ppIDBInitialize = pIDBInitialize;
   return hr;
}

void UnInitializeConnection(IDBInitialize* pIDBInitialize)
{
   if (pIDBInitialize)
   {
      pIDBInitialize->Uninitialize();
      pIDBInitialize->Release();
      pIDBInitialize = NULL;
   }
   CoUninitialize();
}

HRESULT CreateAndSetCommand(IDBInitialize* pIDBInitialize, ICommandText** ppICommandText)
{
   HRESULT hr;
   IDBCreateSession* pIDBCreateSession = NULL;
   IDBCreateCommand* pIDBCreateCommand = NULL;
   ICommandText* pICommandText = NULL;
   ICommandProperties* pICommandProperties = NULL;
   DBPROPSET rgCmdPropSet[1];
   DBPROP rgCmdProperties[1];

const wchar_t* const g_wCmdString = L"declare @x xml, @y nvarchar(max); select @x = (SELECT * FROM Sales.SalesOrderHeader FOR XML AUTO); select @x;";

   *ppICommandText = NULL;

   if (!pIDBInitialize)
   {
      hr = E_FAIL;
      goto _ExitCreateAndSetCommand;
   }

   hr = pIDBInitialize->QueryInterface(IID_IDBCreateSession, (void**) &pIDBCreateSession);
   CHKHR_GOTO(hr, L"Failed to obtain IDBCreateSession interface from DSO.", _ExitCreateAndSetCommand);

   hr = pIDBCreateSession->CreateSession(
      NULL,
      IID_IDBCreateCommand,
      (IUnknown**) &pIDBCreateCommand);

   CHKHR_GOTO(hr, L"Failed to Create a Session for command execution.", _ExitCreateAndSetCommand);

   hr = pIDBCreateCommand->CreateCommand(
      NULL,
      IID_ICommandText,
      (IUnknown**)&pICommandText);

   CHKHR_GOTO(hr, L"Failed to Create a Command object.", _ExitCreateAndSetCommand);

   hr = pICommandText->SetCommandText(DBGUID_DBSQL, g_wCmdString);
   CHKHR_GOTO(hr, L"Failed to Set Command Text.", _ExitCreateAndSetCommand);

   hr = pICommandText->QueryInterface(IID_ICommandProperties, (void**) &pICommandProperties);
   CHKHR_GOTO(hr, L"Failed to obtain ICommandProperties interface from the command object.", _ExitCreateAndSetCommand);

   rgCmdProperties[0].dwPropertyID = DBPROP_ACCESSORDER;
   rgCmdProperties[0].vValue.vt = VT_I4;
   rgCmdProperties[0].vValue.lVal = DBPROPVAL_AO_SEQUENTIAL;
   rgCmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;
   rgCmdProperties[0].colid = DB_NULLID;

   rgCmdPropSet[0].guidPropertySet = DBPROPSET_ROWSET;
   rgCmdPropSet[0].cProperties = 1;
   rgCmdPropSet[0].rgProperties = rgCmdProperties;

   hr = pICommandProperties->SetProperties(1, rgCmdPropSet);
   CHKHR_GOTO(hr, L"Failed to Set Command object Properties.", _ExitCreateAndSetCommand);

_ExitCreateAndSetCommand:

   if (pICommandProperties)
   {
      pICommandProperties->Release();
      pICommandProperties = NULL;
   }

   if (pIDBCreateCommand)
   {
      pIDBCreateCommand->Release();
      pIDBCreateCommand = NULL;
   }

   if (pIDBCreateSession)
   {
      pIDBCreateSession->Release();
      pIDBCreateSession = NULL;
   }

   if (FAILED(hr))
   {
      if (pICommandText)
      {
         pICommandText->Release();
         pICommandText = NULL;
      }
   }

   *ppICommandText = pICommandText;
   return hr;
}

HRESULT ProcessResultSet(IRowset* pIRowset)
{
   HRESULT hr;

   IColumnsInfo* pIColumnsInfo = NULL;
   DBCOLUMNINFO* pDBColumnInfo = NULL;
   ULONG lNumCols = 0;
   wchar_t* pStringsBuffer = NULL;

   DBBINDING* pBindings = NULL;
   DBOBJECT dbobj;
   ULONG idxBinding;
   IAccessor* pIAccessor = NULL;
   HACCESSOR hAccessor = DB_NULL_HACCESSOR;
   HROW hRows[1] = {DB_NULL_HROW};
   HROW* pRow = &hRows[0];
   BYTE* pBuffer = NULL;

   ULONG lNumRowsRetrieved;
   DBLENGTH dwOffset = 0;

   hr = pIRowset->QueryInterface(IID_IColumnsInfo, (void **)&pIColumnsInfo);
   CHKHR_GOTO(hr, L"Failed to QI Rowset for IColumnsInfo.", _ExitProcessResultSet);

   hr = pIColumnsInfo->GetColumnInfo(&lNumCols, &pDBColumnInfo, &pStringsBuffer);
   CHKHR_GOTO(hr, L"Failed to obtain Column Information.", _ExitProcessResultSet);

   pBindings = new DBBINDING[lNumCols];

   if (!pBindings)
   {
      hr = E_OUTOFMEMORY;
      goto _ExitProcessResultSet;
   }

   memset(pBindings, 0, sizeof(DBBINDING) * lNumCols);

   dbobj.dwFlags = STGM_READ;
   dbobj.iid = IID_ISequentialStream;

   for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)
   {
      pBindings[idxBinding].iOrdinal = idxBinding + 1;
      pBindings[idxBinding].obStatus = dwOffset;
      pBindings[idxBinding].obLength = dwOffset + sizeof(DBSTATUS);
      pBindings[idxBinding].obValue = dwOffset + sizeof(DBSTATUS) + sizeof(DBLENGTH);

      pBindings[idxBinding].pTypeInfo = NULL;
      pBindings[idxBinding].pBindExt = NULL;
      pBindings[idxBinding].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;
      pBindings[idxBinding].dwMemOwner = DBMEMOWNER_CLIENTOWNED;
      pBindings[idxBinding].eParamIO = DBPARAMIO_NOTPARAM;
      pBindings[idxBinding].bPrecision = pDBColumnInfo[idxBinding].bPrecision;
      pBindings[idxBinding].bScale = pDBColumnInfo[idxBinding].bScale;

      pBindings[idxBinding].cbMaxLen = 0;
      pBindings[idxBinding].wType = DBTYPE_WSTR;

   // Determine the maximum number of bytes required in our buffer to
   // contain the Unicode string representation of the provider's native
   // data type, including room for the NULL-termination character
   switch( pDBColumnInfo[idxBinding].wType )
   {
      case DBTYPE_NULL:
      case DBTYPE_EMPTY:
      case DBTYPE_I1:
      case DBTYPE_I2:
      case DBTYPE_I4:
      case DBTYPE_UI1:
      case DBTYPE_UI2:
      case DBTYPE_UI4:
      case DBTYPE_R4:
      case DBTYPE_BOOL:
      case DBTYPE_I8:
      case DBTYPE_UI8:
      case DBTYPE_R8:
      case DBTYPE_CY:
      case DBTYPE_ERROR:
      // When the above types are converted to a string, they
      // will all fit into 25 characters, so use that plus space
      // for the NULL-terminator.

      pBindings[idxBinding].cbMaxLen = (25 + 1) * sizeof(WCHAR);
      break;

      case DBTYPE_DECIMAL:
      case DBTYPE_NUMERIC:
      case DBTYPE_DATE:
      case DBTYPE_DBDATE:
      case DBTYPE_DBTIMESTAMP:
      case DBTYPE_GUID:
      // Converted to a string, the above types will all fit into
      // 50 characters, so use that plus space for the terminator.

      pBindings[idxBinding].cbMaxLen = (50 + 1) * sizeof(WCHAR);
      break;

      case DBTYPE_BYTES:
      // In converting DBTYPE_BYTES to a string, each byte
      // becomes two characters (e.g. 0xFF -> "FF"), so we
      // will use double the maximum size of the column plus
      // include space for the NULL-terminator.

      pBindings[idxBinding].cbMaxLen = (pDBColumnInfo[idxBinding].ulColumnSize * 2 + 1) * sizeof(WCHAR);
      break;

      case DBTYPE_STR:
      case DBTYPE_WSTR:
      case DBTYPE_BSTR:
      // Going from a string to our string representation,
      // we can just take the maximum size of the column,
      // a count of characters, and include space for the
      // terminator, which is not included in the column size.

      pBindings[idxBinding].cbMaxLen = (pDBColumnInfo[idxBinding].ulColumnSize + 1) * sizeof(WCHAR);
      break;

      default:
      // For any other type, we will simply use our maximum
      // column buffer size, since the display size of these
      // columns may be variable (e.g. DBTYPE_VARIANT) or
      // unknown (e.g. provider-specific types).
      pBindings[idxBinding].cbMaxLen = MAX_COL_SIZE;
      break;
   }

   // If the provider's native data type for this column is
   // DBTYPE_IUNKNOWN or this is a BLOB column and the user
   // has requested that we bind BLOB columns as ISequentialStream
   // objects, bind this column as an ISequentialStream object if
   // the provider supports our creating another ISequentialStream
   // binding.
   if(pDBColumnInfo[idxBinding].dwFlags & DBCOLUMNFLAGS_ISLONG)
   {
      pBindings[idxBinding].wType = DBTYPE_IUNKNOWN;

      pBindings[idxBinding].cbMaxLen = sizeof(ISequentialStream*);

      pBindings[idxBinding].pObject = (DBOBJECT *)CoTaskMemAlloc(sizeof(DBOBJECT));

      if (!pBindings[idxBinding].pObject)
      {
         hr = E_OUTOFMEMORY;
         goto _ExitProcessResultSet;
      }

      // Direct the provider to create an ISequentialStream
      // object over the data for this column.
      pBindings[idxBinding].pObject->iid = IID_ISequentialStream;

      // We want read access on the ISequentialStream
      // object that the provider will create for us
      pBindings[idxBinding].pObject->dwFlags = STGM_READ;
      }

      // Ensure that the bound maximum length is no more than the
      // maximum column size in bytes that we've defined.
      pBindings[idxBinding].cbMaxLen = min(pBindings[idxBinding].cbMaxLen, MAX_COL_SIZE);

      // Update the offset past the end of this column's data, so
      // that the next column will begin in the correct place in
      // the buffer.
      dwOffset = pBindings[idxBinding].cbMaxLen + pBindings[idxBinding].obValue;

      // Ensure that the data for the next column will be correctly
      // aligned for all platforms, or, if we're done with columns,
      // that if we allocate space for multiple rows that the data
      // for every row is correctly aligned.
      dwOffset = ROUNDUP(dwOffset);
   }

   hr = pIRowset->QueryInterface(IID_IAccessor, (void **) &pIAccessor);
   CHKHR_GOTO(hr, L"Failed to obtain Accessor interface", _ExitProcessResultSet);

   hr = pIAccessor->CreateAccessor(DBACCESSOR_ROWDATA,
      lNumCols,
      pBindings,
      0,
      &hAccessor,
      NULL);

   CHKHR_GOTO(hr, L"Failed to create Accessor", _ExitProcessResultSet);
   for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)
   {
      cout << pDBColumnInfo[idxBinding].pwszName << endl;
   }

   lNumRowsRetrieved = 0;

   hr = pIRowset->GetNextRows(
      NULL,
      0,
      1,
      &lNumRowsRetrieved,
      &pRow);

   CHKHR_GOTO(hr, L"Failed to fetch a row from the rowset", _ExitProcessResultSet);

   pBuffer = new BYTE[sizeof(DBSTATUS) + sizeof(DBLENGTH) + sizeof(IUnknown*)];

   if (!pBuffer)
   {
      hr = E_OUTOFMEMORY;
      goto _ExitProcessResultSet;
   }

   while(lNumRowsRetrieved && hr != DB_S_ENDOFROWSET)
   {
      memset(pBuffer, 0, sizeof(DBSTATUS) + sizeof(DBLENGTH) + sizeof(IUnknown*));

      hr = pIRowset->GetData(hRows[0], hAccessor, pBuffer);
      CHKHR_GOTO(hr, L"Failed to obtain row data", _ExitProcessResultSet);

      for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)
      {
         if (pBindings[idxBinding].wType == DBTYPE_IUNKNOWN)
         {
            BYTE pbBuff[3000];
            ULONG cbNeeded = sizeof(pbBuff)/sizeof(BYTE);
            ULONG cbRead;
            ULONG cbReadTotal = 0;
            ISequentialStream* pISequentialStream = NULL;

            IUnknown* pIUnknown = *((IUnknown**)(pBuffer + pBindings[idxBinding].obValue));
            pIUnknown->QueryInterface(IID_ISequentialStream, (void**)&pISequentialStream);

            do
            {
               hr = pISequentialStream->Read(pbBuff, cbNeeded, &cbRead);
               cbReadTotal += cbRead;
            }
            while (SUCCEEDED(hr) && hr != S_FALSE && cbRead == cbNeeded);

               cout << "Total Bytes Read: " << cbReadTotal << endl;

               pISequentialStream->Release();
               pISequentialStream = NULL;
               pIUnknown->Release();
               pIUnknown = NULL;
            }
         }

         pIRowset->ReleaseRows(1, pRow, NULL, NULL, NULL);

         hr = pIRowset->GetNextRows(NULL,
            0,
            1,
            &lNumRowsRetrieved,
            &pRow);

         CHKHR_GOTO(hr, L"Failed to fetch a row from the rowset.", _ExitProcessResultSet);
   }

_ExitProcessResultSet:

   pIRowset->ReleaseRows(1, pRow, NULL, NULL, NULL);
   delete [] pBuffer;

   if (pIAccessor)
   {
      if (hAccessor != DB_NULL_HACCESSOR)
      {
         pIAccessor->ReleaseAccessor(hAccessor, NULL);
      }

      pIAccessor->Release();
      pIAccessor = NULL;
   }

   if (pBindings)
   {
      for (idxBinding = 0; idxBinding < lNumCols; idxBinding++)
      {
         if (pBindings[idxBinding].pObject)
         CoTaskMemFree(pBindings[idxBinding].pObject);
      }
   }

   delete [] pBindings;

   CoTaskMemFree(pDBColumnInfo);
   CoTaskMemFree(pStringsBuffer);

   if (pIColumnsInfo)
   {
      pIColumnsInfo->Release();
      pIColumnsInfo = NULL;
   }

   return hr;
}
```

若要详细了解 OLE DB Driver for SQL Server 如何公开大值数据类型，请参阅 [BLOB 和 OLE 对象](../../oledb/ole-db-blobs/blobs-and-ole-objects.md)。

## <a name="see-also"></a>另请参阅

[适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)
