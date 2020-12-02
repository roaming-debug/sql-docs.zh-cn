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
ms.openlocfilehash: b8f1d04f2a41c247a3de8a66f5b07ee74f5036b5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129942"
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
  
## <a name="result-set"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要以下权限：  
  
-   **ssis_admin** 数据库角色的成员资格。  
  
  
