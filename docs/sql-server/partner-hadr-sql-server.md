---
title: SQL Server 高可用性和灾难恢复合作伙伴
description: 提供监视 SQL Server 解决方案的第三方合作伙伴列表。
ms.topic: conceptual
ms.custom: seo-dt-2019
ms.date: 09/17/2017
ms.prod: sql
ms.technology: release-landing
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: dd40556746f729d0a64b9eebf6550c774d267126
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235614"
---
# <a name="sql-server-high-availability-and-disaster-recovery-partners"></a>SQL Server 高可用性和灾难恢复合作伙伴
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
要对 SQL Server 服务提供高可用性和灾难恢复，请从各种行业领先工具进行选择。  本文重点介绍提供支持 Microsoft SQL Server 的高可用性和灾难恢复解决方案的 Microsoft 合作伙伴公司。

## <a name="high-availability-and-disaster-recovery-partners"></a>高可用性和灾难恢复合作伙伴

| Partner | 说明 | 链接 | 
| --- | --- | --- |
|![Azure][5] |**Azure Site Recovery**<br>Site Recovery 可复制在虚拟机或物理服务器上运行的工作负荷，确保它们在主站点不可用时在辅助位置仍然可用。 你可以将 SQL Server 虚拟机从本地数据中心复制和故障转移到 Azure 或另一个本地数据中心，也可以从一个 Azure 数据中心复制和故障转移到另一个 Azure 数据中心。<br><br> Enterprise 和 Standard 版 SQL Server 2008 R2 - SQL Server 2016|[网站][azure_website]<br>[市场][azure_marketplace]<br>[数据表][azure_datasheet]<br>[Twitter][azure_twitter]<br>[视频][azure_youtube]|
|![DH2i][2] |**DH2i**<br>DxEnterprise 是适用于 Windows、Linux 和 Docker 的智能可用性软件，可帮助你实现接近零的计划和非计划故障时间、节约大量成本、极大地简化管理以及实现物理和逻辑合并。<br><br>SQL Server 2005+、Windows Server 2008R2+、Ubuntu 16+、RHEL 7+、CentOS 7+|[网站][dh2i_website]<br>[数据表][dh2i_datasheet]<br>[Twitter][dh2i_twitter]<br>[视频][dh2i_youtube]|
|![HPE 徽标][4] |**HPE Serviceguard**<br>通过 HPE Serviceguard for Linux (SGLX)，可防止 Linux &reg; 上的关键 SQL Server 2017 工作负载因跨物理和虚拟环境的众多基础结构和应用程序故障（无论距离多远）而出现意外和计划停机。 HPE SGLX A.12.20.00 以及更高版本为故障转移群集实例和 Always On 可用性组 SQL Server 工作负荷提供上下文敏感监视和恢复选项。 使用 HPE SGLX，可最大化运行时间，且不会影响数据完整性和性能。<br><br>Linux 上的 SQL Server 2017 - RedHat 7.3、7.4、SUSE 12 SP2、SP3|[网站][hpe_website]<br>[数据表][hpe]<br>[下载评估][hpe_download]<br>[博客][hpe_download]<br>[Twitter][hpe_twitter]
|![IDERA][3]|**IDERA**<br>SQL 安全备份是一种适用于 SQL Server 的高性能备份和恢复解决方案，它可以缩短数据库备份时间，减小备份文件大小，在备份文件内提供对数据库的实时读取操作，从而节省资金。<br><br>Microsoft SQL Server：2005 SP1 或更高版本、2008、2008 R2、2012、2014、2016；所有版本 |[网站][idera_website]|
|![NEC][7]|**NEC**<br>ExpressCluster 是面向所有主要故障的全面和完全自动化的高可用性灾难恢复解决方案，这些故障包括 SQL Server 的硬件、软件、网络和站点故障，以及在本地或云环境中的物理计算机或虚拟机上运行的相关应用程序的故障。<br><br>Microsoft SQL Server：2005 版或更高版本；所有版本 |[网站][necec_website]<br>[数据表][necec_datasheet]<br>[视频][necec_youtube]<br>[下载][necec_download]|
|![Portworx][6] |**Portworx**<br>Portworx 是适用于在生产中运行的有状态容器的解决方案。 借助 Portworx，用户可以使用任何容器计划程序（包括 Kubernetes、Mesosphere DC/OS 和 Docker Swarm）在任何基础结构上管理任何数据库或有状态服务。 Portworx 解决了 DevOps 团队在生产中运行容器化数据库及其他有状态服务时遇到的五个最常见问题：持久性、高可用性、数据自动化、支持多个数据存储区和基础结构，以及安全性。<br><br>SQL Server 2017 on Docker |[网站][portworx_website]<br>[文档][portworx_docs]<br>[视频][portworx_youtube]|
|![SIOS][8] |SIOS <br>SIOS 技术为 Windows 或 Linux 上的 SQL Server 交付经济高效的高可用性和灾难恢复解决方案。 SIOS SANless 群集消除了使用共享存储 SAN 的必要性，使你能够完全灵活地保护你在单个和多站点环境中的物理、虚拟、云和混合云配置中最为重要的应用程序。<br><br>将 SIOS DataKeeper 添加到你的 Windows Server 故障转移群集环境，以创建替换传统共享存储的 SANless 卷资源，从而便于在 Azure 中运行 WSFC。<br><br>SIOS 保护套件是完全灵活的群集解决方案，它保护关键 Linux 应用程序（例如，SQL Server、SAP、HANA、Oracle 和许多其他应用程序）。|[网站][sios_website]<br>[数据表][sios_datasheet]<br>[Twitter][sios_twitter]<br>[市场][sios_marketplace]<br>[视频][sios_youtube]|
|![Veeam][1] |**Veeam**<br>Veeam 备份和复制是一种功能强大、简单易用、经济实惠的备份和可用性解决方案。 它可以快速、灵活、可靠地恢复虚拟化应用程序和数据，将 VM（虚拟机）备份和复制整合为一种软件解决方案。 Veeam 备份和复制为 VMware vSphere 和 Microsoft Hyper-V 虚拟环境提供一流的支持。<br><br>SQL Server 2005 SP4 - SQL Server 2016 on Windows |[网站][veeam_website]<br>[数据表][veeam_datasheet]<br>[Twitter][veeam_twitter]<br>[视频][veeam_youtube]|

