---
description: MSSQLSERVER_18483
title: MSSQLSERVER_18483
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18483 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 82b12abb1121ed8f9523a267b5dbe98464c736f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345545"
---
# <a name="mssqlserver_18483"></a>MSSQLSERVER_18483
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|18483|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|REMLOGIN_INVALID_USER|
|消息正文|由于没有在服务器 '%.ls' 上将 '%. ls' 定义为远程登录名，所以无法连接到该服务器。 请确保指定的登录名正确无误。 %.*ls。|
||

## <a name="explanation"></a>说明

当你尝试在使用初始安装了 SQL 实例的另一台计算机的硬盘映像进行还原的系统上配置复制分发服务器时，将出现此错误。 将向用户报告如下错误消息：

> SQL Server Management Studio 无法将“\<Server>\<Instance>”配置为“\<Server>\<Instance>”的分发服务器。 错误 18483：无法连接到服务器“\<Server>\<Instance>”，因为“distributor_admin”未在该服务器上定义为远程登录。 请确保指定的登录名正确无误。 %.*ls。

## <a name="cause"></a>原因

从安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的另一台计算机的硬盘映像部署 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，映像计算机的网络名称将保留在新安装中。 不正确的网络名称会导致复制分发服务器配置失败。 如果在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之后重命名计算机，则会出现相同问题。

## <a name="user-action"></a>用户操作

若要解决此问题，请将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务器名称替换为正确的计算机网络名称。 为此，请执行下列步骤：

1. 登录到从磁盘映像部署 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机，然后在 SSMS 中运行以下 Transact-SQL 语句：

    ```sql
    -- Use the Master database
    USE master
    GO

    -- Declare local variables
    DECLARE @serverproperty_servername varchar(100),
    @servername varchar(100);

    -- Get the value returned by the SERVERPROPERTY system function
    SELECT @serverproperty_servername = CONVERT(varchar(100), SERVERPROPERTY('ServerName'));

    -- Get the value returned by @@SERVERNAME global variable
    SELECT @servername = CONVERT(varchar(100), @@SERVERNAME);

    -- Drop the server with incorrect name
    EXEC sp_dropserver @server=@servername;

    -- Add the correct server as a local server
    EXEC sp_addserver @server=@serverproperty_servername, @local='local';
    ```

2. 重新启动运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机。
3. 若要验证计算机的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 名称和网络名称是否相同，请运行以下 Transact-SQL 语句：

    ```sql
    SELECT @@SERVERNAME, SERVERPROPERTY('ServerName');
    ```

## <a name="more-information"></a>详细信息

可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中使用 `@@SERVERNAME` 全局变量或 `SERVERPROPERTY`（“ServerName”）函数来查找运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的网络名称。 当你重新启动计算机和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务时，`SERVERPROPERTY` 函数的 ServerName 属性会自动报告计算机的网络名称更改。 `@@SERVERNAME` 全局变量将保留原始 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机名称，直到手动重置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 名称。

### <a name="steps-to-reproduce-the-problem"></a>重现此问题的步骤

在从磁盘映像部署 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上，执行以下步骤：

1. 启动 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]。
2. 在“对象资源管理器”中，展开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例名称。
3. 右键单击“复制”文件夹，然后依次单击“配置分发复制”、“配置发布、订阅服务器和分发”。
4. 在“配置分发向导”对话框中，单击“下一步”。
5. 在“分发服务器”页上，单击选择“\<**Server**>\<**Instance**>”将充当自己的分发服务器；SQL Server 将创建分发数据库和单选按钮，然后单击“下一步”。
6. 在“SQL Server 代理启动”对话框中，单击“下一步”。
7. 在“快照文件夹”对话框中，单击“下一步”。

    > [!NOTE]
    > 如果收到确认快照文件夹路径的消息，请单击“是”。
8. 在“分发数据库”对话框中，单击“下一步。
9. 在“发布服务器”对话框中，单击“下一步”。
10. 在“向导操作”对话框中，单击“下一步”。
11. 在“完成向导”对话框中，单击“完成”。

## <a name="see-also"></a>另请参阅

- [重命名托管 SQL Server 独立实例的计算机](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)
- [@@SERVERNAME (Transact-SQL)](../../t-sql/functions/servername-transact-sql.md)
- [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)
- [sp_addserver (Transact-SQL)](../system-stored-procedures/sp-addserver-transact-sql.md)