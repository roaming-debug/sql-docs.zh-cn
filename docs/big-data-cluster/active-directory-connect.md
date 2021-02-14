---
title: 在 Active Directory 模式下连接
titleSuffix: SQL Server Big Data Cluster
description: 了解如何在 Active Directory 域中连接到 SQL Server 大数据群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 809a91c59973bee1602f335ed37fbef5931bbfeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039907"
---
# <a name="connect-big-data-clusters-2019-active-directory-mode"></a>连接 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]：Active Directory 模式

本文介绍如何在 Active Directory 模式下连接到已部署的 SQL Server 大数据群集终结点。 本文中的任务要求在 Active Directory 模式下完成 SQL Server 大数据群集部署。 如果没有群集，请参阅[在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deploy.md)。

## <a name="overview"></a>概述

通过 AD 身份验证登录 SQL Server 主实例。

要验证与 SQL Server 实例的 AD 连接，请使用 `sqlcmd` 连接到 SQL 主实例。 部署时，会自动为提供的组创建登录名（`clusterUsers` 和 `clusterAdmins`）。

如果使用的是 Linux，请先以 AD 用户身份运行 `kinit`，然后运行 `sqlcmd`。 如果使用的是 Windows，则只需以所需用户身份从已加入域的客户端计算机登录。

## <a name="connect-to-master-instance-from-linuxmac"></a>从 Linux/Mac 连接到主实例

```bash
kinit <username>@<domain name>
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="connect-to-master-instance-from-windows"></a>从 Windows 连接到主实例

```cmd
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="log-in-to-sql-server-master-instance-using-azure-data-studio-or-ssms"></a>使用 Azure Data Studio 或 SSMS 登录到 SQL Server 主实例

从已加入域的客户端，可以打开 SSMS 或 Azure Data Studio，并连接到主实例。 这与使用 AD 身份验证连接到任何 SQL Server 实例的体验相同。

从 SSMS：

![SSMS 中的“连接到 SQL Server”对话框](./media/deploy-active-directory/image23.png)

从 Azure Data Studio：

![Azure Data Studio 中的“连接到 SQL Server”对话框](./media/deploy-active-directory/image24.png)}

## <a name="log-in-to-controller-with-ad-authentication"></a>通过 AD 身份验证登录到控制器

### <a name="connect-to-controller-with-ad-authentication-from-linuxmac"></a>通过 AD 身份验证从 Linux/Mac 连接到控制器

使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 和 AD 身份验证连接到控制器终结点有两个选择。 可以使用 --endpoint/-e 参数：

```bash
kinit <username>@<domain name>
azdata login -e https://<controller DNS name>:30080 --auth ad
```

或者，可以使用 --namespace/-n 参数（大数据群集名称）进行连接：

```bash
kinit <username>@<domain name>
azdata login -n <clusterName> --auth ad
```

### <a name="connect-to-controller-with-ad-authentication-from-windows"></a>通过 AD 身份验证从 Windows 连接到控制器

```cmd
azdata login -e https://<controller DNS name>:30080 --auth ad
```

## <a name="use-ad-authentication-to-knox-gateway-webhdfs"></a>对 Knox 网关使用 AD 身份验证 (webHDFS)

还可以通过 Knox 网关终结点使用 curl 发出 HDFS 命令。 这需要对 Knox 进行 AD 身份验证。 以下 curl 命令通过 Knox 网关发出 webHDFS REST 调用，以创建名为 `products` 的目录

```bash
curl -k -v --negotiate -u : https://<Gateway DNS name>:30443/gateway/default/webhdfs/v1/products?op=MKDIRS -X PUT
```

## <a name="next-steps"></a>后续步骤

[对 SQL Server 大数据群集 Active Directory 集成进行故障排除](troubleshoot-active-directory.md)

[概念：在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-background.md)
