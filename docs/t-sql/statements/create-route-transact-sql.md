---
description: CREATE ROUTE (Transact-SQL)
title: CREATE ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_ROUTE_TSQL
- ROUTE
- CREATE ROUTE
- ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- lifetimes [Service Broker]
- routes [Service Broker], creating
- forwarding brokers [SQL Server]
- service names [Service Broker]
- database mirroring [SQL Server], routing
- expired routes [SQL Server]
- activating routes
- CREATE ROUTE statement
ms.assetid: 7e695364-1a98-4cfd-8ebd-137ac5a425b3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b3419eb9e2a557309b3892055066ea22749f7b8e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471928"
---
# <a name="create-route-transact-sql"></a>CREATE ROUTE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  在当前数据库的路由表中添加一个新路由。 对于外发消息，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 通过检查本地数据库中的路由表来确定路由。 对于在另一个实例中发起的会话的消息（包括要转发的消息），[!INCLUDE[ssSB](../../includes/sssb-md.md)] 将在 msdb 中检查路由  。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE ROUTE route_name  
[ AUTHORIZATION owner_name ]  
WITH    
   [ SERVICE_NAME = 'service_name', ]  
   [ BROKER_INSTANCE = 'broker_instance_identifier' , ]  
   [ LIFETIME = route_lifetime , ]  
   ADDRESS =  'next_hop_address'  
   [ , MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>参数  
 route_name   
 要创建的路由的名称。 新路由在当前数据库中创建，并归 AUTHORIZATION 子句中指定的主体数据库所有。 不能指定服务器、数据库和架构名称。 route_name 必须是有效的 sysname   。  
  
 AUTHORIZATION owner_name   
 将路由所有者设置为指定的数据库用户或角色。 如果当前用户为 db_owner 固定数据库角色或 sysadmin 固定服务器角色的成员，则 owner_name 可以为任意有效用户或角色的名称    。 否则，owner_name 必须是当前用户的名称，或者是当前用户对其有 IMPERSONATE 权限的用户的名称，或者是当前用户所属的角色的名称  。 如果省略此子句，则路由属于当前用户。  
  
 WITH  
 介绍用于定义要创建的路由的子句。  
  
 SERVICE_NAME = **'** service _name\__ **'**  
 指定此路由指向的远程服务的名称。 service_name 必须与远程服务使用的名称完全匹配  。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会逐字节进行比较以便与 service_name 匹配  。 也就是说，这种比较区分大小写，并且不考虑当前的排序规则。 如果省略 SERVICE_NAME，则此路由将与任何服务名称匹配，但比指定 SERVICE_NAME 的路由的匹配优先级低。 服务名称为 'SQL/ServiceBroker/BrokerConfiguration' 的路由是指向 Broker Configuration Notice 服务的路由  。 指向此服务的路由不能指定 Broker 实例。  
  
 BROKER_INSTANCE = **'** broker _instance\_identifier\__ **'**  
 指定承载目标服务的数据库。 broker_instance_identifier 参数必须是远程数据库的 Broker 实例标识符，该标识符可以通过在所选数据库中运行以下查询获得  ：  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID()  
```  
  
 如果省略 BROKER_INSTANCE 子句，则此路由将与任何 Broker 实例匹配。 如果会话没有指定 Broker 实例，则匹配任何 Broker 实例的路由的匹配优先级高于具有显式 Broker 实例的路由的匹配优先级。 对于指定了 Broker 实例的会话，具有 Broker 实例的路由的优先级高于匹配任何 Broker 实例的路由的优先级。  
  
 LIFETIME **=** route _lifetime\__  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在路由表中保留路由的时间（秒）。 在生存期结束后，相应的路由即过期，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在为新会话选择路由时将不再考虑该路由。 如果省略此子句，则 route_lifetime 为 NULL，并且路由始终不过期  。  
  
 ADDRESS **='** next _hop\_address\__ **'**  
对于 SQL 托管实例，`ADDRESS` 必须是本地的。 

指定此路由的网络地址。 next_hop_address 按以下格式指定 TCP/IP 地址  ：  
  
 TCP://{ dns_name **netbios_name** ip_address } :port *number* |    |    _\__  
  
 指定的 port_number 必须与指定计算机上 *实例的* 端点的端口号匹配[!INCLUDE[ssSB](../../includes/sssb-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这可以通过在选定数据库中运行如下查询获得：  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 如果服务驻留在镜像数据库中，那么，还必须为驻留镜像数据库的其他实例指定 MIRROR_ADDRESS。 否则，此路由不会故障转移到此镜像。  
  
 当路由为 next_hop_address 指定 'LOCAL' 时，消息将传递给当前  **实例中的服务**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
 当路由为 next_hop_address 指定 'TRANSPORT' 时，根据服务名称中的网络地址确定网络地址   。 指定了 'TRANSPORT' 的路由不能指定服务名称或 Broker 实例  。  
  
 MIRROR_ADDRESS **='** next _hop\_mirror\_address\__ **'**  
 指定有一个镜像数据库驻留在 next_hop_address 的镜像数据库的网络地址  。 next_hop_mirror_address 按以下格式指定 TCP/IP 地址  ：  
  
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
  
## <a name="remarks"></a>备注  
 存储路由的路由表是一个元数据表，可通过 sys.routes 目录视图读取  。 此目录视图只能通过 CREATE ROUTE、ALTER ROUTE 和 DROP ROUTE 语句进行更新。  
  
 默认情况下，每个用户数据库中的路由表包含一个路由。 此路由的名称为 AutoCreatedLocal  。 此路由为 next_hop_address 指定 'LOCAL'，并匹配任何服务名称和 Broker 实例标识符   。  
  
 当路由为 next_hop_address 指定 'TRANSPORT' 时，根据服务名称确定网络地址   。 对于以网络地址开头的服务名称，只要网络地址的格式对 next_hop_address 有效，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 就可以成功地处理  。  
  
 路由表可以包含任意数目的指定相同服务、网络地址和 Broker 实例标识符的路由。 在此情况下，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 会使用专门的过程，在会话中指定的信息与路由表中的信息之间找出最匹配的项，以此来选择路由。  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 不会从路由表中删除过期路由。 可以使用 ALTER ROUTE 语句激活过期路由。  
  
 路由不能是临时对象。 允许路由名称以  **开头，但仅限于永久对象#** 。  
  
## <a name="permissions"></a>权限  
 默认情况下，db_ddladmin 或 db_owner 固定数据库角色和 sysadmin 固定服务器角色的成员拥有创建路由的权限    。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-tcpip-route-by-using-a-dns-name"></a>A. 使用 DNS 名称创建 TCP/IP 路由  
 下面的示例创建一个指向服务 `//Adventure-Works.com/Expenses` 的路由。 此路由指定到此服务的消息需要通过 TCP 传输到由 DNS 名称 `1234` 标识的主机上的端口 `www.Adventure-Works.com`。 目标服务器将到达的消息传递到由唯一标识符 `D8D4D268-00A3-4C62-8F91-634B89C1E315` 标识的 Broker 实例。  
  
```sql 
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://www.Adventure-Works.com:1234' ;  
```  
  
### <a name="b-creating-a-tcpip-route-by-using-a-netbios-name"></a>B. 使用 NetBIOS 名称创建 TCP/IP 路由  
 下面的示例创建一个指向服务 `//Adventure-Works.com/Expenses` 的路由。 此路由指定到此服务的消息需要通过 TCP 传输到由 NetBIOS 名称 `1234` 标识的主机上的端口 `SERVER02`。 目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将到达的消息传递到由唯一标识符 `D8D4D268-00A3-4C62-8F91-634B89C1E315` 标识的数据库实例。  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH   
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://SERVER02:1234' ;  
```  
  
### <a name="c-creating-a-tcpip-route-by-using-an-ip-address"></a>C. 使用 IP 地址创建 TCP/IP 路由  
 下面的示例创建一个指向服务 `//Adventure-Works.com/Expenses` 的路由。 此路由指定到此服务的消息通过 TCP 传输到 IP 地址为 `1234` 的主机上的端口 `192.168.10.2`。 目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将到达的消息传递到由唯一标识符 `D8D4D268-00A3-4C62-8F91-634B89C1E315` 标识的 Broker 实例。  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://192.168.10.2:1234' ;  
```  
  
### <a name="d-creating-a-route-to-a-forwarding-broker"></a>D. 创建到转发中介器的路由  
 下面的示例创建到服务器 `dispatch.Adventure-Works.com` 中的转发中介器的路由。 由于服务名称和 Broker 实例标识符都没有指定，因此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将该路由用于没有未其他路由的服务。  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    ADDRESS = 'TCP://dispatch.Adventure-Works.com' ;   
```  
  
### <a name="e-creating-a-route-to-a-local-service"></a>E. 创建到本地服务的路由  
 下面的示例创建到路由所在实例中的服务 `//Adventure-Works.com/LogRequests` 的路由。  
  
```sql  
CREATE ROUTE LogRequests  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/LogRequests',  
    ADDRESS = 'LOCAL' ;  
```  
  
### <a name="f-creating-a-route-with-a-specified-lifetime"></a>F. 创建具有指定生存期的路由  
 下面的示例创建一个指向服务 `//Adventure-Works.com/Expenses` 的路由。 路由的生存期是 `259200` 秒，即 72 小时。  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    LIFETIME = 259200,  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234' ;  
```  
  
### <a name="g-creating-a-route-to-a-mirrored-database"></a>G. 创建到镜像数据库的路由  
 下面的示例创建一个指向服务 `//Adventure-Works.com/Expenses` 的路由。 该服务驻留在被镜像的数据库中。 其中一个镜像数据库位于地址 `services.Adventure-Works.com:1234`，另一个数据库位于地址 `services-mirror.Adventure-Works.com:1234`。  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = '69fcc80c-2239-4700-8437-1001ecddf933',  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234',   
    MIRROR_ADDRESS = 'TCP://services-mirror.Adventure-Works.com:1234' ;  
```  
  
### <a name="h-creating-a-route-that-uses-the-service-name-for-routing"></a>H. 创建使用服务名称进行路由的路由  
 下面的示例创建一个路由，此路由使用服务名称来确定要向其发送消息的网络地址。 请注意，指定 `'TRANSPORT'` 作为网络地址的路由比其他路由的匹配优先级低。  
  
```sql  
CREATE ROUTE TransportRoute  
    WITH ADDRESS = 'TRANSPORT' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ROUTE (Transact-SQL)](../../t-sql/statements/alter-route-transact-sql.md)   
 [DROP ROUTE (Transact-SQL)](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
