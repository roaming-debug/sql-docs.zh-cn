---
title: 配置快照文件夹共享
titleSuffix: SQL Server on Linux
description: 了解如何在 Linux 上配置快照文件夹共享 SQL Server 复制。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 1e0ace10e1a976128146b82d77660d1edc06040a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471488"
---
# <a name="configure-replication-snapshot-folder-with-shares"></a>使用共享配置复制快照文件夹

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

快照文件夹是指定为共享的目录；对此文件夹执行读写操作的代理必须对其具有足够的访问权限。

![复制关系图][1]

### <a name="replication-snapshot-folder-share-explained"></a>复制快照文件夹共享介绍

在提供示例前，让我们看看 SQL Server 如何在复制中使用 samba 共享。 以下是其工作原理的基本示例。

1. Samba 共享配置为订阅服务器可以看到发布服务器上的复制代理写入 `/local/path1` 的文件
2. SQL Server 配置为在分发服务器上设置发布服务器时使用共享路径，以便所有实例都查看 `//share/path`
3. SQL Server 从 `//share/path` 中查找本地路径，以了解在何处查找文件
4. SQL Server 对 samba 共享支持的本地路径进行读取/写入操作


## <a name="configure-a-samba-share-for-the-snapshot-folder"></a>为快照文件夹配置 samba 共享 

复制代理需要通过复制主机之间的共享目录访问其他计算机上的快照文件夹。 例如，在事务性请求复制中，分发代理驻留在订阅服务器上，这需要访问分发服务器才能获取项目。 在本部分中，我们将介绍如何在两个复制主机上配置 samba 共享的示例。


## <a name="steps"></a>步骤

例如，我们将在主机 1（分发服务器）上使用 Samba 配置要与主机 2（订阅服务器）共享的快照文件夹。 

### <a name="install-and-start-samba-on-both-machines"></a>在两台计算机上安装并启动 Samba 

在 Ubuntu 上：

```bash
sudo apt-get -y install samba
sudo service smbd restart
```

在 RHEL 上：

```bash
sudo yum install samba
sudo service smb start
sudo service smb status
```

### <a name="on-host-1-distributor-set-up-the-samba-share"></a>在主机 1（分发服务器）上设置 Samba 共享 

1. 为 samba 设置用户和密码：

  ```bash
  sudo smbpasswd -a mssql 
  ```

1. 编辑 `/etc/samba/smb.conf` 以包含以下条目，并填写 *share_name* 和 *path* 字段
 ```bash
  <[share_name]>
  path = </local/path/on/host/1>
  writable = yes
  create mask = 770
  directory mask 
  valid users = mssql 
  ```

  **示例**

  ```bash
  [mssql_data]    <- Name of the shared directory
  path = /var/opt/mssql/repldata <- location of directory we wish to share
  writable = yes  <- determines if the share is writable from other hosts
  create mask = 770  <- Linux permissions for files created 
  directory mask = 770 <- Linux permissions for directories created
  valid users = mssql   <- list of users who can login to this share
  ```

### <a name="on-host-2-subscriber--mount-the-samba-share"></a>在主机 2（订阅服务器）上装载 Samba 共享

使用正确的路径编辑命令并在 machine2 上运行以下命令：

  ```bash
  sudo mount //<name_of_host_1>/<share_name> </local/path/on/host/2> -o user=mssql,uid=mssql,gid=mssql
  ```

  **示例**

  ```bash
  mount //host1/mssql_data /var/opt/mssql/repldata_shared -o user=mssql,uid=mssql,gid=mssql

  user=mssql <- sets the login name for samba
  uid=mssql   <- makes the mssql user as the owner of the mounted directory
  gid=mssql   <- sets the mssql group as the owner of the mounted directory
  ```

### <a name="on-both-hosts--configure-sql-server-on-linux-instances-to-use-snapshot-share"></a>在两台主机上将 Linux 上的 SQL Server 实例配置为使用快照共享

将以下部分添加到两台计算机上的 `mssql.conf`。 在所有位置将 samba 共享用于 //share/path。 在本示例中，它将为 `//host1/mssql_data`

  ```bash
  [uncmapping]
  //share/path = /local/path/on/hosts/
  ```

  **示例**

  在 host1 上：

  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/1
  ```

  在 host2 上：
  
  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/2
  ```

### <a name="configuring-publisher-with-shared-paths"></a>使用共享路径配置发布服务器

* 设置复制时，请使用共享路径（示例 `//host1/mssql_data`
* 将 `//host1/mssql_data` 映射到本地目录，并将映射添加到 `mssql.conf`。

## <a name="next-steps"></a>后续步骤

[概念：Linux 上的 SQL Server 复制](sql-server-linux-replication.md)

[复制存储过程](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)。

[1]: ./media/sql-server-linux-replication-snapshot-shares/image1.png
