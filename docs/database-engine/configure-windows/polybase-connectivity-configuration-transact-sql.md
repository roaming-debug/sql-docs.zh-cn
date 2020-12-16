---
title: PolyBase 连接配置 (Transact-SQL) | Microsoft Docs
description: 了解如何使用 sp_configure 显示或更改 PolyBase Hadoop 和 Azure Blob 存储连接的全局配置设置。
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: polybase
ms.topic: reference
helpviewer_keywords:
- PolyBase
ms.assetid: 82252e4f-b1d0-49e5-aa0b-3624aade2add
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 71c71e4809b573dae9507b52bc3d32e5b6f5142e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460740"
---
# <a name="polybase-connectivity-configuration-transact-sql"></a>PolyBase 连接配置 (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]

  显示或更改 PolyBase Hadoop 和 Azure blob 存储连接的全局配置设置。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
--List all of the configuration options  
sp_configure  
[;]  
  
--Configure Hadoop connectivity  
sp_configure [ @configname = ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>参数  
 [ **@configname=** ] **'** _option\_name_ **'**  
 配置选项的名称。 *option_name* 的数据类型为 **varchar(35)** ，默认值为 NULL。 如果未指定该参数，则返回选项的完整列表。  
  
 [ **@configvalue=** ] **'** _value_ **'**  
 新的配置设置。 *value* 的数据类型为 **int**，默认值为 NULL。 最大值取决于各个选项。  
  
 **“hadoop 连接”**  
 为从 PolyBase 到 Hadoop 群集或 Azure blob 存储 (WASB) 的所有连接指定 Hadoop 数据源类型。 若要为外部表创建外部数据源，需要使用此设置。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)，  
  
 这些是 Hadoop 连接设置和其相应支持的 Hadoop 数据源。 一次只有一种设置有效。 选项 1、4 和 7 允许创建多种类型的外部数据源，并在服务器上的所有会话中进行使用。  
  
-   选项 0：禁用 Hadoop 连接  
  
-   选项 1：Windows Server 上的 Hortonworks HDP 1.3  
  
-   选项 1：Azure blob 存储 (WASB[S])  
  
-   选项 2：Linux 上的 Hortonworks HDP 1.3  
  
-   选项 3：Linux 上的 Cloudera CDH 4.3  
  
-   选项 4：Windows Server 上的 Hortonworks HDP 2.0  
  
-   选项 4：Azure blob 存储 (WASB[S])  
  
-   选项 5：Linux 上的 Hortonworks HDP 2.0  
  
-   选项 6：Linux 上的 Cloudera 5.1、5.2、5.3、5.4、5.5、5.9、5.10、5.11、5.12 和 5.13  
  
-   选项 7：Linux 上的 Hortonworks 2.1、2.2、2.3、2.4、2.5、2.6、3.0  
  
-   选项 7：Windows Server 上的 Hortonworks 2.1、2.2 和 2.3  
  
-   选项 7：Azure blob 存储 (WASB[S])  
  
 **RECONFIGURE**  
 更新运行值 (run_value) 以匹配配置值 (config_value)。 请参阅 [结果集](#ResultSets) 以了解 run_value 和 config_value 的定义。 在 RECONFIGURE 语句设置运行值之前，由 sp_configure 设置的新配置值不会生效。  
  
 运行 RECONFIGURE 之后，必须停止并重启 SQL Server 服务。 请注意，停止 SQL Server 服务时，另外两个 PolyBase 引擎和数据移动服务将自动停止。 重启 SQL Server 引擎服务之后，再次重启这两项服务（它们不会自动启动）。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
##  <a name="result-sets"></a><a name="ResultSets"></a> 结果集  
 在不使用参数执行时， **sp_configure** 会返回五列的结果集。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(35)**|配置选项的名称。|  
|**最小值**|**int**|配置选项的最小值。|  
|**最大值**|**int**|配置选项的最大值。|  
|**config_value**|**int**|使用 **sp_configure** 设置的值。|  
|**run_value**|**int**|PolyBase 正在使用当前值。 此值通过运行 RECONFIGURE 进行设置。<br /><br /> **config_value** 和 **run_value** 通常是相同的，除非该值正在进行更改。<br /><br /> 如果正在进行重新配置，则可能需要重启计算机，运行值才会准确。|  
  
## <a name="general-remarks"></a>一般备注  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，运行 RECONFIGURE 之后，为使“hadoop 连接”的运行值生效，需要重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
在 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]中，运行 RECONFIGURE 之后，为使“hadoop 连接”的运行值生效，需要重启 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 区域。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不允许在显式或隐式事务中使用 RECONFIGURE。  
  
## <a name="permissions"></a>权限  
 所有的用户都可以不使用参数或者使用 **参数执行** sp_configure @configname 。  
  
 需要 **ALTER SETTINGS** 服务器级别权限或 **sysadmin** 中固定服务器角色的成员资格，才能更改配置值或运行 RECONFIGURE。  
  
## <a name="examples"></a>示例  
  
### <a name="a-list-all-available-configuration-settings"></a>A. 列出所有可用的配置设置  
 以下示例显示如何列出所有的配置选项。  
  
```  
EXEC sp_configure;  
```  
  
 结果返回选项名称，后跟该选项的最小值和最大值。 **config_value** 是重新配置完成后 SQL 或者 PolyBase 将使用的值。 **run_value** 是当前正在使用的值。 **config_value** 和 **run_value** 通常是相同的，除非该值正在进行更改。  
  
### <a name="b-list-the-configuration-settings-for-one-configuration-name"></a>B. 列出一个配置名称的配置设置  
  
```  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="c-set-hadoop-connectivity"></a>C. 设置 hadoop 连接  
 此示例将 PolyBase 设置为选项 7。 此选项允许 PolyBase 在 Linux 和 Windows Server 以及 Azure blob 存储中，创建和使用 Hortonworks 2.1、2.2 和 2.3 上的外部表。 例如，SQL 可能拥有 30 个外部表，其中 7 个引用 Linux 上 Hortonworks 2.1 的数据，4 个引用 Linux 上 Hortonworks 2.2 的数据，7 个引用 Linux 上 Hortonworks 2.3 的数据，其余 12 个引用 Azure blog 存储。  
  
```  
--Configure external tables to reference data on Hortonworks 2.1, 2.2, and 2.3 on Linux, and Azure blob storage  
  
sp_configure @configname = 'hadoop connectivity', @configvalue = 7;  
GO  
  
RECONFIGURE  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
