---
title: 将数据分类与 Microsoft OLE DB Driver for SQL Server 结合使用 | Microsoft Docs
description: 了解如何使用 Microsoft OLE DB Driver for SQL Server 来获取分类信息。
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- OLE DB Driver for SQL Server, data classification
author: bazizi
ms.author: v-beaziz
manager: kenvh
ms.openlocfilehash: 2a46b952815fa707b03889d3f9aa6480e6e13d32
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506642"
---
# <a name="using-data-classification"></a>使用数据分类
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="overview"></a>概述
[SQL 数据发现和分类](https://docs.microsoft.com/sql/relational-databases/security/sql-data-discovery-and-classification)是一组高级服务，可用于发现数据库中的敏感信息并对其进行分类、标记和报告。 Microsoft OLE DB Driver for SQL Server（版本 [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850)）添加了在基础数据源支持该功能时检索分类元数据的支持。 可以通过 [ISSDataClassification](../ole-db-interfaces/issdataclassification-ole-db.md) 接口访问此信息。

有关如何将分类分配到列的详细信息，请参阅 [SQL 数据发现和分类](https://docs.microsoft.com/sql/relational-databases/security/sql-data-discovery-and-classification)。

## <a name="code-samples"></a>代码示例

可在 SSMS 中执行以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 查询，为示例 C++ 应用程序设置先决条件：

```sql
CREATE DATABASE [mydb]
GO

USE [mydb]
GO

CREATE TABLE [dbo].[mytable](
    [col1] [int] NULL,
    [col2] [int] NULL
)
GO

ADD SENSITIVITY CLASSIFICATION TO [dbo].[mytable].[col1] WITH (label = 'Label1', label_id = 'LabelId1', information_type = 'Type1', information_type_id = 'TypeId1', rank = Medium)
GO

ADD SENSITIVITY CLASSIFICATION TO [dbo].[mytable].[col2] WITH (label = 'Label2', label_id = 'LabelId2', information_type = 'Type2', information_type_id = 'TypeId2', rank = High)
```

以下 C++ 代码使用 Microsoft OLE DB Driver 来获取使用上述 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 查询生成的分类信息：
```cpp
#include <atlbase.h>
#include <msdasc.h>
#include <exception>
#include <iostream>
#include <string>
#include "msoledbsql.h"

void Connect(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* server, const wchar_t* database);
SENSITIVITYCLASSIFICATION* GetSensitivityClassificationInfo(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* query);
void PrintSensitivityClassificationInfo(SENSITIVITYCLASSIFICATION* pSensitivityClassification);

int main()
{
    const wchar_t server[] = L"myserver";
    const wchar_t database[] = L"mydb";
    const wchar_t query[] = L"SELECT col1, col2, col1 + col2 FROM mytable";

    CoInitialize(nullptr);

    try
    {
        // Connect to data source
        CComPtr<IDBInitialize> pIDBInitialize;
        Connect(pIDBInitialize, server, database);

        // Obtain sensitivity classification info
        SENSITIVITYCLASSIFICATION* pSensitivityClassification = GetSensitivityClassificationInfo(pIDBInitialize, query);

        // Print sensitivity classification info
        PrintSensitivityClassificationInfo(pSensitivityClassification);

        if (pSensitivityClassification)
        {
            CComPtr<IMalloc> pIMalloc;
            if (FAILED(CoGetMalloc(1, &pIMalloc)))
            {
                throw std::exception("CoGetMalloc call failed.");
            }

            // Release memory
            pIMalloc->Free(pSensitivityClassification);
        }
    }
    catch (std::exception& e)
    {
        std::cerr << "Exception caught: " << e.what() << std::endl;
        return 1;
    }

    CoUninitialize();
    return 0;
}

void Connect(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* server, const wchar_t* database)
{
    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(server) + L";Database=" +
        std::wstring(database) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    CComPtr<IDataInitialize> pIDataInitialize;
    if (FAILED(CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize))))
    {
        throw std::exception("CoCreateInstance call failed.");
    }

    if (FAILED(pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize))))
    {
        throw std::exception("GetDataSource call failed.");
    }

    if (FAILED(pIDBInitialize->Initialize()))
    {
        throw std::exception("Initialize call failed.");
    }
}

SENSITIVITYCLASSIFICATION* GetSensitivityClassificationInfo(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* query)
{
    CComPtr<IDBCreateSession> pIDBCreateSession;
    if (FAILED(pIDBInitialize.QueryInterface<IDBCreateSession>(&pIDBCreateSession)))
    {
        throw std::exception("QueryInterface call failed.");
    }

    CComPtr<IDBCreateCommand> pIDBCreateCommand;
    if (FAILED(pIDBCreateSession->CreateSession(nullptr, IID_IDBCreateCommand, reinterpret_cast<IUnknown**>(&pIDBCreateCommand))))
    {
        throw std::exception("CreateSession call failed.");
    }

    CComPtr<ICommandText> pICommandText;
    if (FAILED(pIDBCreateCommand->CreateCommand(nullptr, IID_ICommandText, reinterpret_cast<IUnknown**>(&pICommandText))))
    {
        throw std::exception("CreateCommand call failed.");
    }

    if (FAILED(pICommandText->SetCommandText(DBGUID_DBSQL, query)))
    {
        throw std::exception("SetCommandText call failed.");
    }

    CComPtr<ISSDataClassification> pISSDataClassification;
    if (FAILED(pICommandText->Execute(nullptr, IID_ISSDataClassification, nullptr, nullptr, reinterpret_cast<IUnknown**>(&pISSDataClassification))))
    {
        throw std::exception("Execute call failed.");
    }

    SENSITIVITYCLASSIFICATION* pSensitivityClassification = nullptr;
    if (FAILED(pISSDataClassification->GetSensitivityClassification(&pSensitivityClassification)))
    {
        throw std::exception("GetSensitivityClassification call failed.");
    }

    return pSensitivityClassification;
}

void PrintSensitivityClassificationInfo(SENSITIVITYCLASSIFICATION* pSensitivityClassification)
{
    if (!pSensitivityClassification)
    {
        return;
    }

    if (pSensitivityClassification->eQuerySensitivityRank != SENSITIVITYRANK_NOT_DEFINED)
    {
        std::wcout << L"Query sensitivity rank: " << pSensitivityClassification->eQuerySensitivityRank << L"\n\n";
    }

    for (USHORT colIdx = 0; colIdx < pSensitivityClassification->cColumnSensitivityMetadata; ++colIdx)
    {
        const COLUMNSENSITIVITYMETADATA& columnMetadata = pSensitivityClassification->rgColumnSensitivityMetadata[colIdx];

        std::wcout << L"Sensitivity classification for column #" << colIdx << L":" << std::endl;
        for (USHORT propIdx = 0; propIdx < columnMetadata.cSensitivityProperties; ++propIdx)
        {
            const SENSITIVITYPROPERTY& prop = columnMetadata.rgSensitivityProperties[propIdx];

            std::wcout << L"Property #" << propIdx << L":" << std::endl;

            if (prop.eSensitivityRank != SENSITIVITYRANK_NOT_DEFINED)
            {
                std::wcout << L"\tSensitivity rank: \t" << prop.eSensitivityRank << std::endl;
            }

            if (prop.pSensitivityLabel)
            {
                if (prop.pSensitivityLabel->pwszId)
                {
                    std::wcout << L"\tSensitivity label id: \t" << prop.pSensitivityLabel->pwszId << std::endl;
                }
                if (prop.pSensitivityLabel->pwszName)
                {
                    std::wcout << L"\tSensitivity label name: " << prop.pSensitivityLabel->pwszName << std::endl;
                }
            }

            if (prop.pInformationType)
            {
                if (prop.pInformationType->pwszId)
                {
                    std::wcout << L"\tInformation type id: \t" << prop.pInformationType->pwszId << std::endl;
                }
                if (prop.pInformationType->pwszName)
                {
                    std::wcout << L"\tInformation type name: \t" << prop.pInformationType->pwszName << std::endl;
                }
            }

            std::wcout << std::endl;
        }
    }
}
```

## <a name="see-also"></a>请参阅
 [接口 (OLE DB)](../ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)  
 [ISSDataClassification](../ole-db-interfaces/issdataclassification-ole-db.md)  

