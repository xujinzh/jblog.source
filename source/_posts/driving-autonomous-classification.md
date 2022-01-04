---
title: 无人驾驶6个等级划分
mathjax: true
author: Jinzhong Xu
date: 2021-06-22 16:29:29
tags:
- cv
- object tracking
categories:
- research
- object tracking
---

美国汽车工程师学会 ([SAE](https://www.sae.org/standards/content/j3016_201806/)) 定义了6个无人驾驶等级 — 从 0 级（完全手动）到 5 级（完全自动）。这些无人驾驶等级准则已经被美国交通部采纳。 

<!--more-->

![](https://www.synopsys.com/content/dam/synopsys/solutions/images/cs327718450-automotive-levels-infographic-v4.jpg.imgw.850.x.jpg)

# 0 级（无自动驾驶）

在当今的道路上行驶的大多数汽车都是 0 级：手动控制。由人来完成"动态驾驶任务"，尽管可能有相应的系统来辅助驾驶员，例如紧急制动系统，但从技术方面来讲，该辅助系统并未主动"驱动"车辆，所以算不上自动化驾驶。 

# 1 级（驾驶员辅助）

这是自动化的最低级别。车辆具有单独的自动化驾驶员辅助系统，例如转向或加速（巡航控制）。自适应巡航控制系统可以让车辆与前车保持安全距离，驾驶员负责监控驾驶的其他方面（例如转向和制动），因此符合 1 级标准。 

# 2 级（部分自动驾驶）

这指的是高级驾驶员辅助系统或 ADAS。车辆能够控制转向以及加速或减速。因为有驾驶员坐在汽车座位上，并且可以随时控制汽车，所以这一阶段的自动驾驶还算不上无人驾驶。特斯拉的 Autopilot 和凯迪拉克的（通用汽车）Super Cruise 系统都符合 2 级标准。

# 3 级（受条件制约的自动驾驶）

从技术角度来看，从 2 级到 3 级实现了重大飞跃，但从驾驶人员的角度来看，差别虽不是微不足道，却也算不上明显。

3 级无人驾驶汽车具有“环境检测”能力，可以自己根据信息做出决定，例如加速经过缓慢行驶的车辆。但是这个级别仍然需要人类操控。驾驶员必须保持警觉，并且在系统无法执行任务时进行操控。

差不多两年前，奥迪（大众汽车）宣布下一代 A8（奥迪旗舰轿车）将成为世界上第一款量产的 3 级无人驾驶汽车。他们的确做到了。奥迪 A8L 将于 2019 年秋季驶入经销市场。奥迪 A8L 采用 Traffic Jam Pilot 技术，该技术结合了激光雷达扫描仪以及先进的传感器融合和处理能力（如果某一组件发生故障，还可以使用内置冗余）。

然而，虽然奥迪正在开创汽车驾驶技术的奇迹，但美国在这方面的监管程序，已经从联邦层面的指导准则，转变为各州针对自动驾驶汽车制定不同的要求。所以目前 A8L 在美国[仍被归类为 2 级](https://www.cnet.com/roadshow/news/2019-audi-a8-level-3-traffic-jam-pilot-self-driving-automation-not-for-us/) 无人驾驶汽车，并且在交车时不提供达到 3 级功能所需的关键硬件和软件。然而在欧洲，奥迪将在德国首先推出带有 Traffic Jam Pilot 技术的真正 3 级无人驾驶汽车 A8L。 

# 4 级（高度自动驾驶）

3 级和 4 级自动化之间的关键区别在于，如果发生意外或系统失效，4 级自动驾驶汽车可以进行干预。从这个意义上来说，这些汽车*在大多数情况下*不需要人为干预。但是，驾驶员仍然可以选择手动超控。

4 级自动驾驶汽车可以采用无人驾驶模式运行。但由于立法和基础设施发展欠缺，4 级无人驾驶汽车只能在限定区域行驶（通常是在城市路况，最高平均速度达 30 英里/小时）。这被称之为地理围栏（geofencing）。因此，现有的大多数 4 级自动驾驶汽车都面向共享出行领域。例如：

- [NAVYA](https://www.motorauthority.com/news/1118809_navya-already-sells-fully-self-driving-cars-including-in-us) ，一家法国公司，已经在美国建造和销售 4 级自动驾驶纯电动班车和出租车，最高时速可达 55 mph。
- Alphabet 的 [Waymo](https://www.reuters.com/article/us-waymo-selfdriving-focus/waymo-unveils-self-driving-taxi-service-in-arizona-for-paying-customers-idUSKBN1O41M2) 一直在亚利桑那州测试无人驾驶汽车，至今已超过一年，路测行程已达到 1000 万英里以上，最近推出了 4 级无人驾驶出租车服务，车内不设任何安全驾驶员。
- 加拿大汽车供应商 [Magna](https://www.magna.com/insights/article/max4-magna-s-formula-for-winning-the-self-driving-car-race) 开发的 MAX4 自动驾驶技术，可在城市和高速公路环境中实现高达 4 级的自动驾驶功能。他们正在与 Lyft 合作，提供将普通车转化为自动驾驶车型的高科技套件。
- 就在几个月前，[沃尔沃和百度](https://www.autonews.com/article/20181101/MOBILITY/311019997/volvo-baidu-team-up-for-level-4-autonomous-evs-in-china)宣布建立战略合作伙伴关系，共同开发 4 级电动汽车，为中国的自动驾驶出租车市场提供服务。

# 5 级（完全自动驾驶）

5 级自动驾驶汽车不需要人为关注，从而免除了“动态驾驶任务”。5 级自动驾驶汽车甚至都不会有方向盘或加速/制动踏板。他们将不受地理围栏限制，能够去任何地方并完成任何有经验的人类驾驶员可以完成的操控。完全自动驾驶的汽车正在世界各地的几个试点区进行测试，但尚未向公众提供。向公众提供这一服务尚需数年时间。但是可以想象一下，您上车后，说"送我去上班"，剩下的事情就交给汽车来处理，与此同时，您可以润色一下演讲、在 Netflix 上煲剧或者整理下发型，真是太奇妙了。

# 参考链接

1. [我的无人车现在处于哪一阶段？ 了解6个无人驾驶等级](https://www.synopsys.com/zh-cn/automotive/autonomous-driving-levels.html)

