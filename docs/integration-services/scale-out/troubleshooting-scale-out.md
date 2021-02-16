---
title: SSIS Scale Out 故障排除 | Microsoft Docs
description: 排查 SSIS Scale Out 的常见问题，如配置错误和缺少访问权限。
ms.custom: performance
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 7d9c02b6912019304b37d3d536312c6a5707f769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337417"
---
# <a name="troubleshoot-scale-out"></a>Scale Out 故障排除

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



SSIS Scale Out 涉及 SSIS 目录数据库 `SSISDB`、Scale Out Master 服务和 Scale Out Worker 服务之间的通信。 有时，通信会因配置错误、缺少访问权限及其他原因而中断。 可借助本文排查 Scale Out 配置的问题。

要调查出现的问题，请按照下面的步骤逐一操作，直到解决问题。

## <a name="scale-out-master-fails"></a>Scale Out Master 故障

### <a name="symptoms"></a>症状 
-   Scale Out Master 无法连接 SSISDB。 

-   Scale Out Manager 中无法显示主控形状属性。

-   主控形状属性未填入 `[catalog].[master_properties]` 视图。

### <a name="solution"></a>解决方案
1.  检查是否已启用 Scale Out。

    在 SSMS 的对象资源管理器中，右键单击“SSISDB”，然后选中“启用 Scale Out 功能”   。

    ![是否已启用 Scale Out](media/isenabled.PNG)

    如果属性值为 False，通过调用存储过程 `[catalog].[enable_scaleout]` 启用 Scale Out。

2.  检查 Scale Out Master 配置文件中指定的 SQL Server 名称是否正确，然后重启 Scale Out Master 服务。

## <a name="scale-out-worker-fails"></a>Scale Out Worker 故障

### <a name="symptoms"></a>症状 
-   Scale Out Worker 无法连接 Scale Out Master。

-   Scale Out Worker 在添加到 Scale Out Manager 中后无法显示。

-   Scale Out Worker 在 `[catalog].[worker_agents]` 视图中不显示。

-   Scale Out Worker 服务处于运行状态，而 Scale Out Worker 处于脱机状态。

### <a name="solution"></a>解决方案
在 `\<drive\>:\Users\\*[account running worker service]*\AppData\Local\SSIS\Cluster\Agent` 下查看 Scale Out Worker 服务日志中的错误消息。

## <a name="no-endpoint-listening"></a>无终结点侦听

### <a name="symptoms"></a>症状

“System.ServiceModel.EndpointNotFoundException：  可接受消息的 https://[MachineName]:[Port]/ClusterManagement/ 下无任何终结点侦听。”

### <a name="solution"></a>解决方案

1.  检查 Scale Out Master 服务配置文件中指定的端口号是否正确，然后重启 Scale Out Master 服务。 

2.  检查 Scale Out Worker 服务配置中指定的主终结点是否正确，然后重启 Scale Out Worker 服务。

3.  检查 Scale Out Mater 节点上的防火墙端口是否开启。

4.  解决 Scale Out Master 节点和 Scale Out Worker 节点之间任何其他的连接问题。

## <a name="could-not-establish-trust-relationship"></a>无法建立信任关系

### <a name="symptoms"></a>症状
“System.ServiceModel.Security.SecurityNegotiationException:  无法使用授权 '[Machine Name]:[Port]' 与 SSL/TLS 安全通道建立信任关系。”。

“System.Net.WebException:已关闭基础连接：无法与 SSL/TLS 安全通道建立信任关系。”

“System.Security.Authentication.AuthenticationException:  根据验证过程，远程证书无效。”

### <a name="solution"></a>解决方案
1.  如果尚未安装 Scale Out Master 证书，请将其安装到 Scale Out Worker 节点上本地计算机的根证书存储中，然后重启 Scale Out Worker 服务。

2.  检查主终结点的主机名是否包含在 Scale Out Master 证书的 CN 中。 如果没有，重置 Scale Out Worker 配置文件中的主终结点并重启 Scale Out Worker 服务。 

    > [!NOTE]
    > 如果因 DNS 设置不能更改主终结点的主机名，则必须更改 Scale Out Master 证书。 请参阅[管理 SSIS Scale Out 证书](deal-with-certificates-in-ssis-scale-out.md)。

3.  检查 Scale Out Worker 配置中指定的主指纹是否匹配 Scale Out Master 证书的指纹。 

## <a name="could-not-establish-secure-channel"></a>无法建立安全通道

### <a name="symptoms"></a>症状

“System.ServiceModel.Security.SecurityNegotiationException:  无法使用授权 '[Machine Name]:[Port]' 与 SSL/TLS 建立安全通道。”

“System.Net.WebException:请求已中止：无法创建 SSL/TLS 安全通道。”

### <a name="solution"></a>解决方案
通过运行以下命令，检查运行 Scale Out Worker 服务的帐户是否有权访问 Scale Out Worker 证书：

```dos
winhttpcertcfg.exe -l -c LOCAL_MACHINE\MY -s {CN of the worker certificate}
```

如果帐户无权访问，可通过运行以下命令授予访问权限并重启 Scale Out Worker 服务。

```dos
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the worker certificate} -a {the account running Scale Out Worker service}
```

## <a name="http-request-forbidden"></a>HTTP 请求已禁止

### <a name="symptoms"></a>症状

