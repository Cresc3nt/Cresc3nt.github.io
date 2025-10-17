---
layout: distill                                 # distill 风格，便于引入参考文献
title: 低轨卫星网络架构
date: 2025-10-07  10:00:00
description: 本文将对低轨卫星网络架构进行简单梳理。
tags:
  - 低轨卫星网络
  - 无线通信
categories:
  - 科研

toc:
  - name: 低轨卫星参数
    subsections:
      - name: 轨道倾角 (Inclination, $i$)
      - name: 轨道高度 (Height, $h$)
      - name: 最小仰角 (Minimum Elevation Angle, $e$)
      - name: 相位偏移 (Phase Offset, $p$ or $\varphi$)
  - name: 低轨卫星星座
    subsections:
      - name: 星座连接性 (Constellation Connectivity)
      - name: 系统动态性 (System Dynamics)
      - name: 干扰影响 (Impact of Interference)
  - name: 低轨卫星的低延迟优势与应用价值

bibliography: blogs/2025-10-07-leo-net-arch.bib  # 启用参考文献

pretty_table: true                               # 启用美化表格功能（通常用于加载 Bootstrap Table 等表格样式或脚本）
---

## 低轨卫星参数

所有现代低轨宽带星座均采用**圆形轨道**，以确保全球服务的一致性和简化轨道管理。

<aside>
  <p>这篇文章主要参考了<a href="https://bdebopam.github.io/" target="_blank">Debopam Bhattacherjee 博士</a>的毕业论文<d-cite key="DBLP:phd/basesearch/Bhattacherjee21"></d-cite>中的第二章。</p>
</aside>

描述一颗低轨卫星主要涉及四个参数：

<table class="table table-sm table-bordered">
  <thead>
    <tr>
      <th class="text-center">参数</th>
      <th class="text-center">符号</th>
      <th class="text-start">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="text-center">Inclination (轨道倾角)</td>
      <td class="text-center">$i$</td>
      <td class="text-start">轨道面与赤道面夹角，决定纬度覆盖范围</td>
    </tr>
    <tr>
      <td class="text-center">Height (轨道高度)</td>
      <td class="text-center">$h$</td>
      <td class="text-start">卫星距地表高度，影响延迟、寿命与速度</td>
    </tr>
    <tr>
      <td class="text-center">Minimum angle of elevation (最小仰角)</td>
      <td class="text-center">$e$</td>
      <td class="text-start">地面站可见卫星的最低仰角，权衡覆盖与信号质量</td>
    </tr>
    <tr>
      <td class="text-center">Phase offset (相位偏移)</td>
      <td class="text-center">$p$</td>
      <td class="text-start">相邻轨道卫星的相对位置，用于均匀分布</td>
    </tr>
  </tbody>
</table>

### 轨道倾角 (Inclination, $i$)

- 定义：卫星轨道平面与地球赤道平面之间的夹角（卫星从南向北穿越赤道时测量）。
- 典型值：
    - 90°：极地轨道（覆盖全球，包括两极）。
    - 0-90°：顺行轨道（prograde），不经过极地，更多时间在低纬度地区。
    - 90-180°：逆行轨道（retrograde），如太阳同步轨道（Sun-synchronous orbit），每天同一地方时经过同一地点，常用于遥感。
- 设计考量：
    - 避免赤道附近干扰：GEO卫星集中在赤道上空，因此LEO星座通常避免过低的倾角（如<25°）。
    - 发射成本：发射场纬度限制了最小可行倾角（如SpaceX在德州Boca Chica发射，纬度≈26°N，因此倾角不能低于此值，否则需昂贵变轨）。

下图展示了5条极地轨道与五条非极地轨道。

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blogs/2025-10-07-leo-net-arch/polar-and-inclined-orbits.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    polar and inclined orbits
</div>

### 轨道高度 (Height, $h$)

- 定义：卫星距地球表面的高度。

- 低轨范围：$h$ ≤ 2,000 km（典型值：550–1,300 km）。

- 影响：
    - 速度与周期：高度越低，速度越快（约27,000 km/h），轨道周期越短（约100分钟）。
    - 大气阻力：高度越低，大气阻力越大，需频繁轨道维持（但利于退役后快速离轨）。
    - 辐射环境：较低轨道可避开范艾伦辐射带，减少辐射损伤。
    - 监管：受FCC（美国）和ITU（国际）监管。

### 最小仰角 (Minimum Elevation Angle, $e$)

- 定义：地面站能与卫星通信所需的最低仰角（相对于地平线）。

- 作用：决定卫星对地面的覆盖范围（coverage cone）。

