## termios

参考网址:

https://blog.csdn.net/williamwang2013/article/details/8560552
https://blog.csdn.net/williamwang2013/article/details/8560552

### tcgetattr
用来获取终端参数

### ioctl
是设备驱动程序中对设备的I/O通道进行管理的函数。所谓对I/O通道进行管理，就是对设备的一些特性进行控制，例如串口的传输波特率、马达的转速等等。

**函数**：
```
int ioctl(int fd, ind cmd, …)
```

**参数**:
* FIONREAD：

  ```
  ioctl(keyFd, FIONREAD, &b)
  得到缓冲区里有多少字节要被读取，然后将字节数放入b里面。
  ```
  
### fcntl

```
int fcntl(int fd, int cmd, long arg)
参数: 
 cmd: F_SETFL 或 F_GETFL ，代表设置文件属性或者获取文件属性
 arg: 代表设置的属性类型，当 cmd为 F_SETFL 启用。
```

### setvbuf

```
int setvbuf(FILE *stream, char *buffer, int mode, size_t size)

参数
stream -- 这是指向 FILE 对象的指针，该 FILE 对象标识了一个打开的流。
buffer -- 这是分配给用户的缓冲。如果设置为 NULL，该函数会自动分配一个指定大小的缓冲。
mode -- 这指定了文件缓冲的模式：

_IOFBF	全缓冲：对于输出，数据在缓冲填满时被一次性写入。对于输入，缓冲会在请求输入且缓冲为空时被填充。
_IOLBF	行缓冲：对于输出，数据在遇到换行符或者在缓冲填满时被写入，具体视情况而定。对于输入，缓冲会在请求输入且缓冲为空时被填充，直到遇到下一个换行符。
_IONBF	无缓冲：不使用缓冲。每个 I/O 操作都被即时写入。buffer 和 size 参数被忽略。
```

### tcflush
丢弃要写入fd，但尚未传输的数据，或者收到但是尚未读取的数据.

头文件:
```
#include <termoios.h>
```

有下面3种类型。
TCIFLUSH   //清除正收到的数据，不会读取出来
TCOFLUSH   //清除正写入的数据，且不会发送至终端
TCIOFLUSH  //清除所有正在发生的I/O数据。

### cfmakeraw
Raw模式可以禁止行缓冲(line buffering)，处理挂起（CTRLZ）、中断或退出（CTRLC）等控制字符时，将直接传送给程序去处理而不产生终端信号，而cfmakeraw就是设置Raw模式的.

### [采样率](https://blog.csdn.net/q1281405619/article/details/108253494)

采样频率,也称为采样速度或者采样率,定义了每秒从连续信号中提取并组成离散信号的采样个数,它用赫兹(Hz)来表示。即单位时间内采集的点数就叫做采样率。

### 比特率

比特率(bit rate)又称传信率、信息传输速率(简称信息速率，information rate)。其定义是：通信线路(或系统)单位时间(每秒)内传输的信息量，即每秒能传输的二进制位数，通常用Rb表示，其单位是比特/秒(bit/s或b/s，英文缩略语为bps)。

### 波特率

波特率(Baud rate)又称传码率、码元传输速率(简称码元速率)、信号传输速率(简称信号速率，signaling rate)或调制速率。其定义是：通信线路(或系统)单位时间(每秒)内传输的**码元(脉冲)**个数；或者表示信号调制过程中，单位时间内调制信号波形的变换次数，通常用RB表示，单位是波特(Bd或Baud，前者规范)。

### 数据传输率

数据传输率(data transfer rate)又称数据传输速率、数据传送率。其定义是：通信线路(或系统)单位时间(每秒)内传输的字符个数；或者单位时间(每秒)内传输的码组(字块)数或比特数。其单位是字符/秒；或者码组/秒、比特/秒(可见，当数据传输率用“bit/s”作单位时，即等于比特率)。 **所以它的单位在不同的应用中是不同的。**

