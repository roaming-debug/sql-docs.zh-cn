---
title: PolyBase Kerberos 连接疑难解答 | Microsoft Docs
description: 若要使用 Kerberos 保护的 Hadoop 群集解决 PolyBase 的身份验证问题，可以使用 PolyBase 中内置的交互式诊断。
author: alazad-msft
ms.author: alazad
ms.reviewer: mikeray
ms.technology: polybase
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/02/2019
ms.prod: sql
ms.prod_service: polybase, sql-data-warehouse, pdw
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: edf0b261b6046d63e037e601ab9e92dd13d8728e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464778"
---
# <a name="troubleshoot-polybase-kerberos-connectivity"></a>PolyBase Kerberos 连接疑难解答

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

将 PolyBase 用于受 Kerberos 保护的 Hadoop 群集时，可以使用 PolyBase 内置的交互式诊断以协助解决身份验证问题。 

本文可作为指南，引导用户利用此内置诊断完成此类问题的整个调试过程。

> [!TIP]
> 在 Kerberos 安全 HDFS 集群中创建外部表时，如果 HDFS Kerberos 失败，则可选择运行 [HDFS Kerberos Tester](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/hdfs-kerberos-tester) 来排除针对 PolyBase 的 HDFS Kerberos 连接故障，而无需遵循本指南中的步骤。
> 此工具将有助于找出非 SQL Server 问题，使你集中精力解决 HDFS Kerberos 设置问题，即识别用户名/密码错误配置的问题和群集 Kerberos 设置配置错误问题。      
> 该工具独立于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 它作为 Jupyter Notebook 提供并且需要 Azure Data Studio。

## <a name="prerequisites"></a>先决条件

