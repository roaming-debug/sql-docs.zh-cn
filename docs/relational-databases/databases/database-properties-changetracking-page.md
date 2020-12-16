---
title: 数据库属性（“更改跟踪”页）| Microsoft Docs
description: 了解如何使用“数据库属性”对话框中的“更改跟踪”选项卡来查看或修改数据库的更改跟踪设置。
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.changetracking.f1
ms.assetid: 9b929640-bc62-449b-9b06-b5a77b8cf372
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cfe6b46da2124235f4f2e62e70e13ec486f1b6c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480298"
---
# <a name="database-properties-changetracking-page"></a>数据库属性（“更改跟踪”页）
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  使用此页可查看或修改所选数据库的更改跟踪设置。 有关在此页上可用选项的详细信息，请参阅[启用和禁用更改跟踪 (SQL Server)](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)。  
  
## <a name="options"></a>选项  
 **更改跟踪**  
 用于启用或禁用数据库的更改跟踪。  
  
 若要启用更改跟踪，您必须拥有修改数据库的权限。  
  
 将其值设置为 **True** 即可设置允许对各个表启用更改跟踪的数据库选项。  
  
 还可以使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)来配置更改跟踪。  
  
 **保持期**  
 指定在数据库中保留更改跟踪信息的最短期限。 只有当“自动清除”的值为 True 时，才会删除数据。  
  
 默认值为 2。  
  
 **保持期单位**  
 指定“保持期”值的单位。 可以选择 **“天”** 、 **“小时”** 或 **“分钟”** 。 默认值为 **“天”** 。  
  
 最短保持期为 1 分钟。 不存在最长保持期。  
  
 **自动清除**  
 指示是否在经过指定的保持期后自动删除更改跟踪信息。  
  
 启用“自动清除”会将任何先前自定义的保持期重置为默认保持期（即 2 天）。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)  
  
