---
layout: post
title: "RabbitMQ ConfirmCallback 和 ReturnCallback 的区别"
categories: 技术
tags: ["RabbitMQ"]
---

* 目录
{:toc .toc}

通过 Publisher Confirms and Returns 机制，生产者可以判断消息是否发送到了 Exchange 及 Queue，而通过消费者确认机制，RabbitMQ 可以决定是否重发消息给消费者，以保证消息被处理。

## ConfirmCallback 和 ReturnCallback 的区别

ReturnCallback 接口用于实现消息发送到 RabbitMQ 交换器，但无相应队列与交换器绑定时的回调。

ConfirmCallback 接口用于实现消息发送到 RabbitMQ 交换器后接收 ack 回调。

Confirm 主要是用来判断消息是否有正确到达交换机，如果有，那么就 ack 就返回 true，如果没有，则是 false。



Returns are when the broker returns a message because it's undeliverable
(no matching bindings on the exchange to which the message was published,
and the mandatory bit is set).

Confirms are when the broker sends an ack back to the publisher,
indicating that a message was successfully routed.