- 权衡：
  - $e$ 越小 → 覆盖范围大，可用卫星数多，但：路径损耗大（信号弱）、易受地形/建筑物遮挡、潜在干扰增加。
  - $e$ 越大 → 信号质量好、干扰少，但需更多卫星保证覆盖。

- 实例：
  - Starlink初期用 $e$ = 25°，后期提升至 40°。
  - Telesat计划用 $e$ = 10°（但可行性存疑）。

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blogs/2025-10-07-leo-net-arch/radius-of-coverage.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    radius-of-coverage
</div>

### 相位偏移 (Phase Offset, $p$ or $\varphi$)

- 描述相邻轨道上卫星的相对位置，需要设置合理的参数以避免碰撞。在一些研究中，也会用相位因子 $F$ 来表示，其实际含义与相位偏移相同。

  - 🛰️ 在低轨卫星星座中，相位偏移（phase offset）是一个介于 0 到 1 之间的无量纲参数，用于控制相邻轨道面上卫星穿越赤道时刻的相对关系。⏱️  
  - ➤ 若为 0，则相邻轨道面的第 $n$ 颗卫星同时过赤道；  
  - ➤ 若为 1，则第 $n$ 颗卫星与下一轨道面的第 $n+1$ 颗卫星同时过赤道。  
  - 📐 对于含 $P$ 个轨道面的均匀星座，相位偏移必须取 $1/P$ 的整数倍（即 $F/P$，$F$ 为整数相位因子）。  
  - 🔧 该参数不改变轨道高度或倾角，仅决定不同轨道面间卫星的“错位”程度，从而影响覆盖均匀性与碰撞风险。⚠️  
  - 🌍 例如，Starlink 初始壳层（32 个轨道面）采用相位偏移 5/32（$F$=5），而非 0.5，以避免偶数相位因子导致的轨道交叉点近距离接近问题！✨

- 大部分研究中设 $p$ = 0.5，使卫星在壳层（shell）中分布最均匀，最大化覆盖，避免处理复杂的相位优化，从而更专注与拓扑或路由问题的研究。

> 💡 除此之外，一个卫星轨道还应该包含七个要素：包括一个历元和六个开普勒要素，但在低轨卫星网络的研究中所有的轨道均为圆形轨道，所以无需特别关注其他参数。
{: .block-tip }

## 低轨卫星星座

如果每颗卫星轨道参数（高度、倾角等）都不同，它们之间的相对运动将非常复杂。这种状态下星间链路（ISL）只能短暂维持，频繁切换（handoff）会导致连接中断、延迟飙升（切换可能耗时数十秒）。因此需要将卫星分组为壳层——每个壳层内所有卫星共享相同的轨道高度与轨道倾角。

一个低轨卫星星座 $C$ 由 $s$ 个壳层（shell）构成，一个壳层由以下四个参数完全定义：

<table class="table table-sm table-bordered">
  <thead>
    <tr>
      <th class="text-center">参数</th>
      <th class="text-center">符号</th>
      <th class="text-start">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="text-center">Number of orbits (轨道数量)</td>
      <td class="text-center"><em>o</em></td>
      <td class="text-start">一个壳层中包含的轨道面数</td>
    </tr>
    <tr>
      <td class="text-center">Satellites per orbit (每轨卫星数)</td>
      <td class="text-center"><em>n</em></td>
      <td class="text-start">每条轨道上均匀分布的卫星数量</td>
    </tr>
    <tr>
      <td class="text-center">Inclination (轨道倾角)</td>
      <td class="text-center"><em>i</em></td>
      <td class="text-start">壳层中所有轨道共用的倾角</td>
    </tr>
    <tr>
      <td class="text-center">Height (轨道高度)</td>
      <td class="text-center"><em>h</em></td>
      <td class="text-start">壳层中所有卫星的运行高度</td>
    </tr>
  </tbody>
</table>

可以参考<a href="/assets/html/blogs/2025-10-07-leo-net-arch/leo_shell_example1.html" target="_blank">示例1</a>与<a href="/assets/html/blogs/2025-10-07-leo-net-arch/leo_shell_example2.html" target="_blank">示例2</a>，它们分别描述了4条轨道与32条轨道时的壳层样例。

> 💡 许多研究中使用 $o^2$ 来表示星座的大小，代表有 $o$ 条轨道，同时每条轨道上有 $n = o$ 颗卫星。这种对称设计便于仿真中控制变量，同时能直观反映星座密度对延迟和连通性的影响。
{: .block-tip }

