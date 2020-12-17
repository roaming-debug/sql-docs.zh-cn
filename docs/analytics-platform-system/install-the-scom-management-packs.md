---
title: 安装 SCOM 管理包
description: 按照以下步骤下载并安装适用于 SQL Server PDW 的 System Center Operations Manager (SCOM) 管理包。 从 SCOM 监视 SQL Server PDW 需要管理包。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: d8f4145b85d505ccdf1d0fe26b22f2cdf02d9e90
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641490"
---
# <a name="install-sql-server-operations-manager-scom-management-packs-for-analytics-platform-system"></a>安装 SQL Server Operations Manager (SCOM) 用于分析平台系统的管理包
按照以下步骤下载并安装适用于 SQL Server PDW 的 System Center Operations Manager (SCOM) 管理包。 从 SCOM 监视 SQL Server PDW 需要管理包。  
  
## <a name="before-you-begin"></a><a name="BeforeBegin"></a>开始之前  
**先决条件**  
  
必须安装并运行 System Center Operations Manager。 SQL Server PDW 2012 需要 System Center Operations Manager 2007 R2、System Center Operations Manager 2012 或 System Center Operations Manager 2012 Service Pack 1。  
  
## <a name="step-1-download-the-management-packs"></a><a name="Step1"></a>步骤1：下载管理包  
对于 "AP PDW" 工作负荷，下载 [适用于 Microsoft Analytics Platform System 的 System Center 管理包](https://go.microsoft.com/fwlink/?LinkId=396857)。  
  
对于设备管理，请下载 [SQL Server 设备基本管理包](/previous-versions/system-center/packs/gg602398(v=technet.10))。  
  
对于不带 AP 的较早版本的版本，请下载[适用于 Microsoft SQL Server 2012 并行数据仓库设备的 System Center 监视包](./download-and-apply-microsoft-updates.md?view=aps-pdw-2016-au7&preserve-view=true)。  
  
<!-- MISSING LINKS - For the HDInsight workload, download the [System Center Management Pack for HDInsight](https://go.microsoft.com/fwlink/?LinkId=390208).  -->
  
## <a name="step-2-install-the-management-packs"></a><a name="Step2"></a>步骤2：安装管理包  
  
### <a name="install-the-sql-server-appliance-base-management-pack"></a>安装 SQL Server 设备基本管理包  
  
1.  若要运行安装，请双击下载的 SQL Server 设备基本管理包。  
  
2.  接受许可协议，然后单击 " **下一步**"。  
  
    ![接受许可协议](./media/install-the-scom-management-packs/SCOM_licnse_agrmt.png "SCOM_licnse_agrmt")  
  
3.  选择自己的安装文件夹，或使用默认管理包安装文件夹。  
  
    ![选择安装文件夹](./media/install-the-scom-management-packs/SCOM_licnse_agrmt2.png "SCOM_licnse_agrmt2")  
  
4.  单击“安装”  。  
  
    !["确认安装" 步骤中的 "SQL Server 设备基础监视 MP 安装程序" 向导的屏幕截图，安装选项带有红色圆圈。](./media/install-the-scom-management-packs/SCOM_licnse_agrmt3.png "SCOM_licnse_agrmt3")  
  
5.  单击“关闭”  。  
  
    ![单击 "关闭"](./media/install-the-scom-management-packs/SCOM_licnse_agrmt4.png "SCOM_licnse_agrmt4")  
  
### <a name="install-the-monitoring-pack-for-sql-server-pdw-appliance"></a>为 SQL Server PDW 设备安装监视包  
  
1.  若要运行安装，请双击下载的 SQL Server PDW 设备管理包。  
  
2.  接受许可协议，然后单击 " **下一步**"。  
  
    ![接受许可协议](./media/install-the-scom-management-packs/SCOM_licnse_agmtB.png "SCOM_licnse_agmtB")  
  
3.  选择将包含所提取文件的目录。 默认情况下，将显示默认的管理包安装文件夹。 选择默认设置，或选择自己的安装文件夹。  
  
    ![选择安装文件夹](./media/install-the-scom-management-packs/SCOM_licnse_agmtB1.png "SCOM_licnse_agmtB1")  
  
4.  单击“安装”  。  
  
    ![PDWMP 安装程序向导的屏幕截图，其中安装选项以红色圆圈的 "确认安装" 步骤。](./media/install-the-scom-management-packs/SCOM_licnse_agmtB2.png "SCOM_licnse_agmtB2")  
  
5.  单击“关闭”  。  
  
    ![安装完成](./media/install-the-scom-management-packs/SCOM_licnse_agmtB3.png "SCOM_licnse_agmtB3")  
  
## <a name="next-step"></a>下一步  
安装管理包后，请继续执行下一步： [导入用于 PDW 的 SCOM 管理包 &#40;分析平台系统&#41;](import-the-scom-management-pack-for-pdw.md)。  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->