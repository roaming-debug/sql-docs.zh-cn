---
description: catalog.check_schema_version
title: catalog.check_schema_version | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e0d5e9f5-59c6-4118-87b5-4aa5c37a7df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa47e5107551ec2d4ded0832ff52922ffb4bfd55
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247522"
---
# <a name="catalogcheck_schema_version"></a>catalog.check_schema_version 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  确定 SSISDB 目录架构与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 二进制文件（ISServerExec 和 SQLCLR 程序集）是否兼容。  
  
 如果该架构与这些二进制文件不兼容，则 ISServerExec.exc 将记录错误消息。  
  
 当 SSISDB 架构在应用程序修补和升级过程中发生更改时，该架构版本将递增。 建议您在还原 SSISDB 备份之后运行下面的存储过程，以确保架构和二进制文件兼容。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.check_schema_version [ @use32bitruntime = ] use32bitruntime  
```  
  
## <a name="arguments"></a>参数  
 [ @use32bitruntime= ] *use32bitruntime*  
 当此参数设置为 1 时，将调用 32 位版本的 dtexec。 use32bitruntime 为 int。  
  
 
## <a name="return-code-value"></a>返回代码值 
返回 0 表示成功。 

## <a name="result-set"></a>结果集  

返回具有以下格式的表：

| 列名称 | 数据类型 | 说明 |
|---|---|---|
| SERVER_BUILD | **decimal** | SQL Server 版本。 例如，运行 SQL Server 2014 的服务器为 `14.0.3335.7`。 |
| SCHEMA_VERSION | **tinyint** | SQL Server 版本号。 例如，SQL Server 2017 和 2019 分别为 `6` 和 `7`。|
| SCHEMA_BUILD | **string** | 架构内部版本。 |
| ASSEMBLY_BUILD | **string** | 程序集内部版本。 |
| SHARED_COMPONENT_VERSION | **string** | 共享组件版本。 | 

## <a name="permissions"></a>权限  
 此存储过程需要以下权限：  
  
-   **ssis_admin** 数据库角色的成员资格。  
  
  
