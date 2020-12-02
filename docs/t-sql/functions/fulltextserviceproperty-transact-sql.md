---
description: FULLTEXTSERVICEPROPERTY (Transact-SQL)
title: FULLTEXTSERVICEPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXTSERVICEPROPERTY_TSQL
- FULLTEXTSERVICEPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], properties
- FULLTEXTSERVICEPROPERTY function
- services [SQL Server], full-text search properties
- test
ms.assetid: b7dcacb0-af83-4807-9d1e-49148b56b59c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b47a991aa2500236aec9d80d2ae4c51144619f90
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91116040"
---
# <a name="fulltextserviceproperty-transact-sql"></a>FULLTEXTSERVICEPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回与全文引擎属性有关的信息。 可以使用 sp_fulltext_service 设置和检索这些属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FULLTEXTSERVICEPROPERTY ('property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *property*  
 包含全文服务级别属性名称的表达式。 下表列出了这些属性，并提供对返回的信息的说明。  
  
> [!NOTE]
>  以后的 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中将删除下列属性：ConnectTimeout、DataTimeout 和 ResourceUsage。 应避免在新的开发工作中使用这些属性，并着手修改当前使用上述任意属性的应用程序。  
  
|属性|值|  
|--------------|-----------|  
|**ResourceUsage**|返回 0。 支持它仅仅是为了保持向后兼容。|  
|**ConnectTimeout**|返回 0。 支持它仅仅是为了保持向后兼容。|  
|**IsFulltextInstalled**|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例中安装全文组件。<br /><br /> 0 = 未安装全文组件。<br /><br /> 1 = 已安装全文组件。<br /><br /> NULL = 输入无效或发生错误。|  
|**DataTimeout**|返回 0。 支持它仅仅是为了保持向后兼容。|  
|**LoadOSResources**|指示此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中是否注册并使用了操作系统断字符和筛选器。 默认情况下，禁用此属性，以防止更新程序因疏忽而对操作系统 (OS) 的行为进行更改。 如果允许使用 OS 资源，则可以访问在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 索引服务中注册的语言和文档类型的资源，但不能安装特定于实例的资源。 如果允许加载 OS 资源，请确保 OS 资源是受信任的已签名二进制文件；否则，当 VerifySignature 设置为 1 时，将无法加载它们。<br /><br /> 0 = 仅使用特定于此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的筛选器和断字符。<br /><br /> 1 = 加载 OS 筛选器和断字符。|  
|**VerifySignature**|指定 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search 服务是否仅加载已签名的二进制文件。 默认情况下，仅加载已签名的可信二进制文件。<br /><br /> 0 = 不检查二进制文件是否已签名。<br /><br /> 1 = 验证是否仅加载了已签名的可信二进制文件。|  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="examples"></a>示例  
 以下示例检查是否仅加载已签名的二进制文件，并且返回值指示未在进行此验证。  
  
```sql  
SELECT fulltextserviceproperty('VerifySignature');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-----------   
0  
```  
  
 请注意，若要将签名验证重新设置为其默认值 1，您可以使用以下 `sp_fulltext_service` 语句：  
  
```sql  
EXEC sp_fulltext_service @action='verify_signature', @value=1;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [FULLTEXTCATALOGPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_fulltext_service (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
  
  