### 星座连接性 (Constellation Connectivity)

现代低轨卫星星座的连接性建立在两类链路之上：地星链路（Ground-Satellite Links, GSL）与星间链路（Inter-Satellite Links, ISL）。

GSL 使用射频而非激光，因其对云层和降水更具鲁棒性。每颗卫星配备多个天线，每个天线支持多个可软件定义的窄波束（如下图所示），能够动态调整方向、形状与频段分配，以最大化吞吐量。地面站类型决定其连接能力：普通用户终端通常只有一个相控阵天线，一次仅连一颗卫星；而网关或企业终端则使用多个抛物面天线，可同时连接多颗卫星以提升容量和可靠性。GSL 带宽随星地距离 $d_s$ 增大而衰减，建模为与 $h^2/d_s^2$ 成正比；只有当用户位于卫星星下点（nadir）且无其他用户共享时，才能获得满带宽。例如，Starlink 宣称其单星下行容量约为 20 Gbps。

> 💡 卫星的星下点（nadir）是指从卫星向地心方向作一条直线，该直线与地球表面相交的点，也就是卫星正下方的地表位置。在轨道力学和卫星通信中，星下点具有特殊意义：此时地面站与卫星之间的距离最短，信号路径损耗最小，通信质量最好。例如，在计算地星链路（GSL）带宽时，只有当用户终端恰好位于卫星的星下点、且该卫星波束覆盖范围内没有其他用户共享资源时，才能获得该链路的最大可用带宽。随着地面站偏离星下点，星地距离增加，路径损耗增大，可用带宽随之下降，通常建模为与 $h^2/d_s^2$ 成正比（其中 $h$ 为轨道高度，$d_s$为实际星地距离）。因此，星下点是评估卫星覆盖性能和链路质量的关键参考位置。
{: .block-tip }

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blogs/2025-10-07-leo-net-arch/GSL.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    GSL
</div>

相比之下，ISL 是实现低延迟端到端通信的关键。当前行业普遍采用 “+Grid” 拓扑：每颗卫星通过激光链路连接 4 个邻居——同轨道前后各 1 颗，相邻轨道各 1 颗。这种结构在卫星相对速度较低时可维持长期稳定连接，避免频繁切换。ISL 使用激光，因其波束极窄、发散小、干扰低。但其长度受物理限制：激光不能进入 80 km 以下大气层（中层大气含水汽，会散射光束），因此给定轨道高度 $h_ S$ ，可计算最大 ISL 距离。例如，Starlink S1（$h$=550 km）的最大 ISL 长度约为 5,014 km。在稀疏星座中，ISL 仅限相邻轨道；但在密集星座中，一颗卫星理论上可连接上百颗其他卫星。然而，实际部署仍受限于：
- 功耗约束：更长距离需更高发射功率；
- 设备重量与成本：大功率激光终端更重、更贵；
- 热管理与指向精度：高速运动下维持激光对准极具挑战。

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blogs/2025-10-07-leo-net-arch/ISL.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    ISL
</div>

因此，尽管物理可见性允许更多连接，工程现实仍使 +Grid 成为主流选择。值得注意的是，若无 ISL，星座只能采用“弯管模式”（bent-pipe）——数据数据要么通过最近的基站（连接到互联网）传输，要么经过卫星和地面站的多次转发，导致跨洲通信延迟显著增加。Starlink 已发射具备 ISL 能力的卫星，但大规模运行细节尚未公开。

### 系统动态性 (System Dynamics)

LEO 星座的高速运动带来了显著的系统动态性。以 550 km 高度为例，卫星速度高达 27,306 km/h，约每 100 分钟绕地球一圈。这种高速导致：
- 地星链路仅能维持几分钟，需频繁切换；
- ISL 长度和延迟持续变化，尤其在高纬度地区，不同轨道卫星因几何汇聚而相对速度突变；
- 端到端路径所经卫星、GSL 与 ISL 距离、总延迟均随时间演化。

这种动态性虽带来挑战，却具有高度可预测性（基于开普勒轨道力学），与地面移动网络（如蜂窝网）中用户随机移动有本质不同。更重要的是，LEO 网络的移动主体是网络核心本身——卫星即路由器，而非仅终端移动。此外，其规模远超传统移动系统：数千颗具备 Tbps 级交换能力的“飞行节点”在全球尺度上协同工作。这些特性使得 LEO 网络既不能直接套用现有协议，也为基于轨道预测的预计算路由、动态拓扑优化等新方法提供了独特机会。

### 干扰影响 (Impact of Interference)

