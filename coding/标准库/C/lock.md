### pthread_cond_signal

pthread_cond_signal函数的作用是发送一个信号给另外一个正在处于阻塞等待状态的线程,使其脱离阻塞状态,继续执行.如果没有线程处在阻塞等待状态.



### 异步锁操作

1.pthread_mutex_trylock

pthread_mutex_trylock:函数pthread_mutex_trylock是pthread_mutex_lock的非阻塞版本。如果mutex参数所指定的互斥锁已经被锁定的话，调用pthread_mutex_trylock函数不会阻塞当前线程，而是立即返回一个值来描述互斥锁的状况。

2.pthread_mutex_timedlock

pthread_mutex_timedlock: 和pthread_mutex_trylock 基本相似，他代表在到达超时时间时，若还未获得锁，将返回一个非零的错误码。


### 条件锁

示例1：[链接](https://blog.csdn.net/techhome803/article/details/8276058)

pthread_cond_init(&cond, NULL); /* 动态初始化条件变量 */

pthread_cond_t cond = PTHREAD_COND_INITIALIZER; /* 静态初始化条件变量 */

pthread_cond_wait(&cond); /* 等待条件变量触发，使用wait之前，必须要上锁，因为函数里面会进行解锁。*/

pthread_cond_timedwait(&cond); /* 超时等待条件变量触发 ，当在指定时间内有信号传过来时，返回0，否则返回一个非0数（我没有找到返回值的定义);*/

pthread_cond_signal(&cond); /* 激活一个等待该条件的线程，单播 */

pthread_cond_broadcast(&cond); /* 激活所有等待该条件的线程，广播 ，而对于
该函数，它使所有由参数cond指向的条件变量阻塞的线程退出阻塞状态，如果没有阻塞线程，则函数无效*/

pthread_cond_destroy(&cond); /* 销毁条件变量 ,调用destroy函数解除条件变量并不会释放存储条件变量的内存空间,静态创建的条件变量不能调用这个函数*/

pthread_condattr_setshared函数：
功能：设置条件变量的进程共享属性

pthread_condattr_getshared函数：
功能：获取条件变量的进程共享属性
  
pthread_condattr_setclock函数：
功能：此函数用于设置pthread_cond_timewait函数使用的时钟ID

pthread_condattr_getclock函数：
功能：此函数获取可被用于pthread_cond_timedwait函数的时钟ID。
pthread_cond_timedwait函数使用前需要用pthread_condattr_t对条件变量进行初始化。


<!--**条件变量始终都会和一个互斥锁配合使用，当pthread_cond_wait()被调用阻塞住线程的时候，lock会被自动释放，当信号来的时候会自动上锁。thread_1获取到互斥锁之后，因为条件变量的存在，thread_1被阻塞住，互斥锁自动释放掉，当条件变量满足条件之后系统会将互斥锁再重新锁上**-->