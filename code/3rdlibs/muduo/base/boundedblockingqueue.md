# 有边界的堵塞队列

推送数据时，在未到达边界前可以一直推送，到达边界后，就只能等待消费过后，才能继续推送

取数据和普通的堵塞队列是一样的，都是用一个条件变量来实现 `生产者-消费者` 模式。

## 实现
多使用了一个条件变量来对边界进行堵塞，take() 没消费一个数据后，就 `notify()` 发送信号， 而直到到达边界时，条件变量才会阻塞，
说明大部分情况发送的信号是没有人接收的。