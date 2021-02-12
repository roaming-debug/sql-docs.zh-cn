---
title: SQL Server 大数据群集的 HDFS 层权限
titleSuffix: Manage HDFS tiering permissions for SQL Server Big Data Clusters
description: 管理 SQL Server 大数据群集上 HDFS 层的安全性，如针对其他基于 Linux 的系统的权限。
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0f791cd64dd177e01dc086dd08f9216bda4a55b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046668"
---
# <a name="manage-hdfs-permissions-for-big-data-clusters-2019"></a>管理 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的 HDFS 权限

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

HDFS 是一个文件系统，类似于基于 Linux 的文件系统，它使用 POSIX 管理文件权限。 除了传统的 POSIX 权限模型外，HDFS 还支持 POSIX 访问控制列表 (ACL)。 有关详细信息，请参阅[有关 ACL 的 Apache Hadoop 文章](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_.28Access_Control_Lists.29)。

下面各部分提供了有关如何使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 来管理 HDFS 文件和目录权限的示例。

## <a name="prerequisites"></a>必备条件

- [部署大数据群集](deployment-guidance.md)
- [大数据工具](deploy-big-data-tools.md)
  
## <a name="hdfs-shell"></a>HDFS shell

使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 中的 `hdfs` shell 功能，可以直接在 shell 中发出命令，以管理文件和目录的 HDFS 权限。 基础机制使用 WebHdfs 调用来发出命令

以下命令将打开 shell。

```bash
azdata bdc hdfs shell
```

要获取有关 `hdfs` shell 的帮助并了解如何发出命令，请在 shell 处于活动状态的情况下运行以下命令。

```bash
[hdfs] ?
```

下面的示例演示如何创建目录、列出目录以及修改目录权限，并为指定用户 `bob` 提供对目录 `sales` 的读取、写入和执行权限。

```bash
[hdfs] mkdir sales
[hdfs] ls
rwxr-xr-x  hdfs bdcadmins        0 Oct 09 18:02 system/
rwxrwxr-x admin bdcadmins        0 Oct 10 16:47 sales/
--xrwxrwxrwx  hdfs bdcadmins        0 Oct 09 18:03 tmp/
rwxrwxrwx  hdfs bdcadmins        0 Oct 09 17:59 user/

[hdfs] acl modify  '/sales/' 'user:bob:rwx'
acl modify: Change completed.
[hdfs] acl status  '/sales/'
{
  `AclStatus`: {
    `entries`: [
      `user:bob:rwx`,
      `group::r-x`
    ],
    `group`: `bdcadmins`,
    `owner`: `admin`,
    `permission`: `775`,
    `stickyBit`: false
  }
}
```

## <a name="create-a-directory-in-hdfs-using-azure-data-cli-azdata"></a>使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 在 HDFS 中创建目录

在路径 `/sales` 中创建一个名为 `data` 的目录。

```bash
azdata bdc hdfs mkdir --path '/sales/data'
```

## <a name="change-owner-of-a-directory-or-file"></a>更改目录或文件的所有者

在 HDFS 中更改目录 `data` 的所有者用户，并将 `alice` 设置为所有者用户，将 `salesgroup` 设置为所有者组。 必须具有所有者身份才可更改所有者。

```bash
azdata bdc hdfs chown --owner alice --group 'salesgroup' --path '/sales/data'
```

## <a name="change-permissions-of-a-file-or-directory-with-chmod"></a>使用 `chmod` 更改文件或目录的权限

使用 `chmod` 更改文件和目录的权限（适用于所有者、所有者组和其他）。 有关详细信息，请参阅[更改 Linux 文件系统的权限](https://www.lifewire.com/uses-of-command-chmod-2201064)。 在 HDFS 中，模式是相同的。 例如：

```bash
azdata bdc hdfs chmod --permission 750 --path /sales/data
```

```bash
azdata bdc hdfs chmod --permission 775 --path /sales/data/file.txt
```

## <a name="set-sticky-bit-on-directories"></a>对目录设置粘滞位

对目录设置粘滞位可防止意外删除或重定位文件。 粘滞位限制了删除文件或将文件移动到超级用户、目录所有者或文件所有者的权限。 此设置不会影响文件。 下面的示例通过为权限添加 `1` 前缀来对目录 `users` 设置粘滞位。

```bash
azdata bdc hdfs chmod --path /sales/users --permission 1750
```

## <a name="setting-acls-on-files-and-directories"></a>对文件和目录设置 ACL

要在 HDFS 中对文件和目录设置 ACL，请使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 命令。

对目录设置 ACL，并为指定用户 `tom` 提供对目录 `data` 的读取、写入和执行权限。 

> [!NOTE]
> 使用 `set` 命令时，请确保提供完整的 ACL 规范，包括适用于所有者用户、所有者组和其他用户的 ACL 规范。

```bash
azdata bdc hdfs acl set --path '/sales' --aclspec  'user::rw-,user:tom:rwx,group::rw-,other::rw-'
```

## <a name="default-acl-on-directories"></a>目录上的默认 ACL

默认 ACL 允许子目录从父目录继承权限。 仅目录可具有默认 ACL。 创建新文件或子目录后，其自身的访问 ACL 会继承父目录的默认 ACL。 通过这种方式，在创建新的子目录时，默认 ACL 将通过任意深层目录级别向下继承。

下面的示例展示了如何使用 azdata 设置默认 ACL。

```bash
azdata bdc hdfs acl set --path '/sale' --aclspec  'user::rw-,user:tom:rwx,group::rw-,other::rw-,default:group::rw-,default:user::rw-,default:other::rw-'
```

## <a name="next-steps"></a>后续步骤

- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 引用](../azdata/reference/reference-azdata.md)

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)