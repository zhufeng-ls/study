## Notice

**线程安全**

不要在线程之间传递套接字，为每个线程都创建一个独立的套接字



**多部份消息**

* 只有在发送最后一部分时才实际通过线路发送
* 使用zmq_poll()收到消息的第一部分时，其余消息已经到达。
* 您将收到一条消息的所有部分，或者根本没有。
* 消息的每一部分都是一个单独的`zmq_msg`项
* 无论您是否检查 more 属性，您都会收到消息的所有部分。
* 在发送时，ZeroMQ 将消息帧在内存中排队，直到收到最后一个，然后将它们全部发送。
* 没有办法取消部分发送的消息，除非关闭套接字。


