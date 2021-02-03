---
description: ALTER ROUTE (Transact-SQL)
title: ALTER ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ROUTE_TSQL
- ALTER ROUTE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ROUTE statement
- dropping routes
- modifying routes
- removing routes
- routes [Service Broker], modifying
ms.assetid: 8dfb7b16-3dac-4e1e-8c97-adf2aad07830
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: d1fc273bdb22d58974ec5fdc562cf3502348b196
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204041"
---
# <a name="alter-route-transact-sql"></a>ALTER ROUTE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中现有路由的路由信息。 

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER ROUTE route_name  
WITH    
  [ SERVICE_NAME = 'service_name' [ , ] ]  
  [ BROKER_INSTANCE = 'broker_instance' [ , ] ]  
  [ LIFETIME = route_lifetime [ , ] ]  
  [ ADDRESS =  'next_hop_address' [ , ] ]  
  [ MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 route_name   
 要更改的路由的名称。 不能指定服务器、数据库和架构名称。  
  
 WITH  
 引入对要更改的路由进行定义的子句。  
  
 SERVICE_NAME **='** _service\_name_ **'**  
 指定此路由指向的远程服务的名称。 service_name 必须与远程服务使用的名称完全匹配  。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会逐字节进行比较以便与 service_name 匹配  。 也就是说，这种比较区分大小写，并且不考虑当前的排序规则。 服务名称为 'SQL/ServiceBroker/BrokerConfiguration' 的路由是指向 Broker Configuration Notice 服务的路由  。 指向此服务的路由不能指定 Broker 实例。  
  
 如果省略 SERVICE_NAME 子句，则路由的服务名称保持不变。  
  
 BROKER_INSTANCE ='broker **instance'** _\__   
 指定承载目标服务的数据库。  broker_instance 参数必须是远程数据库的 Broker 实例标识符，该标识符可以通过在所选数据库中运行以下查询获得：  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 如果省略 BROKER_INSTANCE 子句，则路由的 Broker 实例保持不变。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 LIFETIME **=** route _lifetime\__  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在路由表中保留路由的时间（秒）。 在生存期结束后，相应的路由即过期，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在为新会话选择路由时将不再考虑该路由。 如果省略此子句，则路由的生存期保持不变。  
  
 ADDRESS ='next **hop** address' _\_\__  

 对于 Azure SQL 托管实例，`ADDRESS` 必须是本地的。

 指定此路由的网络地址。 next_hop_address 按以下格式指定 TCP/IP 地址  ：  
  
 **TCP://** { *dns_name* | *netbios_name* |*ip_address* } **:** *port_number*  
  
 指定的 port_number 必须与指定计算机上 *实例的* 端点的端口号匹配[!INCLUDE[ssSB](../../includes/sssb-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这可以通过在选定数据库中运行如下查询获得：  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 当路由为 next_hop_address 指定 'LOCAL' 时，消息将传递给当前  **实例中的服务**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
 当路由为 next_hop_address 指定 'TRANSPORT' 时，根据服务名称中的网络地址确定网络地址   。 指定了  'TRANSPORT' 的路由可以指定服务名称或 Broker 实例。  
  
 如果  next_hop_address 是数据库镜像的主体服务器，则必须同时指定镜像服务器的 MIRROR_ADDRESS。 否则，此路由将不能自动向镜像服务器进行故障转移。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 MIRROR_ADDRESS **='** next _hop\_mirror\_address\__ **'**  
 指定镜像对的镜像服务器的网络地址，镜像对的主体服务器位于 next_hop_address  。 next_hop_mirror_address 按以下格式指定 TCP/IP 地址  ：  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** *port_number*  
  
 指定的 port_number 必须与指定计算机上 *实例的* 端点的端口号匹配[!INCLUDE[ssSB](../../includes/sssb-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这可以通过在选定数据库中运行如下查询获得：  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 如果指定了 MIRROR_ADDRESS，则路由必须指定 SERVICE_NAME 子句和 BROKER_INSTANCE 子句。 为 next_hop_address 指定 'LOCAL' 或 'TRANSPORT' 的路由不能指定镜像地址    。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
## <a name="remarks"></a>备注  
 存储路由的路由表是元数据表，此表可通过 sys.routes  目录视图读取。 路由表只能通过 CREATE ROUTE、ALTER ROUTE 和 DROP ROUTE 语句进行更新。  
  
 未在 ALTER ROUTE 命令中指定的子句保持不变。 因此，无法通过对路由执行 ALTER 语句来指定此路由不超时、此路由与任何服务名称匹配或此路由与任何 Broker 实例匹配。 若要更改路由的这些特征，必须删除现有路由，然后使用新的信息创建一个新路由。  
  
 当路由为 next_hop_address 指定 'TRANSPORT' 时，根据服务名称确定网络地址   。 对于以网络地址开头的服务名称，只要网络地址的格式对 next_hop_address 有效，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 就可以成功地处理  。 名称包含有效网络地址的服务将路由到服务名称中的网络地址。  
  
 路由表可以包含任意数目的指定相同服务、网络地址和/或 Broker 实例标识符的路由。 在此情况下，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 会使用专门的过程，在会话中指定的信息与路由表中的信息之间找出最匹配的项，以此来选择路由。  
  
 若要更改服务的 AUTHORIZATION，请使用 ALTER AUTHORIZATION 语句。  
  
## <a name="permissions"></a>权限  
 默认情况下，路由的所有者、db_ddladmin  或 db_owner  固定数据库角色和 sysadmin  固定服务器角色的成员拥有更改路由的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-service-for-a-route"></a>A. 更改路由的服务  
 以下示例将 `ExpenseRoute` 路由修改为指向远程服务 `//Adventure-Works.com/Expenses`。  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     SERVICE_NAME = '//Adventure-Works.com/Expenses';  
```  
  
### <a name="b-changing-the-target-database-for-a-route"></a>B. 更改路由的目标数据库  
 以下示例将 `ExpenseRoute` 路由的目标数据库更改为由唯一标识符 `D8D4D268-00A3-4C62-8F91-634B89B1E317.` 标识的数据库  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317';  
```  
  
### <a name="c-changing-the-address-for-a-route"></a>C. 更改路由的地址  
 以下示例将 `ExpenseRoute` 路由的网络地址更改为 IP 地址为 `1234` 的主机上的 TCP 端口 `10.2.19.72`。  
  
```sql  
ALTER ROUTE ExpenseRoute   
   WITH   
     ADDRESS = 'TCP://10.2.19.72:1234';  
```  
  
### <a name="d-changing-the-database-and-address-for-a-route"></a>D. 更改路由的数据库和地址  
 下例将 `ExpenseRoute` 路由的网络地址更改为 DNS 名称为 `1234` 的主机上的 TCP 端口 `www.Adventure-Works.com`。 它还将目标数据库更改为由唯一标识符 `D8D4D268-00A3-4C62-8F91-634B89B1E317` 标识的数据库。  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317',  
     ADDRESS = 'TCP://www.Adventure-Works.com:1234';  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ROUTE (Transact SQL)](../../t-sql/statements/create-route-transact-sql.md)   
 [DROP ROUTE (Transact-SQL)](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
