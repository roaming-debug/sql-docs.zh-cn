---
description: 使用透明网络 IP 解析
title: 使用透明网络 IP 解析 | Microsoft Docs
ms.custom: ''
ms.date: 01/02/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: chris-rossi
ms.author: v-chross
ms.openlocfilehash: 944c51fd392ffb684cf2c2fc02967b040163a9d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351121"
---
# <a name="using-transparent-network-ip-resolution"></a>使用透明网络 IP 解析
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>目的
透明网络 IP 解析 (TNIR) 是对现有 MultiSubnetFailover 功能的修订。 如果第一个解析的主机名 IP 未响应，且存在多个与主机名关联的 IP，TNIR 就会影响驱动程序的连接序列。 TNIR 与 MultiSubnetFailover 交互，以提供下列三个连接序列：<br />
* 0：先尝试一个 IP，再并行尝试所有 IP
* 1：并行尝试所有 IP
* 2:逐一尝试所有 IP

|TransparentNetworkIPResolution|MultiSubnetFailover|行为|
|--------|--------|--------|
|True|True|1|
|正确|False|0|
|False|True|1|
|False|False|2|

## <a name="setting-transparent-network-ip-resolution"></a>设置透明网络 IP 解析
TransparentNetworkIPResolution 默认情况下处于启用状态。 MultiSubnetFailover 默认情况下处于禁用状态。 以下页面提供了有关设置这些属性的详细信息： 
- [将连接字符串关键字与适用于 SQL Server 的 OLE DB 驱动程序结合使用](..\applications\using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [初始化和授权属性](..\ole-db-data-source-objects\initialization-and-authorization-properties.md)

## <a name="see-also"></a>另请参阅 
[适用于 SQL Server 的 OLE DB 驱动程序对高可用性和灾难恢复的支持](./oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)
