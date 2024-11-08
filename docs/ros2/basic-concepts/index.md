# 基础概念

## Nodes

ROS 中的每个节点都应负责单一的模块化用途，例如控制车轮电机或发布激光测距仪的传感器数据。每个节点都可以通过主题、服务、操作或参数从其他节点发送和接收数据。

![ros2 graph](https://docs.ros.org/en/rolling/_images/Nodes-TopicandService.gif)

节点是 ROS 2 图形中的一个参与者，它使用客户端库与其他节点通信。节点可以与同一进程、不同进程或不同机器上的其他节点通信。节点通常是 ROS 图中的计算单元；每个节点都应做一件符合逻辑的事情。

节点可以向已命名的主题发布数据，向其他节点发送数据，也可以订阅已命名的主题，从其他节点获取数据。节点还可以充当服务客户端，让其他节点代表自己执行计算，或者充当服务服务器，为其他节点提供功能。对于长期运行的计算，节点可以充当行动客户端，让其他节点代其执行计算，或充当行动服务器，为其他节点提供功能。节点可提供可配置的参数，以便在运行时改变行为。

节点通常是发布者、订阅者、服务服务器、服务客户端、动作服务器和动作客户端的复杂组合，所有这些都同时存在。
节点之间的连接是通过分布式发现流程建立的。

## Discovery

节点的发现是通过 ROS 2 的底层中间件自动进行的：

1. 当一个节点启动时，它会向网络上具有相同 ROS 域（通过 ROS_DOMAIN_ID 环境变量设置）的其他节点发出自己存在的广告。节点会对该广告作出回应，并提供有关自己的信息，以便建立适当的连接和进行通信。
2. 节点会定期公布自己的存在，以便与新发现的实体建立连接，即使在初始发现期之后也是如此。
3. 节点在离线时向其他节点发布信息。

只有在服务质量设置兼容的情况下，节点才会与其他节点建立连接。以通话者-监听者演示为例。在一个终端运行 C++ talker 节点会发布主题消息，在另一个终端运行 Python 监听器节点会订阅同一主题的消息。你会发现这些节点会自动发现对方，并开始交换消息。

## Topics

ROS 2 将复杂的系统分解成许多模块化节点。主题是 ROS 图形的重要元素，是节点交换信息的总线.

![Topic](https://docs.ros.org/en/rolling/_images/Topic-SinglePublisherandSingleSubscriber.gif)

节点可以向任意数量的主题发布数据，并同时订阅任意数量的主题。

![Topic](https://docs.ros.org/en/rolling/_images/Topic-MultiplePublisherandMultipleSubscriber.gif)

主题是在节点之间移动数据的主要方式之一，因此也是在系统不同部分之间移动数据的主要方式之一。

## Services

服务是 ROS 图中节点的另一种通信方式。与主题的 “发布者-订阅者 ”模式相比，服务基于 “调用-响应 ”模式。主题允许节点订阅数据流并获得持续更新，而服务只有在客户端特别调用时才会提供数据。

![service](https://docs.ros.org/en/rolling/_images/Service-SingleServiceClient.gif)

![service](https://docs.ros.org/en/rolling/_images/Service-MultipleServiceClient.gif)

## Parameters

## Actions

行动是 ROS 2 中的通信类型之一，适用于长时间运行的任务。它们由三部分组成：目标、反馈和结果。

行动建立在主题和服务的基础上。它们的功能与服务类似，只是行动可以取消。它们还能提供稳定的反馈，而服务只会返回一个响应。

行动使用客户端-服务器模式，类似于发布者-订阅者模式（在主题教程中有描述）。行动客户端 “节点向 ”行动服务器 “节点发送目标，”行动服务器 "节点确认目标并返回反馈流和结果。

![action](https://docs.ros.org/en/rolling/_images/Action-SingleActionClient.gif)
