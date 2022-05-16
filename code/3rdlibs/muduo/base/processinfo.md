# processinfo

## 获取 pid

```c
::getpid();
```

## 获取 uid

进程的用户id，扩展有 RUID(实际用户id)、EUID(有效用户id)、SUID(保存设置用户id)。

```c
::getuid();
```

## 获取用户名

需要先获取uid, 再获取用户名
```c
getpwuid_r(uid(), &pwd, buf, sizeof buf, &result);
```

## 获取 euid

```c
::geteuid();
```

## 获取内存页大小

```c
int g_pageSize = static_cast<int>(::sysconf(_SC_PAGE_SIZE));
```

## 是否为Debug构建

判断`NDEBUG`宏是否存在
```c
#ifdef NDEBUG
    return false;
#else
    return true;
#endif
```

## 获取主机名称

```c
::gethostname(buf, sizeof buf);
```

## 获取进程名称

读取 /proc/self/stat文件，在文件的内容中可以找到读取该文件的进程名称
```c++
std::string stat = FileUtil::readFile("/proc/self/stat", 65536, &result);

StringPiece name;
size_t lp = stat.find('(');
size_t rp = stat.rfind(')');
if (lp != string::npos && rp != string::npos && lp < rp)
{
    name.set(stat.data() + lp + 1, static_cast<int>(rp - lp - 1));
}
return name;
```

## 获取进程状态

读取 `/proc/self/status`
```c++
FileUtil::readFile("/proc/self/status", 65536, &result);
```

## 获取线程状态

在线程中读取 `/proc/self/task/%d/stat`
```c++
char buf[64];
snprintf(buf, sizeof buf, "/proc/self/task/%d/stat", CurrentThread::tid());
string result;
FileUtil::readFile(buf, 65536, &result);
```

## 获取进程的绝对路径

```c
string result;
char buf[1024];
ssize_t n = ::readlink("/proc/self/exe", buf, sizeof buf);
if (n > 0) { result.assign(buf, n); }
```

## 获取进程打开的文件描述符个数

```c
__thread int t_numOpenedFiles = 0;
int fdDirFilter(const struct dirent* d)
{
    if (::isdigit(d->d_name[0])) { ++t_numOpenedFiles; }
    return 0;
}

int scanDir(const char* dirpath, int (*filter)(const struct dirent*))
{
    struct dirent** namelist = NULL;
    int result = ::scandir(dirpath, &namelist, filter, alphasort);
    assert(namelist == NULL);
    return result;
}

int ProcessInfo::openedFiles()
{
    t_numOpenedFiles = 0;
    scanDir("/proc/self/fd", fdDirFilter);
    return t_numOpenedFiles;
}
```

## 获取最大的文件描述符个数

```c
struct rlimit rl;
if (::getrlimit(RLIMIT_NOFILE, &rl)) { return openedFiles(); }
else
{
    return static_cast<int>(rl.rlim_cur);
}
```

## 获取 CPU 运行时间

```c++
::times(&tms) >= 0;
const double hz = static_cast<double>(clockTicksPerSecond());
t.userSeconds = static_cast<double>(tms.tms_utime) / hz;
t.systemSeconds = static_cast<double>(tms.tms_stime) / hz;
```

## 获取线程数量

读取 /proc/self/status 中的 Threads 字段
```c++
string status = procStatus();
size_t pos = status.find("Threads:");
```

## 获取所有的线程信息

```c++
__thread std::vector<pid_t>* t_pids = NULL;
int taskDirFilter(const struct dirent* d)
{
    if (::isdigit(d->d_name[0]))
    {
        t_pids->push_back(atoi(d->d_name));
    }
    return 0;
}


std::vector<pid_t> ProcessInfo::threads()
{
    std::vector<pid_t> result;
    t_pids = &result;
    scanDir("/proc/self/task", taskDirFilter);
    t_pids = NULL;
    std::sort(result.begin(), result.end());
    return result;
}
```