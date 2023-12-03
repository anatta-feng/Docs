# 简介

RabbitMQ 是AMQP 协议的实现。AMQP 解决了什么问题？

大型的软件系统模块间需要通信，传统的 IPC 是不适用的，因为传统的 IPC 需要在同一系统内，不适合系统的扩展。

如果使用 Socket，扩展性有了，但是同时会有许多问题：

* 信息的发送者和接收者如何维持这个连接，如果一方的连接中断，这期间的数据是以什么方式丢失？

* 如何降低发送者和接收者的耦合度？

* 如何让Priority高的接收者先接到数据？

* 如何做到Load Balance？有效均衡接收者的负载？

* 如何有效的将数据发送到相关的接收者？也就是说将接收者subscribe 不同的数据，如何做有效的filter。

* 如何做到可扩展，甚至将这个通信模块发到cluster上？

* 如何保证接收者接收到了完整，正确的数据？

AMQP 协议解决了这个问题。

# 系统架构

![](https://upload-images.jianshu.io/upload_images/292448-fb150ae7f05e1f11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)

实质上来说是一个生产者消费者模式，跟之前看的 celery 类似。

## RabbitMQ Server

也叫 Broker Server，来维护从生产者到消费者的路线。

## Client P

数据的生产者

## Client C

数据的消费者

