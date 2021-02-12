---
description: Microsoft Driver for PHP for SQL Server 支持高可用性和灾难恢复
title: Microsoft Drivers for PHP for SQL Server 支持高可用性和灾难恢复 | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 73a80821-d345-4fea-b076-f4aabeb4af3e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a5c5747dad97cc54859cb9b40b966cec66970fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058384"
---
# <a name="support-for-high-availability-disaster-recovery"></a>支持高可用性、灾难恢复
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

本主题介绍了 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 支持高可用性和灾难恢复（版本 3.0 中新增支持）。

自 Microsoft Drivers for PHP for SQL Server 版本 3.0 起，可以在连接字符串中将高可用性、灾难恢复可用性组或故障转移群集实例的可用性组侦听程序指定为服务器。

MultiSubnetFailover 连接属性指明，应用程序正部署在可用性组或故障转移群集实例中，且驱动程序会通过尝试连接到所有 IP 地址来尝试连接到主 SQL Server 实例中的数据库。 在连接到 SQL Server 可用性组侦听程序或 SQL Server 故障转移群集实例时，应始终指定 MultiSubnetFailover=True。 如果将应用程序连接到具有故障转移功能的 AlwaysOn 数据库，那么在故障转移后，原始连接会断开，且应用程序必须建立新连接才能继续运行。

有关 AlwaysOn 可用性组的完整详细信息，可以访问[高可用性、灾难恢复](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)文档页。

## <a name="transparent-network-ip-resolution-tnir"></a>透明网络 IP 解析 (TNIR)

透明网络 IP 解析 (TNIR) 是对现有 MultiSubnetFailover 功能的修订。 如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，就会影响驱动程序的连接序列。 相应的连接选项是 TransparentNetworkIPResolution。 它与 MultiSubnetFailover 一起提供以下四个连接序列： 

- TNIR 已启用且 MultiSubnetFailover 已禁用：先尝试一个 IP，再并行尝试所有 IP
- TNIR 已启用且 MultiSubnetFailover 已启用：并行尝试所有 IP
- TNIR 已禁用且 MultiSubnetFailover 已禁用：逐一尝试所有 IP
- TNIR 已禁用且 MultiSubnetFailover 已启用：并行尝试所有 IP

TNIR 默认处于启用状态，MultiSubnetFailover 默认处于禁用状态。

下面的示例展示了如何使用 PDO_SQLSRV 驱动程序同时启用 TNIR 和 MultiSubnetFailover：

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$connectionString = "sqlsrv:Server=$serverName; TransparentNetworkIPResolution=Enabled; MultiSubnetFailover=yes";
try {
    $conn = new PDO($connectionString, $username, $password, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));
    // your code 
    // more of your code
    // when done, close the connection
    unset($conn);
} catch(PDOException $e) {
    print_r($e->errorInfo);
}
?>
```

## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>升级以便使用来自数据库镜像的多子网群集  
如果连接字符串中已存在 **MultiSubnetFailover** 和 **Failover_Partner** 连接关键字，将出现连接错误。 如果使用的是 MultiSubnetFailover，且 SQL Server 返回指明它属于数据库镜像对的故障转移伙伴响应，也会导致错误抛出。  
  
如果将当前使用数据库镜像的 PHP 应用程序升级到多子网方案，请删除 Failover_Partner 连接属性，并使用设置为“True”的 MultiSubnetFailover 替换它，同时还使用可用性组侦听程序替换连接字符串中的服务器名称。 如果连接字符串使用 Failover_Partner 和 MultiSubnetFailover=true，驱动程序将生成一个错误 。 但是，如果连接字符串使用 Failover_Partner 和 MultiSubnetFailover=false（或 ApplicationIntent=ReadWrite），则该应用程序将使用数据库镜像  。  
  
如果数据库镜像用于 AG 中的主数据库，并且 MultiSubnetFailover=true 用于连接到主数据库（而非连接到可用性组侦听程序）的连接字符串中，则驱动程序将返回错误。  

[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="see-also"></a>另请参阅  
[连接到服务器](../../connect/php/connecting-to-the-server.md)  
