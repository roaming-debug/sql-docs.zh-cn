---
author: MikeRayMSFT
ms.prod: sql
ms.technology: big-data-cluster
ms.topic: include
ms.date: 06/22/2020
ms.author: mikeray
ms.openlocfilehash: 599b4072c5d03c8a4753c7c00bc7edc14e0f3b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038919"
---
从 SQL Server 2019 CU5 开始，当使用基本身份验证部署新群集时，所有终结点（包括网关）都使用 `AZDATA_USERNAME` 和 `AZDATA_PASSWORD`。 升级到 CU5 的群集上的终结点继续使用 `root` 作为用户名连接到网关终结点。 此更改不适用于使用 Active Directory 身份验证的部署。 请参阅发行说明中的[通过网关终结点访问服务所用的凭据](../big-data-cluster/release-notes-big-data-cluster.md#credentials-for-accessing-services-through-gateway-endpoint)。