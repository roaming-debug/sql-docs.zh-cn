---
title: AD 模式部署已停止 - 缺少 DC 的反向查找区域条目
titleSuffix: SQL Server Big Data Cluster
description: 由于域控制器 DNS 服务器中的域控制器缺少反向查找区域条目，导致 AD 模式的 BDC 部署停滞。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02276cee62edd604df5e018bc22cf86cddaaec03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043947"
---
# <a name="ad-mode-deployment-stopped---missing-reverse-lookup-zone-entry-for-dc"></a>AD 模式部署已停止 - 缺少 DC 的反向查找区域条目

Active Directory (AD) 模式下的部署将会停止。 检查症状，判断是否由于域控制器 DNS 服务器缺少反向查找区域条目。 

## <a name="symptom"></a>症状

已开始在 AD 模式下部署 BDC，但部署停滞，无法继续。

以下示例展示了 bash shell 中的部署结果。

```
The privacy statement can be viewed at:
https://go.microsoft.com/fwlink/?LinkId=853010
 
The license terms for SQL Server Big Data Cluster can be viewed at:
Enterprise: https://go.microsoft.com/fwlink/?linkid=2104292
Standard: https://go.microsoft.com/fwlink/?linkid=2104294
Developer: https://go.microsoft.com/fwlink/?linkid=2104079
 
Cluster deployment documentation can be viewed at:
https://aka.ms/bdc-deploy
 
NOTE: Cluster creation can take a significant amount of time depending on
configuration, network speed, and the number of nodes in the cluster.
 
Starting cluster deployment.
Cluster controller endpoint is available at bdc-control.contoso.com:30080, 193.168.5.14:30080.
Waiting for control plane to be ready after 5 minutes.
Waiting for control plane to be ready after 10 minutes.
Waiting for control plane to be ready after 15 minutes.
Waiting for control plane to be ready after 20 minutes.
Waiting for control plane to be ready after 25 minutes.
```

检查当前已部署的 Pod。

```bash
kubectl get pods -n mssql-cluster
```

以下结果表明仅部署了属于控制器的 Pod。 未创建用于计算、数据或存储的 Pod。

```
NAME              READY   STATUS    RESTARTS   AGE
control-rts5t     3/3     Running   0          18m
controldb-0       2/2     Running   0          18m
controlwd-csgst   1/1     Running   0          16m
dns-7kfnz         2/2     Running   0          16m
logsdb-0          1/1     Running   0          16m
logsui-2pc29      1/1     Running   0          16m
metricsdb-0       1/1     Running   0          16m
metricsdc-4rtm4   1/1     Running   0          16m
metricsdc-6lr2t   1/1     Running   0          16m
metricsdc-ftx9m   1/1     Running   0          16m
metricsdc-h59jb   1/1     Running   0          16m
metricsui-lvdpt   1/1     Running   0          16m
mgmtproxy-mkmxp   2/2     Running   0          16m
```

检查安全支持容器日志。 查找 LDAP 错误。 

## <a name="check-security-support-container"></a>检查安全支持容器 

查看安全支持容器日志。

以下命令收集命名空间 `mssql-cluster` 的群集中的安全支持日志。

```bash
azdata bdc debug copy-logs -n mssql-cluster -c security-support
```

提取日志并找到 `\mssql-cluster\control-<identifier>\controller\control-rts5t-controller-stdout.log`。

> [!TIP]
> 可通过多种方式来收集日志。 可以使用 Azure Data Studio 中的笔记本，而不是使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 复制日志。
> 在 Azure Data Studio 中，连接到 Kubernetes 群集，并运行相应的故障排除笔记本。 以下是笔记本的示例。
>
> - TSG027 - 观察群集部署
> - TSG061 - 获取 BDC 命名空间中 Pod 所有容器日志的尾部
> - TSG001 - 运行 `azdata copy-logs`
>

## <a name="inspect-the-logs"></a>检查日志

找到日志。 以下示例指向控制器部署日志。 

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`"

```
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'cntrl-controller'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=cntrl-controller,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'ldap-user'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=ldap-user,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'nginx-mgmtproxy'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=nginx-mgmtproxy,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
```

## <a name="cause"></a>原因

域控制器 DNS 条目中的域控制器的反向查找区域条目丢失。 

## <a name="resolution"></a>解决方法

运行以下 PowerShell 脚本以确认是否配置了反向 DNS 条目（PTR 记录）。

```powershell
#Domain Controller FQDN 'DCserver01.contoso.local'
$Domain_controller_FQDN = 'DCserver01.contoso.local'

#Performing Domain Controller DNS record, reverse PTR Checks...
$DcControllerDnsPtr_Result = New-Object System.Collections.ArrayList
try {
    $Domain_controller_DNS_Record = Resolve-DnsName $Domain_controller_FQDN -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop
    foreach ($ip in $Domain_controller_DNS_Record.IPAddress) {
        #resolving hostname by IP address to make sure we have reverse PTR record 
        if ((Resolve-DnsName $ip).NameHost -eq $Domain_controller_FQDN) {
            [void]$DcControllerDnsPtr_Result.add("OK - $Domain_controller_FQDN has an A record with an IP $ip, Reverse PTR record is in place") 
        }
        else {
            [void]$DcControllerDnsPtr_Result.add("Missing - $Domain_controller_FQDN has an A record with an IP $ip, But no reverse PTR record was found for the host")
        }
    }
}
catch {
    [void]$DcControllerDnsPtr_Result.add("Error - " + $_.exception.message)
}

#show the results 
$DcControllerDnsPtr_Result
```

## <a name="next-steps"></a>后续步骤

[验证域控制器的反向 DNS 条目（PTR 记录）](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller)。