“System.ServiceModel.Security.MessageSecurityException:  HTTP 请求已使用客户端身份验证方案 'Anonymous' 禁止。”

“System.Net.WebException:远程服务器返回了错误：(403) 已禁止。”

### <a name="solution"></a>解决方案
1.  如果尚未安装 Scale Out Worker 证书，请将其安装到 Scale Out Master 节点上本地计算机的根证书存储中，然后重启 Scale Out Worker 服务。

2.  清理 Scale Out Master 节点上本地计算机根证书存储中的无用证书。

3.  将 Schannel 配置为在 TLS/SSL 握手过程中不再发送受信任的根证书颁发机构列表，方法是在 Scale Out Master 节点上添加以下注册表项。

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    值名称：**SendTrustedIssuerList** 

    值类型：**REG_DWORD** 

    值数据：**0 (False)**

4.  如果不能清理如步骤 2 中所述的所有非自签名证书，将下面的注册表项值设置为 2。

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    值名称：**ClientAuthTrustMode** 

    值类型：**REG_DWORD** 

    值数据：**2**

    > [!NOTE]
    > 如果根证书存储中有非自签名证书，客户端证书身份验证将失败。 有关详细信息，请参阅 [Internet Information Services (IIS) 8 可能会拒绝客户端证书请求并随附 HTTP 403.7 或 403.16 错误](https://support.microsoft.com/help/2802568/internet-information-services-iis-8-may-reject-client-certificate-requ)。

## <a name="http-request-error"></a>HTTP 请求错误

### <a name="symptoms"></a>症状

“System.ServiceModel.CommunicationException:向 https://[Machine Name]:[Port]/ClusterManagement/ 发出 HTTP 请求时出错。这可能是因为未正确使用 HTTP.SYS 在 HTTPS 事例中配置服务器证书。此外，客户端与服务器之间的安全绑定不匹配，也可能造成该异常。”

### <a name="solution"></a>解决方案
1.  使用以下命令，检查 Scale Out Master 证书是否已正确绑定到主节点上主终结点中的端口：

    ```dos
    netsh http show sslcert ipport=0.0.0.0:{Master port}
    ```

    检查显示的证书哈希与 Scale Out Master 证书指纹是否匹配。 如果绑定不正确，运行以下命令重置绑定并重启 Scale Out Worker 服务。

    ```dos
    netsh http delete sslcert ipport=0.0.0.0:{Master port}
    netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={Master certificate thumbprint} certstorename=Root  appid={random guid}
    ```

## <a name="cannot-open-certificate-store"></a>无法打开证书存储

### <a name="symptoms"></a>症状
在 Scale Out Manager 中，Scale Out Worker 连接到 Scale Out Master 时验证失败，并收到错误消息：“无法打开计算机上的证书存储”  。

### <a name="solution"></a>解决方案

1.  以管理员身份运行 Scale Out Manager。 如果使用 SSMS 打开 Scale Out Manager，需要以管理员身份运行 SSMS。

2.  如果远程注册表服务未运行，在计算机上启动它。

## <a name="execution-doesnt-start"></a>无法启动执行

### <a name="symptoms"></a>症状
Scale Out 中“执行”不启动。

### <a name="solution"></a>解决方案

检查所选计算机的状态（该计算机用于在 `[catalog].[worker_agents]` 视图中运行包）。 必须至少联机启动一个辅助角色。

## <a name="no-log"></a>无日志

### <a name="symptoms"></a>症状 
包运行成功，但未记录任何消息。

### <a name="solution"></a>解决方案

检查托管 SSISDB 的 SQL Server 实例是否允许 SQL Server 身份验证。

> [!NOTE]  
> 如果已更改用于 Scale Out 日志记录的帐户，请参阅[更改用于 Scale Out 日志记录的帐户](change-logdb-account.md)并验证用于日志记录的连接字符串。

## <a name="error-messages-arent-helpful"></a>错误消息无法提供帮助

### <a name="symptoms"></a>症状
包执行报告中的错误消息不充足，无法进行故障排除。

### <a name="solution"></a>解决方案
可在 `WorkerSettings.config` 中配置的 `TasksRootFolder` 下找到更多执行日志。 默认情况下，此文件夹为 `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`。 [account] 是运行 Scale Out Worker 服务的帐户，其默认值为 `SSISScaleOutWorker140`  。

要为执行 ID 为 [execution ID] 的包查找日志，请执行以下 Transact-SQL 命令，获取 [task ID]（任务 ID）   。 然后，在 `TasksRootFolder` 下查找包含 [task ID] 的子文件夹名称  。

```sql
SELECT [TaskId]
FROM [SSISDB].[internal].[tasks] tasks, [SSISDB].[internal].[executions] executions 
WHERE executions.execution_id = *Your Execution Id* AND tasks.JobId = executions.job_id
```

> [!WARNING]
> 此查询仅用于故障排除。 查询中引用的内部视图将来会发生更改。 

## <a name="next-steps"></a>后续步骤
有关详细信息，请参阅下列关于设置和配置 SSIS Scale Out 的相关文章：
-   [在单台计算机上开始使用 Integration Services (SSIS) Scale Out](get-started-with-ssis-scale-out-onebox.md)
-   [演练：设置 Integration Services (SSIS) Scale Out](walkthrough-set-up-integration-services-scale-out.md)