1. 安装了 PolyBase 的 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]RTM CU6 / [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 CU3 / [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 或更高版本
1. 受 Kerberos（Active Directory 或 MIT）保护的 Hadoop 群集（Cloudera 或 Hortonworks）

## <a name="introduction"></a>简介
这有助于先了解高级别的 Kerberos 协议。 涉及到三个执行组件：

1. Kerberos 客户端 (SQL Server)
1. 受保护的资源（HDFS、MR2、YARN、作业历史记录等）
1. 密钥发行中心（在 Active Directory 中称为域控制器）

在 Hadoop 群集上配置 Kerberos 时，每个 Hadoop 安全资源均会在“密钥发行中心 (KDC)”中注册一个唯一的“服务主体名称 (SPN)” 。 目的是让客户端获取名为票证授予票证 (TGT) 的临时用户票证，以针对其想要访问的特定 SPN 从 KDC 请求另一个名为服务票证 (ST) 的临时票证。  

在 PolyBase 中，请求对任何受 Kerberos 保护的资源进行身份验证时，均将发生下列四步往返握手：

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接到 KDC 并为用户获取 TGT。 使用 KDC 私钥加密 TGT。
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 调用 Hadoop 安全资源（如 HDFS）并确定需要将 ST 用于哪个 SPN。
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回到 KDC，传回 TGT，并请求 ST 以访问该特定安全资源。 此 ST 使用此受保护服务的私钥进行加密。
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将此 ST 转发给 Hadoop 并进行身份验证，以创建针对该服务的会话。

![Polybase SQL Server](./media/polybase-sqlserver.png)

身份验证问题可归纳为上述四个步骤中的一步或多步。 PolyBase 引入了集成诊断工具，用于帮助确定故障点以加快调试进程。

## <a name="troubleshooting"></a>疑难解答

PolyBase 具有以下包含 Hadoop 群集属性的配置 XML 文件：

- core-site.xml
- hdfs-site.xml
- hive-site.xml
- jaas.conf
- mapred-site.xml
- yarn-site.xml

这些文件位于：

`\[System Drive\]:{install path}\{MSSQL##.INSTANCENAME}\MSSQL\Binn\PolyBase\Hadoop\conf`

例如，[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 的默认位置为 `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf`。

更新 **core-site.xml**，添加以下三个属性。 根据以下环境设置值：

```xml
<property>
    <name>polybase.kerberos.realm</name>
    <value>CONTOSO.COM</value>
</property>
<property>
    <name>polybase.kerberos.kdchost</name>
    <value>kerberos.contoso.com</value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```
> [!NOTE]
> `polybase.kerberos.realm` 属性的值必须全部大写。

如果需要执行下推操作，则稍后也需要更新其他 XML，但是在只配置了这个文件的情况下，至少应该可以访问 HDFS 文件系统。

该工具独立于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 运行，因此更新配置 XML 时无需运行或重启该工具。 若要运行此工具，请在已安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的主机上执行以下命令：

```cmd
> cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase  
> java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge {Name Node Address} {Name Node Port} {Service Principal} {Filepath containing Service Principal's Password} {Remote HDFS file path (optional)}
```

## <a name="arguments"></a>参数

| 参数 | 说明|
| --- | --- |
| *Name Node Address* | 名称节点的 IP 或 FQDN。 指的是 CREATE EXTERNAL DATA SOURCE T-SQL 中的“LOCATION”参数。 注意：此工具的 SQL Server 2019 版本要求 IP 或 FQDN 前面有 hdfs:\/\/。|
| *Name Node Port* | 名称节点的端口。 指的是 CREATE EXTERNAL DATA SOURCE T-SQL 中的“LOCATION”参数。 例如，8020。 |
| *Service Principal* | KDC 的管理服务主体。 匹配 `CREATE DATABASE SCOPED CREDENTIAL` T-SQL 中的“IDENTITY”参数。|
| *Service Password* | 将密码存储在文件中并在此处传递该文件路径，而非在控制台上键入密码。 该文件的内容应与 `CREATE DATABASE SCOPED CREDENTIAL` T-SQL 中用作“SECRET”参数的内容一致。 |
| 远程 HDFS 文件路径（可选） | 要访问的现有文件的路径。 如果未指定，将使用根路径“/”。 |

## <a name="example"></a>示例

```cmd
java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
```

该输出非常详细，可用于促进调试，但是无论使用 MIT 还是 AD，只有四个要查找的主要检查点。 它们与上文列出的四个步骤相对应。 

下列摘要源自 MIT KDC。 可在本文末尾处的“参考资料”中查看 MIT 和 AD 的完整示例输出。

## <a name="checkpoint-1"></a>检查点 1
应有 `Server Principal = krbtgt/MYREALM.COM@MYREALM.COM` 的票证的十六进制转储。 这表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已成功对 KDC 进行身份验证并已收到 TGT。 如果没有，则问题全部存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 KDC 之间，而非存在于 Hadoop 上。

PolyBase 不支持 AD 和 MIT 之间的信任关系，且必须针对 Hadoop 群集中配置的同一个 KDC 对其进行配置。 在此类环境中，在该 KDC 上手动创建服务帐户并使用该帐户执行身份验证将可行。

```cmd
|>>> KrbAsReq creating message 
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=143 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=143 
 >>> KrbKdcReq send: #bytes read=646 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbAsRep cons in KrbAsReq.getReply myuser 
 [2017-04-25 21:34:33,548] INFO 687[main] - com.microsoft.polybase.client.KerberosSecureLogin.secureLogin(KerberosSecureLogin.java:97) - Subject: 
 Principal: admin_user@CONTOSO.COM 
 Private Credential: Ticket (hex) = 
 0000: 61 82 01 48 30 82 01 44 A0 03 02 01 05 A1 0E 1B a..H0..D........ 
 0010: 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 21 30 .CONTOSO.COM.!0 
 0020: 1F A0 03 02 01 02 A1 18 30 16 1B 06 6B 72 62 74 ........0...krbt 
 0030: 67 74 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D gt..CONTOSO.COM 
 0040: A3 82 01 08 30 82 01 04 A0 03 02 01 10 A1 03 02 ....0........... 
 *[...Condensed...]* 
 0140: 67 6D F6 41 6C EB E0 C3 3A B2 BD B1 gm.Al...:... 
 Client Principal = admin_user@CONTOSO.COM 
 Server Principal = krbtgt/CONTOSO.COM@CONTOSO.COM 
 *[...Condensed...]* 
 [2017-04-25 21:34:34,500] INFO 1639[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1579) - Successfully authenticated against KDC server. 
```

## <a name="checkpoint-2"></a>检查点 2
PolyBase 将尝试访问 HDFS 并将失败，因为请求不包含必要的服务票证。

```cmd
 [2017-04-25 21:34:34,501] INFO 1640[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1584) - Attempting to access external filesystem at URI: hdfs://10.193.27.232:8020 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Entered Krb5Context.initSecContext with state=STATE_NEW 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Service ticket not found in the subject 
```

## <a name="checkpoint-3"></a>检查点 3
第二个十六进制转储指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 成功使用 TGT 并从 KDC 获得了该名称节点 SPN 的适用服务票证。

```cmd
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=664 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=664 
 >>> KrbKdcReq send: #bytes read=669 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbApReq: APOptions are 00100000 00000000 00000000 00000000 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 Krb5Context setting mySeqNumber to: 1033039363 
 Created InitSecContextToken: 
 0000: 01 00 6E 82 02 4B 30 82 02 47 A0 03 02 01 05 A1 ..n..K0..G...... 
 0010: 03 02 01 0E A2 07 03 05 00 20 00 00 00 A3 82 01 ......... ...... 
 0020: 63 61 82 01 5F 30 82 01 5B A0 03 02 01 05 A1 0E ca.._0..[....... 
 0030: 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 26 ..CONTOSO.COM.& 
 0040: 30 24 A0 03 02 01 00 A1 1D 30 1B 1B 02 6E 6E 1B 0$.......0...nn. 
 0050: 15 73 68 61 73 74 61 2D 68 64 70 32 35 2D 30 30 .hadoop-hdp25-00 
 0060: 2E 6C 6F 63 61 6C A3 82 01 1A 30 82 01 16 A0 03 .local....0..... 
 0070: 02 01 10 A1 03 02 01 01 A2 82 01 08 04 82 01 04 ................ 
 *[...Condensed...]* 
 0240: 03 E3 68 72 C4 D2 8D C2 8A 63 52 1F AE 26 B6 88 ..hr.....cR..&.. 
 0250: C4 . 
```

## <a name="checkpoint-4"></a>检查点 4
最后，应显示目标路径的文件属性，并有一条确认消息。 该文件属性确认 Hadoop 已使用 ST 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进行身份验证，并且已授权会话访问受保护资源。

到达此环节即表示确认以下几点：(i) 这三个执行组件可以正常通信，(ii) core-site.xml 和 jaas.conf 正确，且 (iii) KDC 已识别凭据。

```cmd
 [2017-04-25 21:34:35,096] INFO 2235[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1586) - File properties for "/": FileStatus{path=hdfs://10.193.27.232:8020/; isDirectory=true; modification_time=1492028259862; access_time=0; owner=hdfs; group=hdfs; permission=rwxr-xr-x; isSymlink=false} 
 [2017-04-25 21:34:35,098] INFO 2237[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1587) - Successfully accessed the external file system. 
```

## <a name="common-errors"></a>常见错误
如果此工具正在运行且目标路径的文件属性未显示（检查点 4），在此过程中应该会引发异常。 检查此异常，并考虑四步流程中发生此异常的步骤的上下文。 依次考虑可能会发生的以下常见问题：

| 异常和消息 | 原因 | 
| --- | --- |
| org.apache.hadoop.security.AccessControlException<br>未启用简单身份验证。 可用: [令牌，KERBEROS] | core-site.xml 未将 hadoop.security.authentication 属性设置为“KERBEROS”。|
|javax.security.auth.login.LoginException<br>Kerberos 数据库中找不到客户端  (6) - CLIENT_NOT_FOUND |    提供的管理服务主体在 core-site.xml 中指定的领域不存在。|
| javax.security.auth.login.LoginException<br> 校验和失败 |管理服务主体存在，但是密码错误。 |
| 本机配置名：C:\Windows\krb5.ini<br>从本机配置加载 | 此消息表示 Java 的 krb5LoginModule 检测到了计算机上的自定义客户端配置。 检查自定义客户端设置，因为可能是它们导致出现此问题。 |
| javax.security.auth.login.LoginException<br>java.lang.IllegalArgumentException<br>非法主体名称 admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule：没有规则适用于 admin_user@CONTOSO.COM | 根据 Hadoop 群集的相应规则将“hadoop.security.auth_to_local”属性添加到 core-site.xml。 |
| java.net.ConnectException<br>正在尝试访问 URI: hdfs://10.193.27.230:8020 上的外部文件系统<br>由于连接异常，从 IAAS16981207/10.107.0.245 调用 10.193.27.230:8020 失败 | 对 KDC 的身份验证成功，但是未能访问 Hadoop 名称节点。 检查名称节点 IP 和端口。 验证是否已禁用 Hadoop 上的防火墙。 |
| java.io.FileNotFoundException<br>文件不存在: /test/data.csv |    身份验证成功，但是指定的位置不存在。 首先检查路径，或使用根路径“/”进行测试。 |

## <a name="debugging-tips"></a>调试提示

### <a name="mit-kdc"></a>MIT KDC  
在 KDC 主机或任何经过配置的 KDC 客户端上运行 kadmin.local >（管理员登录）> listprincs 可查看已注册 KDC 的所有 SPN，包括管理员。 如果在 Hadoop 群集上正确配置了 Kerberos，则群集中的每个可用服务均应有一个 SPN（例如 `nn`、`dn`、`rm`、`yarn`、`spnego` 等）默认情况下，其对应的 keytab 文件（密码替换项）位于 /etc/security/keytabs 下。 已使用 KDC 私钥对它们进行加密。  

也可考虑使用 [`kinit`](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) 在本地 KDC 上验证管理员凭据。 用法示例为：`kinit identity@MYREALM.COM`。 密码提示框指示存在标识。  
默认情况下，可在 /var/log/krb5kdc.log 中获取 KDC 日志，其中包括所有的票证请求（包括生成请求的客户端 IP）。 应有来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机 IP（此工具的运行位置）的两个请求：第一个是针对身份验证服务器的 TGT 的 AS\_REQ，第二个是针对票证授予服务器的 ST 的 TGS\_REQ 。

```bash
 [root@MY-KDC log]# tail -2 /var/log/krb5kdc.log 
 May 09 09:48:26 MY-KDC.local krb5kdc[2547](info): **AS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **krbtgt/CONTOSO.COM@CONTOSO.COM** 
 May 09 09:48:29 MY-KDC.local krb5kdc[2547](info): **TGS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **nn/hadoop-hdp25-00.local@CONTOSO.COM** 
```

### <a name="active-directory"></a>Active Directory 
在 Active Directory 中，可通过浏览至“控制面板 > Active Directory 用户和计算机 > MyRealm >  MyOrganizationalUnit”查看 SPN。 如果在 Hadoop 群集上正确配置了 Kerberos，则每个可用服务均有一个 SPN（例如 `nn`、`dn`、`rm`、`yarn`、`spnego` 等）

### <a name="general-debugging-tips"></a>常规调试提示
如果有一些 Java 经验，则有助于查看日志并调试 Kerberos 问题，这些问题与 SQL Server PolyBase 功能无关。

如果仍然无法访问 Kerberos，请按照以下步骤进行调试：

1. 确保可从外部 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问 Kerberos HDFS 数据。 可以： 

    - 编写自己的 Java 程序或
    - 使用 PolyBase 安装文件夹中的 `HdfsBridge` 类。 例如：

      ```java
      -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
      ```

     在上述示例中，`admin_user` 仅包含用户名，不包含任何域部分。

2. 如果无法从 PolyBase 外部访问 Kerberos HDFS 数据：
    - 有两种类型的 Kerberos 身份验证：Active Directory Kerberos 身份验证和 MIT Kerberos 身份验证。
    - 确保用户存在于域帐户中，并在尝试访问 HDFS 时使用相同的用户帐户。

3. 对于活动目录 Kerberos，请确保可在 Windows 上使用 `klist` 命令查看缓存的票证。
    - 登录到 PolyBase 计算机并在命令提示符中运行 `klist` 和 `klist tgt` 以查看 KDC、用户名和加密类型是否正确。

4. 如果 KDC 仅支持 AES256，请确保已安装 [JCE 策略文件](http://www.oracle.com/technetwork/java/javase/downloads/index.html)。

## <a name="see-also"></a>另请参阅
[使用 Active Directory 身份验证将 PolyBase 与 Cloudera 集成](/archive/blogs/microsoftrservertigerteam/integrating-polybase-with-cloudera-using-active-directory-authentication)  
[Cloudera 的 Kerberos for CDH 设置指南](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_sg_principal_keytab.html)  
[Hortonworks 的 Kerberos for HDP 设置指南](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/ch_configuring_amb_hdp_for_kerberos.html)  
[PolyBase 故障排除](polybase-troubleshooting.md)