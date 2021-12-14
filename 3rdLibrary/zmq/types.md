- [ ] send_flags

  * ZMQ_DONTWAIT

    > 异步

  * ZMQ_SNDMORE

    > 异步，消息有多部份，其余的已经正在发送的路上

  * none

    > 同步

- [ ] 通信方式

  **单播传输**

  * IPC ： 进程间通信，不能用于windows,且创建时要注意权限，不同用户权限的进程间不能通信，是一个文件，后缀名是.ipc
  * tcp： 网络间通信
  * inporc：线程间通信, 限制条件是:服务器必须在任何客户端发出连接之前发出绑定

  **多播传输**

  * epgm
  * pgm

- [ ] 内置的核心 ZeroMQ 模式是：

  - [**请求-回复**](https://zeromq.org/socket-api/#request-reply-pattern)，将一组客户端连接到一组服务。这是一个远程过程调用和任务分配模式。
  - [**Pub-sub**](https://zeromq.org/socket-api/#publish-subscribe-pattern)，它将一组发布者连接到一组订阅者。这是一种数据分布模式。
  - [**Pipeline**](https://zeromq.org/socket-api/#pipeline-pattern)，以扇出/扇入模式连接节点，可以有多个步骤和循环。这是一个并行的任务分配和收集模式。
  - [**Exclusive pair**](https://zeromq.org/socket-api/#exclusive-pair-pattern)，专门连接两个socket。这是在一个进程中连接两个线程的模式，不要与“正常”的套接字对混淆。

- [ ] zmq_poll

  可以接听多个socket的事件，也是事件驱动机制的。

- [ ] zmq_proxy

  代理模式，通过他可以接收多个服务端，多个客户端的请求和回复。

- [ ] 套接字类型

  * Router

    > 当ZMQ_ROUTER收到一个消息的时候，会自动在消息前面添加一帧，这一帧用来识别发送端的地址。

  * Dealer

    > 当有多个对端的时候，循环给单个对端发送消息。（注意：不是群发消息，与PUB不同）

  * Rep

    > 必须先接收消息，在回复数据，顺序不能打乱。

  * Req

    > 必须先发送，在接受回复的消息

- [ ] 合法的套接字绑定对

  * PUB - SUB
  * REQ - REP
  * REQ - ROUTER
  * DEALER - REP
  * DEALER - ROUTER
  * DEALER - DEALER
  * ROUTER - ROUTER
  * PUSH - PULL
  * PAIR - PAIR

  

  



