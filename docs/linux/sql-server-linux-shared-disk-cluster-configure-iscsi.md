---
title: 配置 iSCSI FCI 存储 - Linux 上的 SQL Server
description: 了解如何使用 iSCSI 为 Linux 上的 SQL Server 配置故障转移群集实例 (FCI)。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: dfd63aa749b6bf158ae5e9dcef06ba032c31ddee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346332"
---
# <a name="configure-failover-cluster-instance---iscsi---sql-server-on-linux"></a>配置故障转移群集实例 - iSCSI - Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Linux 上为故障转移群集实例 (FCI) 配置 iSCSI 存储。 

## <a name="configure-iscsi"></a>配置 iSCSI 
iSCSI 使用网络将名为“目标”的服务器中的磁盘呈现给服务器。 连接到 iSCSI 目标的服务器要求已配置 iSCSI 发起程序。 目标上的磁盘具有显式权限，因此只有能够访问它们的发起程序才能执行此操作。 目标本身应该是高度可用和可靠的。

### <a name="important-iscsi-target-information"></a>重要的 iSCSI 目标信息
虽然本部分不介绍如何配置 iSCSI 目标，因为此过程特定于要使用的源类型，但请确保已配置（群集节点将使用的）磁盘的安全性。  

如果使用基于 Linux 的 iSCSI 目标，则不应在任何 FCI 节点上配置目标。 处于性能和可用性的考虑，应将 iSCSI 网络与源服务器和客户端服务器上常规网络流量所使用的网络分开。 用于 iSCSI 的网络速度应很快。 请记住，网络确实会消耗一些处理器带宽，所以如果使用的是常规服务器，请相应进行规划。
在目标上需要确保的最重要的事是为创建的磁盘分配适当的权限，以便只有那些参与 FCI 的服务器才能访问它们。 下面显示了一个示例 Microsoft iSCSI 目标，其中 linuxnodes1 是创建的名称，在这种情况下，分配了节点的 IP 地址，以便向其显示 NewFCIDisk1.vhdx。

![Initiator][1]

### <a name="instructions"></a>Instructions

本部分介绍如何在用作 FCI 节点的服务器上配置 iSCSI 发起程序。 此说明同样适用于 RHEL 和 Ubuntu。

