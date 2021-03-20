---
description: ALTER EXTERNAL DATA SOURCE (Transact-SQL)
title: ALTER EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER EXTERNAL DATA SOURCE
- ALTER_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- polybase, alter external data source statement
- ALTER EXTERNAL DATA SOURCE statement
ms.assetid: a34b9e90-199d-46d0-817a-a7e69387bf5f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 642c52250c5802968595e68c1b428ed9d1b2e668
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751887"
---
# <a name="alter-external-data-source-transact-sql"></a>ALTER EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  修改用于创建外部表的外部数据源。 外部数据源可以是适用于 SQL SERVER 的 Hadoop 和 Azure blob 存储 (WASBS) 或适用于 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 的 Azure blob 存储 (WASBS) 或 Azure Data Lake Storage (ABFSS/ADL)。 

## <a name="syntax"></a>语法  

```syntaxsql
-- Modify an external data source
-- Applies to: SQL Server (2016 or later) and APS
ALTER EXTERNAL DATA SOURCE data_source_name SET
    {   
        LOCATION = '<prefix>://<path>[:<port>]' [,] |
        RESOURCE_MANAGER_LOCATION = <'IP address;Port'> [,] |
        CREDENTIAL = credential_name
    }  
    [;]  

-- Modify an external data source pointing to Azure Blob storage
-- Applies to: SQL Server (starting with 2017)
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        LOCATION = 'https://storage_account_name.blob.core.windows.net'
        [, CREDENTIAL = credential_name ] 

-- Modify an external data source pointing to Azure Blob storage or Azure Data Lake storage
-- Applies to: Azure Synapse Analytics
ALTER EXTERNAL DATA SOURCE data_source_name
    SET
        [LOCATION = '<location prefix>://<location path>']
        [, CREDENTIAL = credential_name ] 
```

## <a name="arguments"></a>参数  
 data_source_name 指定数据源的用户定义名称。 该名称必须是唯一的。

 LOCATION =“<prefix>://<path>[:<port>]”：提供外部数据源的连接协议、路径和端口。 若要了解有效的位置选项，请参阅 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](create-external-data-source-transact-sql.md#location--prefixpathport)。

 RESOURCE_MANAGER_LOCATION = '\<IP address;Port>'（不适用于 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]） 指定 Hadoop 资源管理器位置。 如果指定，查询优化器可以选择通过使用 Hadoop 的计算功能预处理 PolyBase 查询的数据。 这是基于开销的决策。 这称为谓词下推，可以显著减少 Hadoop 和 SQL之间传输的数据量，并因此提高查询性能。

 CREDENTIAL = Credential_Name 指定命名凭据。 请参阅 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。

TYPE = [HADOOP | BLOB_STORAGE]   
适用对象：[!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]。
仅对于大容量操作，`LOCATION` 必须是有效的 Azure Blob 存储 URL。 请勿将 /、文件名或共享访问签名参数放在 `LOCATION` URL 的末尾。
必须使用 `SHARED ACCESS SIGNATURE` 作为标识创建所使用的凭据。 有关共享访问签名的详细信息，请参阅[使用共享访问签名 (SAS)](/azure/storage/storage-dotnet-shared-access-signature-part-1)。

  

## <a name="remarks"></a>备注
 一次只能修改一个源。 修改同一个源的并发请求会导致一个语句等待。 但是，可以同时修改不同的源。 此语句可以与其他语句同时运行。

## <a name="permissions"></a>权限  
 需要 ALTER ANY EXTERNAL DATA SOURCE 权限。
 > [!IMPORTANT]  
 > ALTER ANY EXTERNAL DATA SOURCE 权限授予任何主体创建和修改任何外部数据源对象的权限，还授予访问数据库上所有数据库作用域凭据的权限。 必须将此权限视为高度特权，因此必须仅授予系统中受信任的主体。


## <a name="examples"></a>示例  
 下面的示例更改了现有数据源的位置和资源管理器位置。

```sql  
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
     LOCATION = 'hdfs://10.10.10.10:8020',
     RESOURCE_MANAGER_LOCATION = '10.10.10.10:8032'
    ;
```

 下面的示例更改了用于连接到现有数据源的凭据。

```sql 
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
   CREDENTIAL = new_hadoop_user
    ;
```
 以下示例将凭证更改为新位置。 此示例是为 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 创建的外部数据源。 

```sql  
ALTER EXTERNAL DATA SOURCE AzureStorage_west SET
   LOCATION = 'wasbs://loadingdemodataset@updatedproductioncontainer.blob.core.windows.net',
   CREDENTIAL = AzureStorageCredential
```