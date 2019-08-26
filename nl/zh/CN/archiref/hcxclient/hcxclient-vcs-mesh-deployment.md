---

copyright:

  years:  2019, 2019

lastupdated: "2019-07-09"

subcollection: vmware-solutions


---

# HCX 内部部署服务网
{: #hcxclient-vcs-mesh-deployment}

查看以下步骤来配置 HCX 客户机实例。

## 对 IBM Cloud for VMware Solutions 环境进行站点配对
{: #hcxclient-vcs-mesh-deployment-sitepair}

1. 登录到 VMware vSphere Web Client。
2. 从**主页**菜单中，选择 **HCX** 选项。
3. 在**基础架构** > **互连**下，单击**添加站点配对**。
  1. 将“站点 URL”设置为 HCX Cloud Manager URL，例如 `https://x.x.x.x.x`。
  2. 将“用户名”和“密码”设置为 HCX Manager 管理员详细信息：admin/password。

    先前的 vCenter Server 实例详细信息可以从 IBM Cloud for VMware Solutions 控制台中的**服务** > **HCX on IBM Cloud** 下获取。

4. 单击**连接**。

### 结果
{: #hcxclient-vcs-mesh-deployment-sitepair-results}

“站点配对”已注册并显示在 UI 上。

## 创建 HCX 内部部署概要文件
{: #hcxclient-vcs-mesh-deployment-profiles}

### 创建内部部署服务网网络概要文件
{: #hcxclient-vcs-mesh-deployment-profiles-network}

1. 登录到 vSphere Web Client。
2. 从**主页**菜单中，选择 **HCX** 选项。
3. 在**基础架构**下，单击**互连**。
4. 在**多站点服务网**下，单击**网络概要文件**
5. 在**创建网络概要文件**中：
   1. 选择“分布式端口组”，例如“外部”。
   2. 提供**外部 IP** 的 IP 地址范围、**外部子网前缀长度**、**外部网关**和 **DNS 详细信息**。
   3. 将 MTU 设置为 1500。
   4. 单击**创建**。
6. 对管理网络和 vMotion 网络重复上述步骤。
   注：将 MTU 值设置为 9000。

#### 结果
{: #hcxclient-vcs-mesh-deployment-profiles-network-results}

|网络名|MTU|
| -----| ----|
|外部|1500|
|管理|9000|
|vMotion|9000|

### 创建内部部署服务网计算概要文件
{: #hcxclient-vcs-mesh-deployment-profiles-compute}

1. 登录到 vSphere Web Client。
2. 从**主页**菜单中，选择 **HCX** 选项。
3. 在**基础架构**下，单击**互连**。
4. 在**多站点服务网**下，单击**计算概要文件**。
5. 在**创建计算概要文件**中：
   1. 提供“计算概要文件名称”。
   2. 选择要启用的“所有服务”，然后单击**继续**。
   3. 选择“集群”，然后单击**继续**。
   4. 选择“数据存储”，然后单击**继续**。
   5. 选择“管理的网络概要文件”，然后单击**继续**。
   6. 选择**外部/上行链路的网络概要文件**，然后单击**继续**。
   7. 选择“vMotion 的网络概要文件”，然后单击**继续**。
   8. 选择“vSphere 复制（管理）的网络概要文件”，然后单击**继续**。
   9. 选择用于扩展的“分布式交换机”，例如“专用交换机”，然后单击**完成**。

#### 结果
{: #hcxclient-vcs-mesh-deployment-profiles-compute-results}

已创建集群和存储器组合的计算概要文件，可用于创建服务网。

## HCX 内部部署服务网
{: #hcxclient-vcs-mesh-deployment-servicemesh}

## 创建内部部署服务网
{: #hcxclient-vcs-mesh-deployment-servicemesh-creation}

1. 登录到 vSphere Web Client。
2. 从**主页**菜单中，选择 **HCX** 选项。
3. 在**基础架构**下，单击**互连**。
4. 在**多站点服务网**下，单击**服务网**。
5. 在**创建服务网**中：
   1. 选择站点：内部部署和 vCloud 组织，然后单击**继续**。
   2. 选择“源计算概要文件”。
   3. 选择“远程计算概要文件”。例如，CloudCompute。
   4. 选择“所有服务”，然后单击**继续**。
   5. 在“高级配置 - 覆盖上行链路网络概要文件（可选）”页面上，单击**继续**。
   6. 单击**继续**。
   7. 单击**继续**，在“高级配置 - 配置 WAN 优化服务带宽限制”页面上保留缺省值。
   8. 提供“服务名称”，然后单击**完成**。
6. 观察创建服务网的任务列表。成功完成后，内部部署位置会有三个 HCX 设备，并且云位置也会有三个 HCX 设备。

### 结果
{: #hcxclient-vcs-mesh-deployment-servicemesh-results}

HCX 服务网是源站点和目标站点的有效 HCX 服务配置。可以将服务网添加到在这两个站点上都创建了有效计算概要文件的已连接站点对。

添加服务网会在这两个站点上都启动 HCX 互连虚拟设备的部署。互连服务网始终在源站点上创建。

## 网络延伸
{: #hcxclient-vcs-mesh-deployment-network-stretching}

### 延伸网络的过程
{: #hcxclient-vcs-mesh-deployment-stretching-process-stretch}

要通过 HCX 延伸网络（VLAN 或 VXLAN），请在客户机端 vCenter Web UI 中完成以下步骤。
1. 登录到 vSphere Web Client。
2. 从**主页**菜单中，选择 **HCX** 选项。
3. 在左侧菜单中的**服务**下，单击**网络扩展**。
4. 单击**扩展网络**：
   1. 选择要扩展的网络。
   2. 以 CIDR 格式输入当前缺省网关和子网掩码。
   3. 单击屏幕底部的**延伸**以开始网络延伸工作流程。

在 vCenter 客户机任务窗格中监视网络进度。

## 网络延伸的概念和最佳实践
{: #hcxclient-vcs-mesh-deployment-stretching-best-practices-network}

用于将客户机端网络桥接到云端 VXLAN 的媒介是包含专有 HCX 技术的尖端多隧道 VPN。该 VPN 并不是基于 NSX，但的确会使用 NSX 并扩展其功能。此过程通过客户机端 vCenter Web 用户界面进行控制，在客户机端和云端自动部署并启动两个端点。选择要延伸的网络时，既可单个选择，也可以批量选择。

此外，作为网络延伸工作流程的一部分，将授权云端的 NSX 来构建 VXLAN。随即 VXLAN 会连接到在指定的云端 L3 设备（处于未连接状态的 DLR 或 ESG）以及云端网络扩展设备上创建的接口。

迁移特定应用程序时，适用虚拟机 (VM) 所使用的所有网络都必须在整个 {{site.data.keyword.cloud}} 实例中延伸。

为什么是通常情况，而不是始终这样？在迁移 VM 后，断开与客户机端特定流量的连接比较有利。例如，VM 访客备份客户机可能会在移至云时导致很高的带宽使用量。VM 迁移时不需要访客中的备份客户机，因为在云端有更现代的块级别备份会自动选取该客户机。

不会访问客户机的备份网络适配器，因为访问此适配器将意味着访问每个 VM 以关闭访客中的客户机备份安排。因此，如果使用了备份网络，那么备份可能会失败。这是临时情况，在迁移后可以访问所有 VM 来禁用访客中的备份客户机之后，会解决此情况。

理论上，单个网络扩展的带宽为 4 Gbps。但是，该值可能是针对单个网络扩展对中所有延伸网络的限制，单个延伸网络是不会达到此带宽的。如果分配了足够的下层带宽并且等待时间短（小于约 10 毫秒），那么单个延伸网络的带宽可以达到约 1 Gbps。

### 邻近路由选项
{: #hcxclient-vcs-mesh-deployment-stretching-prox-routing}

如果没有任何类型的路由优化，那么扩展的网络会路由回客户机端以进行任何 L3 访问。这种长号效应会导致低效的流量模式，因为包需要在客户机（源）和云之间来回传输。甚至对于源 VM 和目标 VM 都在云中的情况也是如此。HCX 的“邻近路由”功能旨在解决此问题及处理本地流量输出。

## 相关链接
{: #hcxclient-vcs-mesh-deployment-deployment-related}

* [HCX 组件和术语的词汇表](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-components)
* [准备安装环境](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-planning-prep-install)
* [HCX 客户机部署](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-vcs-client-deployment)
* [VMware 混合云迁移](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-migrations)
* [监视参数和组件](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-monitoring)
* [HCX 故障诊断](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-troubleshooting)