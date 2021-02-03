---
description: CREATE ENDPOINT (Transact-SQL)
title: CREATE ENDPOINT (Transact-SQL)
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENDPOINT
- CREATE ENDPOINT
- ENDPOINT_TSQL
- CREATE_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HTTP SOAP support [SQL Server]
- CREATE ENDPOINT statement
- Availability Groups [SQL Server], configuring
- endpoints [SQL Server], creating
- SOAP [SQL Server built-in support], endpoints
- SOAP [SQL Server built-in support], sqlbatch
- DATABASE_MIRRORING option
- HTTP protocol option [SQL Server]
- SOAP [SQL Server built-in support], ad hoc
- TCP protocol option [SQL Server]
- SERVICE_BROKER option
- Availability Groups [SQL Server], endpoint
ms.assetid: 6405e7ec-0b5b-4afd-9792-1bfa5a2491f6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6f92bac093c72ccf93f4db8f8251fe3b9b8027d1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234280"
---
# <a name="create-endpoint-transact-sql"></a>CREATE ENDPOINT (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建端点并定义其属性，包括可用于客户端应用程序的方法。 有关相关权限的信息，请参阅 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)。  
  
 CREATE ENDPOINT 的语法在逻辑上分为两部分：  
  
-   第一部分从 AS 开始，到 FOR 子句之前结束。  
  
     在此部分中，需要提供传输协议 (TCP) 特定的信息，设置端点的侦听端口号，以及设置端点身份验证的方法和/或要限制访问端点的 IP 地址列表（如果有的话）。  
  
-   第二部分从 FOR 子句开始。  
  
     在此部分中，需要定义端点上所支持的负载。 负载可以为以下多种支持类型中的一种：[!INCLUDE[tsql](../../includes/tsql-md.md)]、Service Broker、数据库镜像。 在此部分中，还需要提供语言特定信息。  
  
> **注意：** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中已删除了本机 XML Web 服务（SOAP/HTTP 终结点）。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
AS { TCP } (  
   <protocol_specific_arguments>  
        )  
FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_arguments>  
        )  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( xx.xx.xx.xx IPv4 address ) | ( '__:__1' IPv6 address ) ]  
  
)  
  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ [ , ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
   ]  
   [ [ , ] MESSAGE_FORWARDING = { ENABLED | DISABLED } ]  
   [ [ , ] MESSAGE_FORWARD_SIZE = forward_size ]  
)  
  

<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
            WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
   [ [ [ , ] ] ENCRYPTION = { DISABLED | { { SUPPORTED | REQUIRED }   
       [ ALGORITHM { AES | RC4 | AES RC4 | RC4 AES } ] }   
  
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 endPointName  
 所创建的端点的已分配名称。 在更新或删除端点时使用。  
  
 AUTHORIZATION login  
 指定被分配了新建端点对象的所有权的有效 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Windows 登录帐户。 如果没有指定 AUTHORIZATION，默认情况下调用方将成为新建对象的所有者。  
  
 若要通过指定 AUTHORIZATION 分配所有权，调用方必须对指定的 login 具有 IMPERSONATE 权限。  
  
 若要重新分配所有权，请参阅 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)。  
  
 STATE = { STARTED | STOPPED | DISABLED }   
 端点创建时的状态。 如果在创建端点时未指定状态，则默认值为 STOPPED。  
  
 STARTED  
 端点已启动并在积极地侦听连接。  
  
 DISABLED  
 端点被禁用。 在该状态下，服务器侦听端口请求但向客户端返回错误。  
  
 STOPPED  
 端点被停止。 在该状态下，服务器不侦听端点端口，也不对使用端点的任何尝试请求进行响应。  
  
 若要更改状态，请使用 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)。  
  
 AS { TCP }  
 指定要使用的传输协议。  
  
 FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING }  
 指定负载类型。  
  
 当前，没有要传入 `<language_specific_arguments>` 参数 (parameter) 的特定于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语言的参数 (argument)。  
  
 **TCP 协议选项**  
  
 下列参数仅适用于 TCP 协议选项。  
  
 LISTENER_PORT listenerPort **=**   
 指定 Service Broker TCP/IP 协议在其上侦听连接的端口号。 按照约定，将使用 4022，但 1024 和 32767 之间的任何数字都有效。  
  
 LISTENER_IP **=** ALL | **(** _4-part-ip_ **)**  |  **(** "*ip_address_v6*" **)**  
 指定端点将侦听的 IP 地址。 默认值为 ALL。 这表示侦听器将接受任何有效 IP 地址上的连接。  
  
 如果用 IP 地址而不是完全限定域名（`ALTER DATABASE SET PARTNER = partner_IP_address` 或 `ALTER DATABASE SET WITNESS = witness_IP_address`）配置数据库镜像，则在创建镜像端点时必须指定 `LISTENER_IP =IP_address` 而不是 `LISTENER_IP=ALL`。  
  
 **SERVICE_BROKER 和 DATABASE_MIRRORING 选项**  
  
 下列 AUTHENTICATION 和 ENCRYPTION 参数是 SERVICE_BROKER 和 DATABASE_MIRRORING 选项所共有的。  
  
