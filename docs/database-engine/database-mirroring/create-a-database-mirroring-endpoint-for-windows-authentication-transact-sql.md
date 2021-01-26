---
title: 创建数据库镜像终结点 (Transact-SQL)
description: 使用 Transact-SQL 在 SQL Server 中通过 Windows 身份验证创建数据库镜像终结点。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: baf1a4b1-6790-4275-b261-490bca33bdb9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 86646e3af14403f6d48c8af009525c6c016ef9e3
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98782972"
---
# <a name="create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql"></a>创建使用 Windows 身份验证的数据库镜像端点 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何创建一个数据库镜像端点，该端点通过 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中使用 Windows 身份验证。 为了支持数据库镜像或 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个实例都需要一个数据库镜像端点。 一个服务器实例只能有一个数据库镜像端点，该端点有一个端口。 在创建端点时，数据库镜像端点可以使用本地系统上任何可用的端口。 服务器实例上的所有数据库镜像会话都侦听该端口，并且数据库镜像的所有传入连接都使用该端口。  
  
> [!IMPORTANT]  
>  如果数据库镜像端点已存在并处于使用状态，则我们建议您使用此端点。 删除正在使用的端点会中断现有会话。  
  
 **本主题内容**  
  
-   **开始之前：** [安全性](#Security)  
  
-   若要创建数据库镜像端点，请使用：[Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
 服务器实例的身份验证和加密方法由系统管理员建立。  
  
> [!WARNING]  
>  不推荐使用 RC4 算法。 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 我们建议使用 AES。  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求具有 `CREATE ENDPOINT` 权限，或者具有 `sysadmin` 固定服务器角色的成员身份。 有关详细信息，请参阅 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-database-mirroring-endpoint-that-uses-windows-authentication"></a>创建使用 Windows 身份验证的数据库镜像端点  
  
1.  连接至要为其创建数据库镜像端点的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  使用以下语句确定数据库镜像端点是否已经存在：  
  
    ```sql  
    SELECT name, role_desc, state_desc FROM sys.database_mirroring_endpoints;   
    ```  
  
    > [!IMPORTANT]  
    >  如果服务器实例已有数据库镜像端点，则可以将该端点用于在服务器实例上建立的任何其他会话。  
  
4.  要通过 Transact-SQL 创建使用 Windows 身份验证的终结点，请使用 `CREATE ENDPOINT` 语句。 该语句采用以下常规形式：  
  
     ```syntaxsql
     CREATE ENDPOINT *\<endpointName>*  
  
     STATE=STARTED  
  
     AS TCP ( LISTENER_PORT = *\<listenerPortList>* )  
  
     FOR DATABASE_MIRRORING  
  
     (  
  
     [ AUTHENTICATION = **WINDOWS** [ *\<authorizationMethod>* ]  
  
     ]  
  
     [ [**,**] ENCRYPTION = **REQUIRED**  
  
     [ ALGORITHM { *\<algorithm>* } ]  
  
     ]  
  
     [**,**] ROLE = *\<role>*  
  
     )  
     ```
     
     其中：  
  
    -   \<endpointName> 是服务器实例的数据库镜像终结点的唯一名称。  
  
    -   STARTED 指定端点启动并开始侦听连接。 数据库镜像端点创建后通常处于 STARTED 状态。 另外，可以启动处于 STOPPED 状态（默认值）或 DISABLED 状态中的会话。  
  
    -   \<listenerPortList> 是需要服务器在其上侦听数据库镜像消息的单一端口号 (nnnn) 。 只允许使用 TCP，指定任何其他协议都会导致发生错误。  
  
         每个计算机系统一次只能使用一个端口号。 在创建端点时，数据库镜像端点可以使用本地系统上任何可用的端口。 若要识别出系统上 TCP 端点当前使用的端口，请使用以下 Transact-SQL 语句：  
  
        ```sql  
        SELECT name, port FROM sys.tcp_endpoints;  
        ```  
  
        > [!IMPORTANT]  
        >  每个服务器实例需要且只需要一个唯一的侦听器端口。  
  
    -   对于 Windows 身份验证，AUTHENTICATION 选项是可选的，除非您只想让端点使用 NTLM 或 Kerberos 对连接进行身份验证。 \<authorizationMethod> 指定用于对连接进行身份验证的以下某种方法：NTLM、KERBEROS 或 NEGOTIATE。 默认方法 NEGOTIATE 会导致端点使用 Windows 协商协议来选择 NTLM 或 Kerberos。 根据另一侧端点的身份验证级别的不同，协商协议启用具有或没有身份验证的连接。  
  
    -   默认情况下，ENCRYPTION 设置为 REQUIRED。 这意味着此端点的所有连接都必须加密。 但您可以禁用加密，或对于端点可以选择禁用加密。 可以选择的选项如下：  
  
        |值|定义|  
        |-----------|----------------|  
        |DISABLED|指定不对通过连接发送的数据加密。|  
        |SUPPORTED|指定只有在对方端点指定了 SUPPORTED 或者 REQUIRED 时，才加密数据。|  
        |REQUIRED|指定必须对通过连接发送的数据加密。|  
  
         如果端点要求加密，则对于其他端点，必须将 ENCRYPTION 设置为 SUPPORTED 或 REQUIRED。  
  
    -   \<algorithm> 提供为终结点指定加密标准的选项。 \<algorithm> 的值可以是以下算法或这些算法的组合之一：RC4、AES、AES RC4 或 RC4 AES。  
  
         AES RC4 指定此端点协商加密算法，但优先使用 AES 算法。 RC4 AES 指定此端点协商加密算法，但优先使用 RC4 算法。 如果两个端点都指定了这两种算法，但顺序不同，则接受连接的端点入选。 显式地提供相同的算法以避免不同服务器之间的连接错误。
  
        > [!WARNING]  
        >  不推荐使用 RC4 算法。 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 我们建议使用 AES。  
  
    -   \<role> 定义服务器可以执行的一个或多个角色。 必须指定 ROLE。 不过，该端点的角色仅针对数据库镜像。 对于 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]，该端点的角色将被忽略。  
  
         若要让服务器实例对于一个数据库镜像会话执行一个角色，对于另一个会话执行另一个角色，请指定 ROLE = ALL。 要让服务器实例限于伙伴角色或见证角色，请分别指定 `ROLE = PARTNER` 或 `ROLE = WITNESS`。  
  
        > [!NOTE]  
        >  有关不同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的数据库镜像选项详细信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
     有关 CREATE ENDPOINT 语法的完整说明，请参阅 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)中使用 Windows 身份验证。  
  
    > [!NOTE]  
    >  若要更改现有的端点，请使用 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)中使用 Windows 身份验证。  
  
