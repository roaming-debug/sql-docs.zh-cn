---
title: SQL Server 大数据群集 HDFS 加密区域使用指南
titleSuffix: SQL Server big data clusters
description: 本文介绍了如何使用 BDC 的 SQL Server HDFS 加密区域功能
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4c56d065de396a282c97c0723f118e5911617544
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489291"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>SQL Server 大数据群集 HDFS 加密区域使用指南

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本指南演示了如何使用 SQL Server 大数据群集的静态加密功能来使用加密区域对 HDFS 文件夹进行加密。 它还介绍了 HDFS 密钥管理任务。

请注意，在 ```/securelake``` 处已装载一个默认加密区域，可供使用。 它是使用系统生成的256 位密钥 securelakekey 创建的。 此密钥可用于创建其他加密区域。

## <a name="prerequisites"></a><a id="prereqs"></a>先决条件

- 集成了 [Active Directory](active-directory-prerequisites.md) 的 [SQL Server 大数据群集 CU8 及更高版本](release-notes-big-data-cluster.md)。
- 拥有管理权限的用户。
- 在 AD 模式下配置并记录到群集中的 [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)]。

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>使用所提供的系统管理的密钥创建加密区域

1. 创建 HDFS 文件夹

   ```console
   azdata bdc hdfs mkdir -p /user/zone/folder
   ```

1. 发出加密区域创建命令，以使用 securelakekey 密钥来加密文件夹。

   ```console
   azdata bdc hdfs encryption-zone create --path /user/zone/folder --keyname securelakekey
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>创建自定义新密钥和加密区域

1. 使用以下模式来创建 256 位密钥。

   ```console
   azdata bdc hdfs key create -n mydatalakekey
   ```

1. 使用用户密钥创建和加密新的 HDFS 路径。

   ```console
   azdata bdc hdfs encryption-zone create --path /user/mydatalake --keyname mydatalakekey
   ```

## <a name="hdfs-key-rotation-and-encryption-zone-re-encryption"></a>HDFS 密钥轮换和加密区域重新加密

1. 这会创建新版本的 securelakekey，其中包含新的密钥材料。

   ```console
   azdata hdfs bdc key roll -n securelakekey
   ```

1. 重新加密与上述密钥关联的加密区域

   ```console
   azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
   ```

## <a name="hdfs-key-and-encryption-zone-monitoring"></a>HDFS 密钥和加密区域监视

1. 为了监视加密区域重新加密的状态 

   ```console
   azdata bdc hdfs encryption-zone status
   ```

1. 为了获取有关加密区域中的文件的加密信息

   ```console
   azdata bdc hdfs encryption-zone get-file-encryption-info --path /securelake/data.csv
   ```

1. 列出所有加密区域

   ```console
   azdata bdc hdfs encryption-zone list
   ```

1. 为了列出 HDFS 的所有可用密钥

   ```console
   azdata bdc hdfs key list
   ```

1. 为了创建 HDFS 加密的自定义密钥。 可能的大小为 128、192、256。 默认值为 256

   ```console
   azdata hdfs key create --name key1 --size 256
   ```

## <a name="next-steps"></a>后续步骤

有关在大数据群集中使用 azdata 的信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)

将 azdata 用于[已启用 Azure Arc 的数据服务](/azure/azure-arc/data/)