> [!NOTE]  
>  有关特定于 SERVICE_BROKER 的选项，请参阅本节后面的“SERVICE_BROKER 选项”。 有关特定于 DATABASE_MIRRORING 的选项，请参阅本节后面的“DATABASE_MIRRORING 选项”。  
  
 AUTHENTICATION = \<authentication_options>：为此终结点指定连接需要的 TCP/IP 身份验证。 默认值为 WINDOWS。  
  
 支持的身份验证方法包括 NTLM 或 Kerberos 或这两种方法。  
  
> [!IMPORTANT]  
>  服务器实例上所有的镜像连接都只使用一个数据库镜像端点。 任何创建其他数据库镜像端点的尝试都将失败。  
  
 \<authentication_options> ::=  
  
 WINDOWS [ { NTLM \| KERBEROS \| NEGOTIATE } ]   
 指定端点使用 Windows 身份验证协议进行连接以验证端点。 这是默认值。  
  
 如果指定某一授权方法（NTLM 或 KERBEROS），则始终将该方法用作身份验证协议。 默认值 NEGOTIATE 允许端点使用 Windows 协商协议在 NTLM 和 Kerberos 之间进行选择。  
  
 CERTIFICATE certificate_name  
 指定端点使用 certificate_name 指定的证书验证连接以建立授权标识。 远端点必须具有其公钥与指定证书的私钥相匹配的证书。  
  
 WINDOWS [ { NTLM \| KERBEROS \| NEGOTIATE } ] CERTIFICATE certificate_name  
 指定端点通过使用 Windows 身份验证尝试进行连接；如果该尝试失败，则尝试使用指定的证书。  
  
 CERTIFICATE certificate_name WINDOWS [ { NTLM \| KERBEROS \| NEGOTIATE } ]  
 指定端点通过使用指定的证书尝试进行连接；如果该尝试失败，则尝试使用 Windows 身份验证。  
  
 ENCRYPTION = { DISABLED \| SUPPORTED \| REQUIRED } [ALGORITHM { AES \| RC4 \| AES RC4 \| RC4 AES } ]   
 指定是否在过程中使用加密。 默认值为 REQUIRED。  
  
 DISABLED  
 指定不对通过连接发送的数据加密。  
  
 SUPPORTED  
 指定只有在对方端点指定了 SUPPORTED 或者 REQUIRED 时，才加密数据。  
  
 REQUIRED  
 指定与此端点的连接必须使用加密。 因此，若要连接到此端点，则另一端点必须将 ENCRYPTION 设置为 SUPPORTED 或 REQUIRED。  
  
 也可以使用 ALGORITHM 参数指定端点使用的加密形式，如下所示：  
  
 **AES**  
 指定端点必须使用 AES 算法。 这是 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 或更高版本中的默认值。  
  
 RC4  
 指定端点必须使用 RC4 算法。 这是 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的默认值。  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本中，可以在任何兼容性级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
  
 AES RC4  
 指定两个端点将与优先使用 AES 算法的此端点协商加密算法。  
  
 RC4 AES  
 指定两个端点将与优先使用 RC4 算法的此端点协商加密算法。  
  
