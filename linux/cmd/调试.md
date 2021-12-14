### 检查内存泄漏

  ```
  valgrind --tool=memcheck --leak-check=full --suppressions=vg.supp ./hwclient
  
  --leak-check=full 显示具体代码中泄漏的地方,
       1.full 表示显示全部。
       2.yes 与full相同
       3.summary 缺省值，表示显示简要信息
       4.no 不显示
       
  -show-reachble=yes 是否检查控制范围之外的泄漏,如全局指针，static指针
  --log-file=a.log 是否输出到文件
  
  
  definitely lost: 绝对丢失，没有任何指针指向该区域，已经造成了内存泄露。
  indirectly lost: 间接丢失，指向该内存的指针也位于内存泄露处。
  possibly lost: 可能丢失，仍然存在某个指针能够访问某块内存，但该指针指向的已经不是该内存首地址。
  still reachable: 仍然可以访问，仍有指针引用该内存块，只是没有释放而已，可以通过设置--show-reachable=yes来报错。
  suppressed: 抑制错误中的丢失
  ```

* 