有关支持的分发的 iSCSI 发起程序的更多信息，请访问以下链接：
- [Red Hat](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/iscsi-api.html)
- [SUSE](https://www.suse.com/documentation/sles11/stor_admin/data/sec_inst_system_iscsi_initiator.html) 
- [Ubuntu](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html)

1.  选择将参与 FCI 配置的其中一个服务器。 选择任何一个均可。 iSCSI 应位于专用网络上，因此请配置 iSCSI 以识别和使用该网络。 运行 `sudo iscsiadm -m iface -I <iSCSIIfaceName> -o new`，其中 `<iSCSIIfaceName>` 是网络的唯一或友好名称。 下面的示例使用 `iSCSINIC`：
   
    ```bash
    sudo iscsiadm -m iface -I iSCSINIC -o new
    ```
    ![iface 命令及对该命令的响应的屏幕截图。][6]
 
2.  编辑 `/var/lib/iscsi/ifaces/iSCSIIfaceName`。 确保已完整填写以下值：

    - iface.net_ifacename 是操作系统中显示的网卡名称。
    - iface.hwaddress 是将在下面为此接口创建的唯一名称的 MAC 地址。
    - iface.ipaddress
    - iface.subnet_Mask 

    请参阅以下示例：

    ![值完全填写好的文件屏幕截图。][2]

3.  查找 iSCSI 目标。

    ```bash
    sudo iscsiadm -m discovery -t sendtargets -I <iSCSINetName> -p <TargetIPAddress>:<TargetPort>
    ```

     \<iSCSINetName> 是网络的唯一/友好名称，\<TargetIPAddress> 是 iSCSI 目标的 IP 地址，\<TargetPort> 是 iSCSI 目标的端口。 

    ![discovery 命令及对该命令的响应的屏幕截图。][3]

 
4.  登录到目标

    ```bash
    sudo iscsiadm -m node -I <iSCSIIfaceName> -p TargetIPAddress -l
    ```

    \<iSCSIIfaceName> 网络的唯一/友好名称，\<TargetIPAddress> 是 iSCSI 目标的 IP 地址。

    ![iSCSITargetLogin][4]

5.  检查是否存在与 iSCSI 目标的连接。

    ```bash
    sudo iscsiadm -m session
    ```

    ![iSCSIVerify][5]

 
6.  检查 iSCSI 附加的磁盘

    ```bash
    sudo grep "Attached SCSI" /var/log/messages
    ```
    ![grep 命令及对该命令的响应的屏幕截图，其中显示了附加的 SCSI 磁盘。][7]

7.  在 iSCSI 磁盘上创建物理卷。

    ```bash
    sudo pvcreate /dev/<devicename>
    ```

    \<devicename> 是上一步中设备的名称。 

 
8.  在 iSCSI 磁盘上创建卷组。 分配给单个卷组的磁盘被视为池或集合。 

    ```bash
    sudo vgcreate <VolumeGroupName> /dev/devicename
    ```

    \<VolumeGroupName> 是卷组的名称，\<devicename> 是第 6 步中设备的名称。 
 
9.  创建并验证磁盘的逻辑卷。

    ```bash
    sudo lvcreate -Lsize -n <LogicalVolumeName> <VolumeGroupName>
    ```
    
    \<size> 是要创建的卷的大小，可以以 G (GB)、T (TB) 等为单位指定。\<LogicalVolumeName> 是逻辑卷的名称，\<VolumeGroupName> 是上一步中卷组的名称。 

    下面的示例创建了一个 25GB 的卷。
 
    ![Create25GBVol][10]

10. 执行 `sudo lvs` 以查看创建的 LVM。
 
11. 使用支持的文件系统格式化逻辑卷。 对于 EXT4，使用以下示例：

    ```bash
    sudo mkfs.ext4 /dev/<VolumeGroupName>/<LogicalVolumeName>
    ```

    \<VolumeGroupName> 是上一步中卷组的名称。 \<LogicalVolumeName> 是上一步中逻辑卷的名称。  

12. 对于系统数据库或存储在默认数据位置的任何内容，请执行以下步骤。 否则则，请跳至步骤 13。

   * 确保正在使用的服务器上的 SQL Server 已停止运行。

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ```
    
   * 彻底切换为超级用户。 如果成功，不会收到任何确认信息。

        ```bash
        sudo -i
        ```
    
   * 切换为 mssql 用户。 如果成功，不会收到任何确认信息。

        ```bash
        su mssql
        ```
    
   * 创建一个临时目录来存储 SQL Server 数据和日志文件。 如果成功，不会收到任何确认信息。

        ```bash
        mkdir <TempDir>
        ```
    
        \<TempDir> 是文件夹的名称。 以下示例创建名为 /var/opt/mssql/TempDir 的文件夹。

        ```bash
        mkdir /var/opt/mssql/TempDir
        ```
        
   * 将 SQL Server 的数据和日志文件复制到临时目录。 如果成功，不会收到任何确认信息。

        ```bash
        cp /var/opt/mssql/data/* <TempDir>
        ```
    
        \<TempDir> 是上一步中的文件夹的名称。
    
   * 验证文件是否位于目录中。

        ```bash
        ls \<TempDir>
        ```
 
        \<TempDir> 是步骤 d 中文件夹的名称。

   * 删除现有 SQL Server 数据目录中的文件。 如果成功，不会收到任何确认信息。

        ```bash
        rm - f /var/opt/mssql/data/*
        ```
    
   * 验证文件是否已删除。 下图显示了从 c 到 h 的整个序列的示例。

        ```bash
        ls /var/opt/mssql/data
        ```
    
        ![ls 命令及对该命令的响应的屏幕截图。][8]

   * 键入 `exit` 切换回根用户。

   * 将 iSCSI 逻辑卷装入 SQL Server 数据文件夹中。 如果成功，不会收到任何确认信息。

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> /var/opt/mssql/data
        ```

        \<VolumeGroupName> 是卷组的名称，\<LogicalVolumeName> 是已创建的逻辑卷的名称。 以下示例语法与上一个命令中的卷组和逻辑卷匹配。

        ```bash
        mount /dev/FCIDataVG1/FCIDataLV1 /var/opt/mssql/data
        ```

   * 将装载的所有者更改为 mssql。 如果成功，不会收到任何确认信息。

        ```bash
        chown mssql /var/opt/mssql/data
        ```

   * 将装载组的所有权更改为 mssql。 如果成功，不会收到任何确认信息。

        ```bash
        chgrp mssql /var/opt/mssql/data
        ```

   * 切换为 mssql 用户。 如果成功，不会收到任何确认信息。

        ```bash
        su mssql
        ``` 

   * 从临时目录 /var/opt/mssql/data 复制文件。 如果成功，不会收到任何确认信息。

        ```bash
        cp /var/opt/mssql/TempDir/* /var/opt/mssql/data
        ``` 
    
   * 验证文件是否在那里。

        ```bash
        ls /var/opt/mssql/data
        ``` 

   *    输入 `exit` 以退出 mssql 身份。

   *    输入 `exit` 以退出 root 身份。

   *    启动 SQL Server。 如果正确复制了所有内容并正确应用了安全性，SQL Server 应显示为已启动。

        ```bash
        sudo systemctl start mssql-server
        sudo systemctl status mssql-server
        ``` 

   *    停止 SQL Server 并验证它是否已关闭。

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ``` 

13. 对于系统数据库以外的其他内容，例如用户数据库或备份，请按照以下步骤操作。 如果仅使用默认位置，请跳至步骤 14。

   *    切换为超级用户。 如果成功，不会收到任何确认信息。

        ```bash
        sudo -i
        ```

   *    创建将由 SQL Server 使用的文件夹。 

        ```bash
        mkdir <FolderName>
        ```

        \<FolderName> 是文件夹的名称。 如果文件夹不在正确的位置，需要指定文件夹的完整路径。 以下示例创建名为 /var/opt/mssql/userdata 的文件夹。

        ```bash
        mkdir /var/opt/mssql/userdata
        ```

   *    将 iSCSI 逻辑卷装载到上一步中创建的文件夹中。 如果成功，不会收到任何确认信息。

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> 是卷组的名称，\<LogicalVolumeName> 是已创建的逻辑卷的名称，\<FolderName> 是文件夹的名称。 示例语法如下所示。

        ```bash
        mount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

   *    将创建的文件夹的所有权更改为 mssql。 如果成功，不会收到任何确认信息。

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> 是已创建的文件夹的名称。 下面显示了一个示例。

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    将创建的文件夹组更改为 mssql。 如果成功，不会收到任何确认信息。

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> 是已创建的文件夹的名称。 下面显示了一个示例。

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    键入 `exit` 以退出超级用户身份。

   *    若要进行测试，请在该文件夹中创建数据库。 以下示例使用 sqlcmd 创建数据库，将上下文切换到该数据库，验证操作系统级别是否存在文件，然后删除临时位置。 可以使用 SSMS。
  
        ![sqlcmd 命令及对该命令的响应的屏幕截图。][9]

   *    卸载共享 

        ```bash
        sudo umount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> 是卷组的名称，\<LogicalVolumeName> 是已创建的逻辑卷的名称，\<FolderName> 是文件夹的名称。 示例语法如下所示。

        ```bash
        sudo umount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

14. 配置服务器，以便只有 Pacemaker 可以激活卷组。

    ```bash
    sudo lvmconf --enable-halvm --services -startstopservices
    ```

15. 生成服务器上卷组的列表。 列出的任何非 iSCSI 磁盘内容都由系统使用，例如用于 OS 磁盘的内容。

    ```bash
    sudo vgs
    ```

16. 修改文件 /etc/lvm/lvm.conf 的激活配置部分。 配置下列行：

    ```bash
    volume_list = [ <ListOfVGsNotUsedByPacemaker> ]
    ```

    \<ListOfVGsNotUsedByPacemaker> 是步骤 20 的输出中 FCI 不会使用的卷组的列表。 将每个卷组括在引号中并用逗号分隔。 下面显示了一个示例。

    ![显示 volume_list 值的示例的屏幕截图。][11]

17. 当 Linux 启动时，它将装载文件系统。 要确保只有 Pacemaker 可以装入 iSCSI 磁盘，请重新生成根文件系统映像。 

    运行以下命令，可能需要一些时间才能完成。 运行成功后不会收到任何消息。

    ```bash
    sudo dracut -H -f /boot/initramfs-$(uname -r).img $(uname -r)
    ```

18. 重新启动服务器。

19. 在将参与 FCI 的另一台服务器上，执行步骤 1 - 6。 这将向 iSCSI 服务器呈现 iSCSI 目标。 
 
20. 生成服务器上卷组的列表。 列表中应会显示之前创建的卷组。 

    ```bash
    sudo vgs
    ``` 

23. 启动 SQL Server 并验证它是否可以在此服务器上启动。

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```

24. 停止 SQL Server 并验证它是否已关闭。

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

25. 在将参与 FCI 的任何其他服务器上重复步骤 1 - 6。

现在可以配置 FCI 了。

| 分发 | 主题 |
| :----------- | :---- |
| 附带 HA 加载项的 Red Hat Enterprise Linux | [配置](sql-server-linux-shared-disk-cluster-configure.md)<br/>[操作](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md) |
| 附带 HA 加载项的 SUSE Linux Enterprise Server | [配置](sql-server-linux-shared-disk-cluster-sles-configure.md) |

## <a name="next-steps"></a>后续步骤

[配置故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/05-initiator.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/10-iscsiTargetSettings.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/15-iSCSITargetResults.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/20-iSCSITargetLogin.png
[5]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/25-iSCSIVerify.png
[6]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/7-setiscsinetwork.png
[7]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/30-iSCSIattachedDisks.png
[8]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/45-CopyMove.png
[9]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/50-ExampleCreateSSMS.png
[10]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/40-Create25GBVol.png
[11]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/55-ListOfVGs.png
