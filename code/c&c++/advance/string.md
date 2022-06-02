# std::string 

## assign

* string &operator=(const string &s); 把字符串s赋给当前字符串
* string &assign(const char *s); 用c类型字符串s赋值
* string &assign(const char *s,int n); 用c字符串s开始的n个字符赋值
* string &assign(const string &s); 把字符串s赋给当前字符串
* string &assign(int n,char c); 用n个字符c赋值给当前字符串
* string &assign(const string &s,int start,int n); 把字符串s中从start* 的n个字符赋给当前字符串
* string &assign(const_iterator first,const_itertor last);把first和last迭代器之间的部分赋给字符串

## substr

substr 是以自身作为被操作的资源，即截取自身字符串。
而 assign 是用别的string给当前字符串赋值
```c++
string a=s.substr(0,4); 
```

## ctor

* string(const char* a, const char* b); a,b必须是同一个字符串的不同偏移，最后的string 为 a为起始， 长度为 （b-a） 的字符串。
