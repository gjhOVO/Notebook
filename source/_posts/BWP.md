---
title: BWP
date: 2024-09-17 17:21:30
categories:
    - 5G
---
# BWP

由于NR的信道带宽非常大，为了终端节能，NR种引入了BWP(BandWidth Part)的概念，BWP内的物理资源称为PRB，DL和UL最多同时配置4个BWP，但是同一时间只能有一个处于激活状态(但是也不可能所有BWP都处于非激活态)PDSCH,PDCCH,CSI-RS,PUSCH,PUCCH都必须要在激活的BWP内进行发送。对于FDD，上行BWP和下行BWP需要分别配置，对于TDD来说，上下行的BWP成对配置，上下行的BWP的中心频率必须相同，带宽可以不一样。

![20240917170904](https://raw.githubusercontent.com/gjhOVO/PicGo/main/PicGo/images20240917170904.png)



BWP有几种分类，InitialBWP，FirstActiveBWP，DefaultBWP和RegularBWP几种。

## Initial DL/UL BWP

这种BWP用于RRC建立之前的初始访问，记为BWP0，在小区初始接入阶段当搜索到SIB1消息后，SIB1消息中携带了Initial DL/UL BWP的相关信息。

在还没有读到SIB1的时候，Initial DL BWP跟Coreset0是一样的频域范围，读到SIB1以后，用SIB1中的配置作为Initial DL/UL BWP，并且用新的配置进行随机接入和RRC连接。**SIB1中应当携带Initial DL BWP的信息，应当包含完整的Coreset0。**

## First Active BWP

可以给SpCell或者SCell配置,在MCG中SpCell等同于PCell,在SCG中,SpCell等同于PSCell。SCell在小区组的SpCell之上提供额外的无线资源。是SpCell进行RRC配置或重配时激活的UL/DL BWP

## Default BWP

网络会给UE配置一个BWP计时器统计UE是否超过一定时间在active BWP上没有接收也没发送数据，就从active BWP切换到default BWP进行节能。如果未配置，就用Initial DL BWP作为default BWP，对于TDD，切换DL active BWP和UL active BWP时同时的。而FDD是可以单独互相切换的。

![20240917223853](https://raw.githubusercontent.com/gjhOVO/PicGo/main/PicGo/images20240917223853.png)

# BWP配置

非BWP0配置分为common parameters和dedicated parameters,common是cell specific也就是小区通用的，对于不同的UE来说，这个参数必须通用。

common parameters DL BWP包含小区特定的BWP参数(**频域位置,带宽,SCS，循环前缀CP**)以及DL BWP对于PDCCH和PDSCH的额外小区参数。UL BWP包含小区参数以及针对随机接入，PUCCH，PUSCH的参数。

dedicated parameters DL BWP包含PDCCH,PDSCH以及semi-persistant 半持久调度以及链路监测配置。UL BWP包含PUSCH,PUCCH,SRS等。

![20240917230932](https://raw.githubusercontent.com/gjhOVO/PicGo/main/PicGo/images20240917230932.png)

如上图所示，BWP0有两种配置方式：

Option1：只用cell-specific prarameters配置

Option2：UE-specific和cell-specific同时使用

Option1因为只有cell-specific参数，因此功能受限，只能用做临时作用，实际上还应该配置一个二者参数都有的BWP。因为没有UE参数，因此协议规定最多配置4个BWP，不算这个BWP0。

Ootion2的BWP0就二者参数都有，因此不需要配置多个BWP，只需要BWP0就可以跟UE建立完全的连接。这个算在协议的4个BWP里面，因此还可以最多建立3个BWP。
