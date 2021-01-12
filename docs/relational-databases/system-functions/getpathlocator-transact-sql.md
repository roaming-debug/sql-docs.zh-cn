---
description: GetPathLocator (Transact-SQL)
title: GetPathLocator (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- GetPathLocator
- GetPathLocator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetPathLocator function
ms.assetid: 78b7e220-445b-4fdf-811b-7253f4f2b058
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 155921b08c936bfb758f60c65b4ad737546141f5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096347"
---
# <a name="getpathlocator-transact-sql"></a>GetPathLocator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 FileTable 中指定文件或目录的路径定位器 ID 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
GetPathLocator(filenamespace_path)  
```  
  
## <a name="arguments"></a>参数  
 *filenamespace_path*  
 FileTable 中的命名空间路径。 命名空间路径的类型为 **nvarchar (max)**。  
  
 当数据库属于 Always On 可用性组时， **GetPathLocator** 函数将接受虚拟网络名称 (VNN) 或计算机名称。  
  
## <a name="return-type"></a>返回类型  
 **hierarchyid**  
  
## <a name="general-remarks"></a>一般备注  
 有关详细信息，请参阅 [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)。  
  
## <a name="examples"></a>示例  
 将文件从文件服务器迁移到 FileTable 时，可以使用 **GetPathLocator** 函数。 在这种情况下，您需要将文件移入 FileTable，然后用 FileTable UNC 路径替换每个文件的原始 UNC 路径。 有关完整示例，请参阅将 [文件加载到 filetable](../../relational-databases/blob/load-files-into-filetables.md)。  
  
## <a name="see-also"></a>另请参阅  
 [在 FileTable 中使用目录和路径](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