###  <a name="example-creating-endpoints-to-support-for-database-mirroring-transact-sql"></a><a name="TsqlExample"></a> 示例：创建终结点以支持数据库镜像 (Transact-SQL)  
 下面的示例为三个不同的计算机系统上的默认服务器实例创建数据库镜像端点：  
  
|服务器实例的角色|主机的名称|  
|-----------------------------|---------------------------|  
|伙伴（最初在主体角色中）|`SQLHOST01\.`|  
|伙伴（最初在镜像角色中）|`SQLHOST02\.`|  
|见证|`SQLHOST03\.`|  
  
 在本示例中，尽管任何可用的端口号都可以，但所有三个端点都使用端口号 7022。 AUTHENTICATION 选项是不必要的，因为端点使用默认类型，即 Windows 身份验证。 ENCRYPTION 选项也是不必要的，因为端点都打算通过协商来决定连接的身份验证方法，这是 Windows 身份验证的默认行为。 而且，所有端点都需要加密，这是默认行为。  
  
 每个服务器实例都只能作为伙伴或见证服务器，而每个服务器端点都明确指定了角色（ROLE=PARTNER 或 ROLE=WITNESS）。  
  
> [!IMPORTANT]  
>  每个服务器实例只能有一个端点。 因此，如果您希望某个服务器实例在一些会话中作为伙伴，在另一些会话中作为见证服务器，请指定 ROLE=ALL。  
  
```sql  
--Endpoint for initial principal server instance, which  
--is the only server instance running on SQLHOST01.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for initial mirror server instance, which  
--is the only server instance running on SQLHOST02.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for witness server instance, which  
--is the only server instance running on SQLHOST03.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=WITNESS);  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **配置数据库镜像端点**  
  
-   [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [使用数据库镜像终结点证书 (Transact-SQL)](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [允许数据库镜像终结点使用证书进行出站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [允许数据库镜像终结点将证书用于入站连接 (Transact-SQL)](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **查看有关数据库镜像端点的信息**  
  
-   [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [选择加密算法](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [示例：使用 Windows 身份验证设置数据库镜像 (Transact-SQL)](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  