> [!NOTE]  
>  不推荐使用 RC4 算法。 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 我们建议使用 AES。  
  
 如果两个端点都指定了这两种算法，但顺序不同，则接受连接的端点入选。  
  
 **SERVICE_BROKER 选项**  
  
 下列参数专用于 SERVICE_BROKER 选项。  
  
 MESSAGE_FORWARDING = { ENABLED | DISABLED }   
 确定是否将转发此端点所收到的位于其他位置的服务的消息。  
  
 ENABLED  
 如果提供了转发地址，则转发消息。  
  
 DISABLED  
 放弃位于其他位置的服务的消息。 这是默认值。  
  
 MESSAGE_FORWARD_SIZE =forward_size  
 指定存储要转发的消息时为要使用的端点分配的最大存储量 (MB)。  
  
 **DATABASE_MIRRORING 选项**  
  
 下列参数专用于 DATABASE_MIRRORING 选项。  
  
 ROLE = { WITNESS | PARTNER | ALL }  
 指定端点支持的数据库镜像角色（一个或多个）。  
  
 WITNESS  
 启用要在镜像过程中以见证角色执行的端点。  
  
> [!NOTE]  
>  在 [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] 中，只提供 WITNESS 选项。  
  
 PARTNER  
 启用要在镜像过程中以伙伴角色执行的端点。  
  
 ALL  
 启用要在镜像过程中以见证角色兼伙伴角色执行的端点。  
  
 有关这些角色的详细信息，请参阅[数据库镜像 &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
> [!NOTE]  
>  没有用于 DATABASE_MIRRORING 的默认端口。  
  
## <a name="remarks"></a>备注  
 ENDPOINT DDL 语句不能在用户事务内部执行。 即使活动的快照隔离级别事务正在使用被更改的端点，ENDPOINT DDL 语句也不会失败。  
  
 下列用户可以对 ENDPOINT 执行请求：  
  
-   sysadmin 固定服务器角色的成员  
  
-   端点的所有者  
  
-   已被授予对端点的 CONNECT 权限的用户或组  
  
## <a name="permissions"></a>权限  
 要求具有 CREATE ENDPOINT 权限，或者具有 **sysadmin** 固定服务器角色的成员身份。 有关详细信息，请参阅 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)。  
  
## <a name="example"></a>示例  
  
### <a name="creating-a-database-mirroring-endpoint"></a>创建数据库镜像终结点  
 下面的示例创建一个数据库镜像端点。 该端点使用端口号 `7022`，也可以使用任何可用的端口号。 该端点配置为使用只使用 Kerberos 的 Windows 身份验证。 `ENCRYPTION` 选项配置为 `SUPPORTED` 的非默认值以支持加密或未加密数据。 该端点目前配置为同时支持伙伴和见证角色。  
  
```sql  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (  
       AUTHENTICATION = WINDOWS KERBEROS,  
       ENCRYPTION = SUPPORTED,  
       ROLE=ALL);  
GO  
```  

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv4-address-and-port"></a>新建指向特定 IPv4 地址和端口的终结点

```sql
CREATE ENDPOINT ipv4_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = (10.0.75.1)
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public; -- Keep existing public permission on default endpoint for demo purpose
GRANT CONNECT ON ENDPOINT::ipv4_endpoint_special
TO login_name;
```

### <a name="create-a-new-endpoint-pointing-to-a-specific-ipv6-address-and-port"></a>新建指向特定 IPv6 地址和端口的终结点

```sql
CREATE ENDPOINT ipv6_endpoint_special
STATE = STARTED
AS TCP (
    LISTENER_PORT = 55555, LISTENER_IP = ('::1')
)
FOR TSQL ();

GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] TO public;
GRANT CONNECT ON ENDPOINT::ipv6_endpoint_special

```
  
## <a name="see-also"></a>另请参阅  
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [DROP ENDPOINT (Transact SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

