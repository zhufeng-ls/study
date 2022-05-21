# 线程池

## 1. 实现

### 1.1 确定任务队列的最大容量
### 1.2 确定线程池的线程数量
### 1.3 使用简单的 生产者-消费者 模型

1. 创建两个条件变量， notEmpty_, notFull_,

2. 在线程内部，一直在循环消费任务，通过 `take()` 函数。
而 `take()` 函数内部一直通过 `notEmpty_.wait()` 来等待任务的到来， 且同时 `notFull_.notify();` 来通知可以使用 `run()` 函数推送任务了。

3. 我们在外部可以通过 `run()` 函数循环往线程池里面推送任务，其实是否存放任务的`std::deque`(双向队列)里面填充，填充时要注意上锁，且填充时可以使用`右值引用`来提高效率, 填充时要主要队列是否满了，若满了，则通过 `notFull_.wait()` 进行堵塞。若不满，则 `notEmpty_.notify();`  通知消费线程可以消费任务了。

4. 若停止线程池， 先将running_ 状态置为 false。然后 `notFull.notifyAll_();`、 `notEmpty_.notifyAll();` 通知所有消费线程。
   
   这里有几个注意事项:  
   * take() 函数内部取任务时，当收到信号时，一定要判断队列是否为空，不能盲目的取。
        ```c++
        ThreadPool::Task ThreadPool::take()
        {
            MutexLockGuard lock_(mutex_);
            while (queue_.empty() && running_) { notEmpty_.wait(); }

            Task task;

            // 要先判断队列里是否有任务
            if (!queue_.empty())
            {
                task = queue_.front();
                queue_.pop_front();
                if (maxQueueSize_ > 0)
                {
                    notFull_.notify();
                }
            }
            return task;
        }
        ```
    * run() 推送任务时， 若要使用断言判断任务队列是否满了，那么判断运行状态的代码就只能放在 `_notFull.wait();` 后面，否则当任务队列满了的时候，关闭线程池就会异常报错退出。
        ```c++
        void ThreadPool::run(Task task)
        {
            if (threads_.empty())
            {
                task();
            }
            else
            {
                MutexLockGuard lock(mutex_);
                while (isFull() && running_) { notFull_.wait(); }
                
                // 这个一定要放在条件变量的后面
                if (!running_)
                {
                    return;
                }
                // 否则到这一步就会崩溃
                assert(!isFull());

                queue_.push_back(std::move(task));
                notEmpty_.notify();
            }
        }
        ```