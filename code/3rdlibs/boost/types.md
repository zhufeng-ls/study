## Boost


### boost::numeric_cast
在c++中，我们经常需要把不同类型的数字互相转换，如将一个数字在long和short之间转换。但由于各数字的精度不同，当一个数字从"大"类型到"小"类型就可能导致转换失败，如下所示：

  ```c++
  long n1 = 99999999;
  short n2 = static_cast<short>(n1);
  ```
  
  对于如上转换，n2得到的是一个负数，显然这个不是我们所期望的，并且这种运行时的错误是很难检测的，一旦使用了这个错误的转换后的数据，后果不堪设想。
  
  boost::numeric_cast可以帮助我们解决这一问题，对于上面的转换，boost::numeric_cast会抛出一个boost:: bad_numeric_cast这个异常对象。从而保证转换后值的有效性。上述代码可以改写为如下：
  ```
  try
  {
      long n1 = 99999999;
      short n2 = boost::numeric_cast<short>(n1);
  }
  catch(boost::bad_numeric_cast&)
  { 
      std::cout<<"The conversion failed"<<std::endl; 
  } 
  ```

### implicit_cast

涉及到对象的转换时，若是向下转换可能会导致程序崩溃（基类向子类转换），implicit_cast 只允许向上转换。 

### down_cast

允许向下转换

### boost::any

可以放任意的类型，也可以是用户自定义的类型。
  

