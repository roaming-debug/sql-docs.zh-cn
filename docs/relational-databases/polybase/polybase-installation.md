---
title: 在 Windows 上安装 PolyBase | Microsoft Docs
description: 了解如何将 PolyBase 安装为单节点或 PolyBase 横向扩展组。 可以使用安装向导或命令提示符。 最后，启用 PolyBase。
ms.date: 09/24/2018
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
helpviewer_keywords:
- PolyBase, installation
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 00cb6468e269b057fb5d5d539a3f0efdd8be36ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351814"
---
# <a name="install-polybase-on-windows"></a>在 Windows 上安装 PolyBase

[!INCLUDE [SQL Server Windows Only ](../../includes/applies-to-version/sql-windows-only.md)]

若要安装 SQL Server 试用版，请转到 [SQL Server 评估](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)。 
   
## <a name="prerequisites"></a>先决条件  
   
- 64 位 SQL Server 评估版。  
   
- Microsoft .NET Framework 4.5。  

- 最小内存：4 GB。 
   
- 最小硬盘空间：2 GB。
  
- 建议：至少 16 GB 的 RAM。
   
- 必须启用 TCP/IP 才能使 PolyBase 正常工作。 SQL Server 的所有版本均默认启用 TCP/IP，Developer 和 Express SQL Server 版本除外。 若要使 PolyBase 在 Developer 和 Express 版本上正常工作，必须启用 TCP/IP 连接。 请参阅[启用或禁用服务器网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)。


>[!NOTE] 
> 在每台计算机上仅可在一个 SQL Server 实例上安装 PolyBase。


>[!NOTE]
>若要使用 PolyBase，必须对数据库具有 sysadmin 或 CONTROL SERVER 级别的权限。

>[!IMPORTANT]
>若要对 Hadoop 使用计算下推功能，目标 Hadoop 群集必须具有 HDFS、YARN 和 MapReduce 的核心组件且已启用作业历史记录服务器。 PolyBase 通过 MapReduce 提交下推查询，并且从作业历史记录服务器拉取状态。 缺少任一组件，查询都会失败。
  
## <a name="single-node-or-polybase-scale-out-group"></a>单节点或 PolyBase 横向扩展组

在 SQL Server 实例上安装 PolyBase 之前，决定是需要单节点安装还是需要 [PolyBase 横向扩展组](../../relational-databases/polybase/polybase-scale-out-groups.md)。

对于 PolyBase 横向扩展组，请确保：

- 所有计算机都位于同一域中。
- 在 PolyBase 安装期间使用相同的服务帐户和密码。
- SQL Server 实例彼此之间可以通过网络进行通信。
- SQL Server 实例的 SQL Server 版本相同。

独立安装或在横向扩展组中安装 PolyBase 后，将无法再更改。 若要更改此设置，必须卸载并重新安装该功能。

## <a name="use-the-installation-wizard"></a>使用安装向导
   
1. 运行 SQL Server setup.exe。   
   
2. 选择“安装”，然后选择“全新独立 SQL Server 安装或添加功能” 。  
   
