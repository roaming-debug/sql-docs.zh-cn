---
title: AD 模式登录失败 - 不受信任的域
titleSuffix: SQL Server Big Data Cluster
description: 修复行为 - 当终结点 DNS 条目被配置为指向别名的 CNAME 时，客户端无法进行身份验证。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 05/01/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 41145555b16dc437a4cb8f47ec490077f8aa8db3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045547"
---
# <a name="symptom-ad-mode-login-fails---untrusted-domain-big-data-clusters"></a>故障描述：AD 模式登录失败 - 不受信任的域（大数据群集）

在 Active Directory 模式下的 SQL Server 大数据群集 (BDC) 上，连接尝试可能失败，并返回以下错误：

`Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.`

如果已将 DNS 条目配置为 CNAME（它指向将流量分发到 Kubernetes 节点的反向代理的别名），则会发生这种情况。

## <a name="root-cause"></a>根本原因

如果将终结点的 DNS 条目配置为 CNAME（它指向将流量分发到 Kubernetes 节点的反向代理的别名）：

- Kerberos 身份验证过程会查找与 CNAME 条目匹配的服务主体名称 (SPN)；而不是 BDC 在 Active Directory 中注册的实际 SPN
- 身份验证会失败 

## <a name="confirm-root-cause"></a>确认根本原因

身份验证失败后，请检查 Kerberos 票证的缓存。 

要检查票证的缓存，请使用 `klist` 命令。 

查找与你尝试连接的终结点匹配的 SPN 的票证。

找不到预期票证。

在下面的示例中，主终结点（`bdc-sql` DNS 记录）是设置为 `ServerReverseProxy` 反向代理的 CNAME

```PowerShell
Resolve-DnsName bdc-sql
```

以下部分显示前一个命令的结果。

```
Name                           Type   TTL   Section    NameHost
----                           ----   ---   -------    --------
bdc-sql.mydomain.com           CNAME  3600  Answer     ReverseProxyServer.mydomain.com

Name       : ReverseProxyServer.mydomain.com
QueryType  : A
TTL        : 3600
Section    : Answer
IP4Address : 193.168.5.10
```