## <a name="next-steps"></a>后续步骤
要深入了解其他合作伙伴，请参阅[监视][mon_partners]、[管理合作伙伴][management_partners]以及[开发合作伙伴][dev_partners]。

<!--Image references-->
[1]: ./media/partner-hadr-sql-server/Veeam-green-logo.png
[2]: ./media/partner-hadr-sql-server/dh2i-logo.png
[3]: ./media/partner-hadr-sql-server/idera-logo.png
[4]: ./media/partner-hadr-sql-server/hpe.png
[5]: ./media/partner-hadr-sql-server/azure-logo.png
[6]: ./media/partner-hadr-sql-server/portworx-logo.png
[7]: ./media/partner-hadr-sql-server/nec-logo.png
[8]: ./media/partner-hadr-sql-server/sios-logo.png

<!--Article links-->
[mon_partners]: ./partner-monitor-sql-server.md
[management_partners]: ./partner-management-sql-server.md
[dev_partners]: ./partner-dev-sql-server.md

<!--Website links -->
[veeam_website]:https://www.veeam.com/
[dh2i_website]:https://dh2i.com
[idera_website]:https://www.idera.com/productssolutions/sqlserver
[hpe_website]: https://www.hpe.com/us/en/product-catalog/detail/pip.376220.html
[azure_website]: /azure/site-recovery/site-recovery-sql
[necec_website]: https://www.necam.com/ExpressCluster/
[portworx_website]: https://portworx.com/
[sios_website]: https://us.sios.com/

<!--Get Started Links-->

<!--Datasheet Links-->
[veeam_datasheet]:https://www.veeam.com/veeam_backup_9_5_datasheet_en_ds.pdf
[dh2i_datasheet]:https://dh2i.com/wp-content/uploads/DxE-Win-QuickFacts.pdf
[hpe]:https://www.hpe.com/h20195/v2/default.aspx?cc=us&lc=en&oid=376220
[necec_datasheet]: https://www.necam.com/docs/?id=0d9ef7a7-f935-4909-b6bb-20a47b3
[azure_datasheet]: /azure/site-recovery/vmware-physical-azure-support-matrix
[sios_datasheet]: https://us.sios.com/solutions/high-availability-cluster-software-cloud/

<!--Marketplace Links -->
[azure_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps?search=site%20recovery&page=1
[sios_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8
<!--Press links-->
<!--[veeam_press]:-->

<!--YouTube links-->
[veeam_youtube]:https://www.youtube.com/user/YouVeeam
[dh2i_youtube]:https://www.youtube.com/user/dh2icompany 
[idera_youtube]:https://www.idera.com/resourcecentral/videos/sql-safe-overview
[azure_youtube]: https://mva.microsoft.com/en-US/training-courses/is-your-lack-of-a-disaster-recovery-site-keeping-you-up-at-night-8680?l=oF7YrFH1_7504984382
[necec_youtube]: https://www.youtube.com/watch?v=9La3Cw1Q1Jk
[portworx_youtube]: https://www.youtube.com/channel/UCSexpvQ9esSRgiS_Q9_3mLQ
[sios_youtube]: https://www.youtube.com/watch?v=U3M44gJNWQE

<!--Twitter links-->
[veeam_twitter]:https://twitter.com/veeam
[dh2i_twitter]:https://twitter.com/dh2i
[hpe_twitter]:https://twitter.com/hpe
[azure_twitter]:https://twitter.com/hashtag/azuresiterecovery
[sios_twitter]:https://www.twitter.com/SIOSTech

<!--Docs links>-->
[portworx_docs]: https://docs.portworx.com/

<!--Download links-->
[hpe_download]: https://h20392.www2.hpe.com/portal/swdepot/displayProductInfo.do?productNumber=SGLX-DEMO
[necec_download]: https://www.necam.com/ExpressCluster/30daytrial/
<!--Blog links-->
[hpe_blog]: https://community.hpe.com/t5/Servers-The-Right-Compute/SQL-Server-for-Linux-Is-Here-and-A-New-Chapter-for-Mission/ba-p/6977571#.WiHWW0xFwUE