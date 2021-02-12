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
ms.openlocfilehash: bfcc348595a645eca3b1150ae39e7c777f4e192a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046757"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>SQL Server 大数据群集 HDFS 加密区域使用指南

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本指南演示了如何使用 SQL Server 大数据群集的静态加密功能来使用加密区域对 HDFS 文件夹进行加密。

请注意，在 ```/securelake``` 处已装载了一个默认加密区域，可供使用。 它是使用系统生成的256 位密钥 securelakekey 创建的。 此密钥可用于创建其他加密区域。

## <a name="prerequisites"></a><a id="prereqs"></a>先决条件

- 集成了 Active Directory 的 [SQL Server 大数据群集 CU8 及更高版本](release-notes-big-data-cluster.md)。
- 拥有管理权限的用户。

## <a name="login-into-the-name-node"></a>登录名称节点

按照 [Active Directory 连接说明](active-directory-connect.md)操作来执行群集登录。 登录 namenode (nmnode-0-0)，以发出密钥和加密区域命令。

   ```console
   kubectl exec -it -c hadoop -n <cluster_namespace> nmnode-0-0 -- /bin/bash
   ```

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>使用所提供的系统管理的密钥创建加密区域

1. 创建 HDFS 文件夹

   ```console
   hdfs dfs -mkdir -p /user/zone/folder
   ```

1. 发出加密区域创建命令，以使用 securelakekey 密钥来加密文件夹。

   ```console
   hdfs crypto -createZone -keyName securelakekey -path /user/zone/folder
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>创建自定义新密钥和加密区域

1. 使用以下模式来创建 256 位密钥。

   ```console
   kinit hdfs
   hadoop key create mydatalakekey -size 256
   ```

1. 使用用户密钥创建和加密新的 HDFS 路径。

   ```console
   hdfs dfs -mkdir -p /user/mydatalake
   hdfs crypto -createZone -keyName mydatalakekey -path /user/mydatalake
   ```
