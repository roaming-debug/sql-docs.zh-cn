---
title: 保护 SQL Server Docker 容器
description: 了解保护 SQL Server Docker 容器的不同方法，以及如何在主机上以不同的非根用户身份运行容器
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperfq1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 6e4aa3285f8f74dc9eaa46c52c64ee281f839edf
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489807"
---
# <a name="secure-sql-server-docker-containers"></a>保护 SQL Server Docker 容器

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

默认情况下，SQL Server 2017 容器以根用户身份启动。 这可能会导致一些安全问题。 本文介绍了在运行 SQL Server Docker 容器时具有的安全选项，以及如何以非根用户身份生成 SQL Server 容器。

## <a name="build-and-run-non-root-sql-server-2017-containers"></a><a id="buildnonrootcontainer"></a> 生成并运行非根 SQL Server 2017 容器

按照以下步骤生成以 `mssql`（非根）用户身份启动的 SQL Server 2017 容器。

> [!NOTE]
> SQL Server 2019 容器自动以非根身份启动，因此以下步骤仅适用于 SQL Server 2017 容器，默认情况下，该容器以根身份启动。

1. 下载[适用于非根 SQL Server 容器的示例dockerfile](https://raw.githubusercontent.com/microsoft/mssql-docker/master/linux/preview/examples/mssql-server-linux-non-root/Dockerfile)，并将其另存为 `dockerfile`。

2. 在 dockerfile 目录的上下文中运行以下命令，生成非根 SQL Server 容器：

    ```bash
    cd <path to dockerfile>
    docker build -t 2017-latest-non-root .
    ```

3. 启动容器。

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword@" --cap-add SYS_PTRACE --name sql1 -p 1433:1433 -d 2017-latest-non-root
    ```

    > [!NOTE]
    > 非根 SQL Server 容器需要 `--cap-add SYS_PTRACE` 标志来生成用于排除故障的转储。

4. 检查是否以非根用户的身份运行容器：

    ```bash
    docker exec -it sql1 bash
    ```

    运行 `whoami`，这将返回容器中运行的用户。
    
    ```bash
    whoami
    ```

## <a name="run-container-as-a-different-non-root-user-on-the-host"></a><a id="nonrootuser"></a>在主机上以其他非根用户的身份运行容器

若要以其他非根用户的身份运行 SQL Server 容器，请将 -u 标志添加到 docker run 命令。 非根容器具有以下限制：必须作为根组的一部分运行，除非已将卷装载到非根用户可以访问的 `/var/opt/mssql`。 根组不向非根用户授予任何额外的根权限。

#### <a name="run-as-a-user-with-a-uid-4000"></a>以具有 UID 4000 的用户身份运行

可使用自定义 UID 启动 SQL Server。 例如，以下命令使用 UID 4000 启动 SQL Server：

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u 4000:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

> [!Warning]
> 确保 SQL Server 容器具有命名用户，如“mssql”或“root”，否则 SQLCMD 将无法在容器中运行。 可通过在容器中运行 `whoami` 来检查是否以命名用户的身份运行 SQL Server 容器。

#### <a name="run-the-non-root-container-as-the-root-user"></a>以根用户的身份运行非根容器

如果需要，可以根用户的身份运行非根容器。 这还会将所有文件权限自动授予容器，因为容器具有更高的特权。

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -u 0:0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-user-on-your-host-machine"></a>在主机计算机上以用户的身份运行

可使用以下命令在主机计算机上以现有用户身份启动 SQL Server：
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u $(id -u myusername):0 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

#### <a name="run-as-a-different-user-and-group"></a>以其他用户和组的身份运行

可使用自定义用户和组启动 SQL Server。 在此示例中，已装载的卷具有为主机计算机上的用户或组配置的权限。

```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" --cap-add SYS_PTRACE -u (id -u myusername):(id -g myusername) -v /path/to/mssql:/var/opt/mssql -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

## <a name="configure-persistent-storage-permissions-for-non-root-containers"></a><a id="storagepermissions"></a>为非根容器配置持久存储权限

若要允许非根用户访问已装载卷上的数据库文件，请确保以可读取/写入持久文件存储的用户身份或组成员身份运行容器。  

可通过此命令获取数据库文件的当前所有权。

```bash
ls -ll <database file dir>
```

如果 SQL Server 无权访问持久数据库文件，请运行以下命令之一。

#### <a name="grant-the-root-group-rw-access-to-the-db-files"></a>授予对 DB 文件的根组读/写访问权限

授予对以下目录的根组权限，使非根 SQL Server 容器有权访问数据库文件。

```bash
chgrp -R 0 <database file dir>
chmod -R g=u <database file dir>
```

#### <a name="set-the-non-root-user-as-the-owner-of-the-files"></a>将非根用户设置为文件的所有者

这可以是默认的非根用户，也可以是要指定的任何其他非根用户。 在此示例中，我们将 UID 10001 设置为非根用户。

```bash
chown -R 10001:0 <database file dir>
```
## <a name="encrypting-connections-to-sql-server-linux-containers"></a>加密与 SQL Server Linux 容器的连接

要加密与 SQL Server Linux 容器的连接，需要一个证书，有关该证书的要求，请参阅[此处]。

以下示例演示如何加密与 SQL Server Linux 容器的连接。 在示例中，我们使用自签名证书，但在此类环境的生产场景中则不应使用此证书，而应使用 CA 证书。

1. 创建自签名证书，该证书仅适用于测试和非生产环境。
  
      ```bash
      openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=sql1.contoso.com' -keyout /container/sql1/mssql.key -out /container/sql1/mssql.pem -days 365
      ```
     其中，sql1 是 SQL 容器的主机名，因此，连接到此容器时，连接字符串中使用的名称将为“sql1.contoso.com,port”。

    > [!NOTE]
    > 运行上述命令之前，请确保文件夹路径 /container/sql1/ 已存在。

2. 确保为 mssql.key 和 mssql.pem 文件设置正确的权限，以避免在将文件装载到 SQL 容器时出现错误：

    ```bash
    chmod 440 /container/sql1/mssql.pem
    chmod 440 /container/sql1/mssql.key
    ```

3. 现在，创建包含以下内容的 mssql.conf 文件以启用由服务器启动的加密；要启用由客户端启动的加密，请将最后一行更改为“forceencryption = 0”。

    ```bash
    [network]
    tlscert = /etc/ssl/certs/mssql.pem
    tlskey = /etc/ssl/private/mssql.key
    tlsprotocols = 1.2
    forceencryption = 1
    ```

    > [!NOTE]
    > 对于某些 Linux 分发版，存储证书和密钥的路径还可以分别为 /etc/pki/tls/certs/ 和 /etc/pki/tls/private/。 更新 SQL 容器的 mssql.conf 之前，请先验证路径。 SQL Server 将根据你在 mssql.conf 中设置的位置在容器中搜索证书及其密钥。 在本例中，该位置为 /etc/ssl/certs/ 和 /etc/ssl/private/。

    还会在同一文件夹位置 /container/sql1/ 下创建 mssql.conf 文件。 运行上述步骤之后，sql1 文件夹中应包含三个文件：mssql.conf、mssql.key 和 mssql.pem。

4. 使用如下所示的命令部署 SQL 容器：

    ```bash
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5434:1433 --name sql1 -h sql1 -v /container/sql1/mssql.conf:/var/opt/mssql/mssql.conf -v   /container/sql1/mssql.pem:/etc/ssl/certs/mssql.pem -v /container/sql1/mssql.key:/etc/ssl/private/mssql.key -d mcr.microsoft.com/mssql/server:2019-latest
    ```

    在上面的命令中，我们已将 mssql.conf、mssql.pem 和 mssql.key 文件装载到容器中，并将容器中的 1433 端口（SQL Server 默认端口）映射到主机的 5434 端口。 

    > [!NOTE]
    > 如果使用 RHEL 8 和更高版本，则还可以使用“podman run”命令来代替“docker run”。 

按照[此处][1]的“在客户端计算机上注册证书”和“示例连接字符串”部分所述，开始加密与 Linux 上 SQL Server 容器的连接。

  [Encrypting connection to SQL Server Linux]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true
  [這裡]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#requirements-for-certificates
  [1]: https://docs.microsoft.com/sql/linux/sql-server-linux-encrypted-connections?view=sql-server-ver15&preserve-view=true#client-initiated-encryption

## <a name="next-steps"></a>后续步骤

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- 通过查看[快速入门](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)，开始在 Docker 上使用 SQL Server 2017 容器映像

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- 通过查看[快速入门](quickstart-install-connect-docker.md?view=sql-server-ver15)，开始在 Docker 上使用 SQL Server 2019 容器映像

::: moniker-end

- [部署并连接到 SQL Server Docker 容器](sql-server-linux-docker-container-deployment.md)

- [引用 Docker 容器的其他配置和自定义](sql-server-linux-docker-container-configure.md)

- [SQL Server Docker 容器故障排除](sql-server-linux-docker-container-troubleshooting.md)