3. 在“功能选择”页上，选择“针对外部数据的 PolyBase 查询服务”。  

   ![PolyBase 服务](../../relational-databases/polybase/media/install-wizard.png "PolyBase 服务")  
   
   >[!NOTE]
   >SQL Server 2019 PolyBase 现在包含附加选项“适用于 HDFS 数据源的 Java 连接器”。 若要详细了解此功能，请参阅 [SQL Server 预览功能](https://cloudblogs.microsoft.com/sqlserver/2019/04/24/sql-server-2019-community-technology-preview-2-5-is-now-available/)。
   
4. 在服务器配置页上，将“SQL Server PolyBase 引擎服务”和“SQL Server PolyBase 数据移动服务”配置为在同一域帐户下运行 。  

   >[!IMPORTANT]
   >在 PolyBase 横向扩展组中，所有节点上的 PolyBase 引擎和 PolyBase 数据移动服务必须在同一个域帐户下运行。 请参阅 [PolyBase 横向扩展组](#enable)。

5. 在“PolyBase 配置页”上，选择两个选项之一。 有关详细信息，请参阅 [PolyBase 横向扩展组](../../relational-databases/polybase/polybase-scale-out-groups.md)。  
   
   - 将 SQL Server 实例用作已启用 PolyBase 的独立实例。  
   
     选择此选项可将 SQL Server 实例用作独立的头节点。  
   
   - 将 SQL Server 实例作为 PolyBase 横向扩展组的一部分使用。 此选项将打开防火墙以允许传入连接。 允许与 SQL Server 数据库引擎、SQL Server PolyBase 引擎、SQL Server PolyBase 数据移动服务和 SQL Browser 建立连接。 防火墙还允许来自 PolyBase 横向扩展组中其他节点的传入连接。  
   
     此选项还将启用 Microsoft 分布式事务处理协调器 (MSDTC) 防火墙连接并修改 MSDTC 注册表设置。  
   
6. 在 PolyBase 配置页上，指定具有至少六个端口的端口范围。 SQL Server 安装程序分配该范围中的前六个可用端口。  

   >[!IMPORTANT]
   > 安装后，必须[启用 PolyBase 功能](#enable)。


##  <a name="use-a-command-prompt"></a><a name="installing"></a> 使用命令提示符

使用此表中的值来创建安装脚本。 SQL Server PolyBase 引擎和 SQL Server PolyBase 数据移动服务必须在同一帐户下运行。 在 PolyBase 横向扩展组中，所有节点上的 PolyBase 服务必须在同一个域帐户下运行。  
   
<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017"

|SQL Server 组件 (SQL Server component)|参数和值|说明|  
|--------------------------|--------------------------|-----------------|  
|SQL Server 安装程序控件|**必需**<br /><br /> /FEATURES=PolyBase|选择 PolyBase 功能。|  
|SQL Server PolyBase 引擎|**可选**<br /><br /> /PBENGSVCACCOUNT|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|SQL Server PolyBase 引擎|**可选**<br /><br /> /PBENGSVCPASSWORD|指定引擎服务帐户的密码。|  
|SQL Server PolyBase 引擎|**可选**<br /><br /> /PBENGSVCSTARTUPTYPE|指定 PolyBase 引擎的启动模式：自动（默认）、禁用和手动。|  
|SQL Server PolyBase 数据移动 |**可选**<br /><br /> /PBDMSSVCACCOUNT|指定数据移动服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|SQL Server PolyBase 数据移动 |**可选**<br /><br /> /PBDMSSVCPASSWORD|指定数据移动帐户的密码。|  
|SQL Server PolyBase 数据移动 |**可选**<br /><br /> /PBDMSSVCSTARTUPTYPE|指定数据移动服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|**可选**<br /><br /> /PBSCALEOUT|指定是否将 SQL Server 实例用作 PolyBase 横向扩展计算组的一部分。 <br />支持的值：True、False。|  
|PolyBase|**可选**<br /><br /> /PBPORTRANGE|指定一个端口范围，其中至少具有六个端口用于 PolyBase 服务。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

|SQL Server 组件 (SQL Server component)|参数和值|说明|  
|--------------------------|--------------------------|-----------------|  
|SQL Server 安装程序控件|**必需**<br /><br /> /FEATURES=PolyBaseCore, PolyBaseJava, PolyBase | PolyBaseCore 安装对除 Hadoop 连接外的所有 PolyBase 功能的支持。 PolyBaseJava 支持 Hadoop 连接。 PolyBase 安装两者。 |  
|SQL Server PolyBase 引擎|**可选**<br /><br /> /PBENGSVCACCOUNT|指定引擎服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|SQL Server PolyBase 引擎|**可选**<br /><br /> /PBENGSVCPASSWORD|指定引擎服务帐户的密码。|  
|SQL Server PolyBase 引擎|**可选**<br /><br /> /PBENGSVCSTARTUPTYPE|指定 PolyBase 引擎的启动模式：自动（默认）、禁用和手动。|  
|SQL Server PolyBase 数据移动 |**可选**<br /><br /> /PBDMSSVCACCOUNT|指定数据移动服务的帐户。 默认为 **NT Authority\NETWORK SERVICE**。|  
|SQL Server PolyBase 数据移动 |**可选**<br /><br /> /PBDMSSVCPASSWORD|指定数据移动帐户的密码。|  
|SQL Server PolyBase 数据移动 |**可选**<br /><br /> /PBDMSSVCSTARTUPTYPE|指定数据移动服务的启动模式：自动（默认）、禁用和手动。|  
|PolyBase|**可选**<br /><br /> /PBSCALEOUT|指定是否将 SQL Server 实例用作 PolyBase 横向扩展计算组的一部分。 <br />支持的值：True、False。|  
|PolyBase|**可选**<br /><br /> /PBPORTRANGE|指定一个端口范围，其中至少具有六个端口用于 PolyBase 服务。 示例：<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end

安装后，必须[启用 PolyBase 功能](#enable)。



**示例**

此示例显示一个示例安装程序脚本。  

```cmd
   
Setup.exe /Q /ACTION=INSTALL /IACCEPTSQLSERVERLICENSETERMS /FEATURES=SQLEngine,PolyBase   
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="\<fabric-domain>\Administrator"   
/INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /PBSCALEOUT=TRUE   
/PBPORTRANGE=16450-16460 /SECURITYMODE=SQL /SAPWD="<StrongPassword>"   
/PBENGSVCACCOUNT="<DomainName>\<UserName>" /PBENGSVCPASSWORD="<StrongPassword>"   
/PBDMSSVCACCOUNT="<DomainName>\<UserName>" /PBDMSSVCPASSWORD="<StrongPassword>"  
   
```  

## <a name="enable-polybase"></a><a id="enable"></a> 启用 PolyBase

安装完成后，必须启用 PolyBase 来获取其功能。 使用以下 Transact-SQL 命令。 在大数据群集安装过程中部署的 SQL 2019 实例默认启用此设置。


```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE;
```


## <a name="post-installation-notes"></a>安装后注意事项  

PolyBase 安装三个用户数据库：DWConfiguration、DWDiagnostics 和 DWQueue。 这些数据库供 PolyBase 使用。 请勿更改或删除它们。  
   
### <a name="how-to-confirm-installation"></a><a id="confirminstall"></a> 如何确认安装  

运行以下命令。 如果已安装 PolyBase，则返回 1。 否则返回 0。  

```sql  
SELECT SERVERPROPERTY ('IsPolyBaseInstalled') AS IsPolyBaseInstalled;  
```  

### <a name="firewall-rules"></a>防火墙规则  

SQL Server PolyBase 安装程序将在计算机上创建以下防火墙规则：  
   
- SQL Server PolyBase - 数据库引擎 - \<SQLServerInstanceName> (TCP-In)  
   
- SQL Server PolyBase - PolyBase 服务 - \<SQLServerInstanceName> (TCP-In)  

- SQL Server PolyBase - SQL Browser - (UDP-In)  
   
安装时，如果使用 SQL Server 实例作为 PolyBase 横向扩展组中的一部分，则将启用这些规则。 防火墙将开启以允许传入连接。 允许将这些连接用于 SQL Server 数据库引擎、SQL Server PolyBase 引擎、SQL Server PolyBase 数据移动服务和 SQL Browser。 如果安装期间未运行计算机上的防火墙服务，SQL Server 安装程序将无法启用这些规则。 在这种情况下，请在计算机上启动防火墙服务并启用这些安装后规则。  
   
#### <a name="to-enable-the-firewall-rules"></a>启用防火墙规则的步骤  

1. 打开“控制面板”  

2. 选择“系统和安全”，然后选择“Windows 防火墙” 。  
   
3. 选择“高级设置”，然后选择“入站规则” 。  
   
4. 右键单击已禁用的规则，然后选择“启用规则”。  
   
### <a name="polybase-service-accounts"></a>PolyBase 服务帐户

若要更改 PolyBase 引擎和 PolyBase 数据移动服务的服务帐户，请卸载并重新安装 PolyBase 功能。

## <a name="next-steps"></a>后续步骤  

请参阅 [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md)。