尽管 LEO 星座密度高、频谱复用频繁，但现行的主流星座已部署一整套在线、软件定义的干扰抑制机制，包括：
- 低轨道高度缩小波束覆盖范围；
- 极窄波束（Starlink：1.0°–1.5°）减少空间重叠；
- 多天线 + 多波束支持灵活频段分配；
- 空间复用策略：相同频段可在角度分离 ≥10° 的区域复用；
- 动态波束分裂/合并以应对负载与干扰变化；
- 优先服务高仰角用户（信号强、路径损耗小）；
- 在轨流量重路由避开拥塞或干扰区域。

鉴于这些成熟且可调的缓解手段，现有的研究聚焦于拓扑结构、动态路由与端到端延迟等核心网络问题。

## 低轨卫星的低延迟优势与应用价值

低轨（LEO）卫星星座不仅是一种全球覆盖的通信基础设施，更代表了一种突破现有地面光纤网络延迟瓶颈的新范式。当前互联网的高延迟部分源于光纤路径的物理限制：光在光纤中传播速度仅为真空中光速 $c$ 的约 2/3 ，且实际铺设路径受地形与经济因素制约，往往严重绕行。即便假设使用所有已知光纤并沿最短路径传输（即“f-latency”），其延迟仍显著高于理论极限（“c-latency”，即光在真空中沿大圆传播所需时间）。LEO 星座则通过近真空环境中的射频与激光链路，以接近光速 $c$ 直线传输数据，在长距离通信中展现出显著优势。仿真表明，一个中等规模的 LEO 星座（如 30²，即 900 颗卫星）在华盛顿特区至法兰克福的路径上，其端到端延迟几乎总是优于最优光纤（32.6 ms）；而更密集的星座（如 40²、50²）甚至能匹配或超越高频交易（HFT），逼近 c-latency（21.7 ms）。

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blogs/2025-10-07-leo-net-arch/latency-example.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    华盛顿-法兰克福端到端通信延迟
</div>

> 📊 这张图展示了不同星座大小下华盛顿特区与法兰克福之间的延迟。CDF 曲线越靠左、越陡峭，说明该网络的延迟表现越好、越稳定。<br>
> 纵轴（CDF）：累积分布函数，这里表示延迟小于或等于横轴值的概率。例如，如果某条曲线在横轴 35 ms 处对应的 CDF 值是 0.8，那就意味着，在这个星座配置下，有 80% 的时间，端到端延迟都小于或等于 35 毫秒。<br>
> c-latency：光在真空中沿地球表面最短路径（大圆弧）传播所需的时间，是这是理论上可能达到的最低延迟极限。<br>
> f-latency：理想光纤网络能达到的最佳延迟，不考虑现实中的路由绕行或设备处理延迟。<br>
> Hibernia：实际存在的、专为低延迟设计的跨大西洋海底光缆——Hibernia Express。<br>
> Internet：指当前全球公共互联网的实际平均延迟。<br>
> HFT：High-Frequency Trading（高频交易）行业的实际观测延迟。<br>
{: .block-tip }

这种低延迟并非仅是技术指标的提升，而是对现代互联网应用体验与商业价值的直接赋能。许多关键应用——如增强/虚拟现实（AR/VR）、远程手术、实时协同创作——对交互延迟极为敏感，毫秒级的改善即可决定成败。即使是看似普通的网页浏览，其用户体验也与延迟高度相关：实验表明，在带宽固定为 10 Mbps 的条件下，客户端与服务器间 RTT 每增加 10 毫秒，页面 85% 视觉内容完成加载的时间（viz85）。这一线性关系意味着，LEO 在跨洲通信中节省的数十毫秒，可转化为显著更快的页面加载速度。而商业影响更为直接：Google 发现搜索延迟增加 400 毫秒会导致用户搜索量下降 0.7%；亚马逊测算每增加 100 毫秒延迟，销售额下降约 1%。正因如此，内容分发网络（CDN）将“降低延迟”作为核心价值主张。

综上，LEO 星座通过其独特的空间路径与近光速传播能力，不仅在物理层面提供了超越地面光纤的低延迟可能性，更在应用层面回应了互联网对极致交互体验与商业效率的迫切需求。这一“技术潜力—应用价值”的双重优势，构成了推动 LEO 网络研究与部署的核心驱动力。

<!-- ## 参考文献

1. Debopam Bhattacherjee. *Towards Performant Networking from Low-Earth Orbit*. PhD thesis, ETH Zurich, August 2021. doi:[10.3929/ETHZ-B-000511115](https://doi.org/10.3929/ETHZ-B-000511115). -->