>[!NOTE]
>以下部分引用 [`tshark`](https://www.wireshark.org/docs/man-pages/tshark.html)。 `tshark` 是作为 [Wireshark](https://www.wireshark.org/docs/) 网络跟踪实用工具的一部分安装的命令行实用工具。

若要查看从 Active Directory 请求的 SPN，请使用 `tshark`。 以下命令将网络跟踪捕获限制为 Kerberos 协议通信，并仅显示 `krb-error (30)` 消息。 这些消息应包含失败的 SPN 请求消息。

```bash
tshark -Y "kerberos && kerberos.msg_type == 30" -T fields -e kerberos.error_code -e kerberos.SNameString
```

在其他命令行界面中，尝试连接到主 Pod：

```bash
klist purge

sqlcmd -S bdc-sql.mydomain.com,31433 -E
```

来看看下面的示例输出。

```bash
klist purge

Current LogonId is 0:0xf6b58
        Deleting all tickets:
        Ticket(s) purged!

sqlcmd -S bdc-sql.mydomain.com,31433 -E
sqlcmd: Error: Microsoft ODBC Driver 17 for SQL Server : Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.
```

查看 `tshark` 输出。 

```bash
Capturing on 'Ethernet 3'
25      krbtgt,RLAZURE.COM
7       MSSQLSvc,ReverseProxyServer.mydomain.com:31433
2 packets captured
```

请注意，客户端请求的 `SPN MSSQLSvc,ReverseProxyServer.mydomain.com:31433` 不存在。 连接尝试最终失败，显示错误 7。 错误 7 表示 `KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN Server not found in Kerberos database`。

在正确的配置中，客户端将请求 BDC 注册的 SPN。 本例中正确的 SPN 应该是 `MSSQLSvc,bdc-sql.mydomain.com:31433`。

>[!NOTE]
>错误 25 表示 `KDC_ERR_PREAUTH_REQUIRED` - 需要额外的预身份验证。 可以安全地忽略此错误。 在初始 Kerberos AD 请求中返回 `KDC_ERR_PREAUTH_REQUIRED`。 默认情况下，Windows Kerberos 客户端在第一个请求中不包括预身份验证信息。

要查看 BDC 为主终结点注册的 SPN 列表，请运行 `setspn -L mssql-master`。 

请参阅以下示例输出：

```bash
Registered ServicePrincipalNames for CN=mssql-master,OU=bdc,DC=mydomain,DC=com:
        MSSQLSvc/bdc-sqlread.mydomain.com:31436
        MSSQLSvc/-sqlread:31436
        MSSQLSvc/bdc-sqlread.mydomain.com
        MSSQLSvc/bdc-sqlread
        MSSQLSvc/bdc-sql.mydomain.com:31433
        MSSQLSvc/bdc-sql:31433
        MSSQLSvc/bdc-sql.mydomain.com
        MSSQLSvc/bdc-sql
        MSSQLSvc/master-p-svc.mydomain.com:1533
        MSSQLSvc/master-p-svc:1533
        MSSQLSvc/master-p-svc.mydomain.com:1433
        MSSQLSvc/master-p-svc:1433
        MSSQLSvc/master-p-svc.mydomain.com
        MSSQLSvc/master-p-svc
        MSSQLSvc/master-svc.mydomain.com:1533
        MSSQLSvc/master-svc:1533
        MSSQLSvc/master-svc.mydomain.com:1433
        MSSQLSvc/master-svc:1433
        MSSQLSvc/master-svc.mydomain.com
        MSSQLSvc/master-svc
```

在上面的结果中，不应注册反向代理地址。

## <a name="resolve"></a>解决

本节介绍可解决此问题的两种方法。 做出适当的更改后，在客户端中运行 `ipconfig -flushdns` 和 `klist purge`。 然后再次尝试连接。

### <a name="option-1"></a>选项 1

在 DNS 中删除每个 BDC 终结点的 CNAME 记录，并将其替换为指向每个 Kubernetes 节点或每个 Kubernetes 主节点（如果有多个主节点）的多个 `A` 记录。

>[!TIP]
>下面所述的脚本使用 PowerShell。 有关详细信息，请参阅[在 Linux 上安装 PowerShell](/powershell/scripting/install/installing-powershell-core-on-linux)。

可以使用以下 PowerShell 脚本更新 DNS 终结点记录。 从连接到同一域的任何计算机运行以下脚本：

```powershell
#Specify the DNS server, example contoso.local
$Domain_DNS_name=mydomain.com'

#DNS records for bdc endpoints
$Controller_DNS_name = 'bdc-control'
$Managment_proxy_DNS_name= 'bdc-proxy'
$Master_Primary_DNS_name = 'bdc-sql'
$Master_Secondary_DNS_name = 'bdc-sqlread'
$Gateway_DNS_name = 'bdc-gateway'
$AppProxy_DNS_name = 'bdc-appproxy'

#Performing Endpoint DNS records Checks..

#Build array of endpoints 
$BdcEndpointsDns = New-Object System.Collections.ArrayList

[void]$BdcEndpointsDns.Add($Controller_DNS_name)
[void]$BdcEndpointsDns.Add($Managment_proxy_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Primary_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Secondary_DNS_name)
[void]$BdcEndpointsDns.Add($Gateway_DNS_name)
[void]$BdcEndpointsDns.Add($AppProxy_DNS_name)

#Build arrary for results 
$BdcEndpointsDns_Result = New-Object System.Collections.ArrayList

foreach ($DnsName in $BdcEndpointsDns) {
    try {
        $endpoint_DNS_record = Resolve-DnsName $DnsName -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop 
        foreach ($ip in $endpoint_DNS_record.IPAddress) {
            [void]$BdcEndpointsDns_Result.Add("OK - $DnsName is an A record with an IP $ip")
        }
    }
    catch {
        [void]$BdcEndpointsDns_Result.Add("MisConfiguration - $DnsName is not an A record or does not exists")
    }  
}

#show the results 
$BdcEndpointsDns_Result
```

### <a name="option-2"></a>方法 2

另外，将 CNAME 修改为指向反向代理的 IP 地址（而不是反向代理的名称）后，也可以解决此问题。

## <a name="confirm-resolution"></a>确认解决方法

使用上面的一种方法解决修复后，通过使用 Active Directory 连接到大数据群集来确认修复。

## <a name="next-steps"></a>后续步骤

[验证域控制器的反向 DNS 条目（PTR 记录）](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller)。
