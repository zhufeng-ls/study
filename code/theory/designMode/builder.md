# 建造者模式

**参考:**

https://www.zhihu.com/question/21857130/answer/2392642986


建造者（Builder）模式在百度百科上的定义：是一种将复杂对象的构建和它的表示分离，使得同样的构建过程可以创建不同的表示。\
换句话说: 一个对象如果很复杂，使用建造者模式**允许用户通过简单的方式构建这个对象**，而不用关注对象的内部细节,同样的构建过程，传入的参数不同，就可以创建不同的对象

和工厂方法的区别:\
**工厂方法是对于基本组件的创建，而建造者是对基本组件的组合。**

## 实现

**初始版**:

需要有4个类
* 产品类: 要创建的具体对象
* 抽象建造者类: 定义好建造者的接口
* 具象建造者类: 抽象建造者类的具体实现，内部是对产品类的不同设置。根据产品类参数的不同，可以实现多个建造者类。
* 引导类: 对建造者类的引导，算是最外层封装的接口。传入的建造者对象的不同，表示也不同。符合`同样的构建过程具有不同的表示`。


**变通版**:

初始版的代码过于复杂，而变通版则将产品类、建造者类、引导类合并在一个类中，实现原理就是在产品类的内部创建一个建造者的子类。

```java
public class Car {
    // 轮子
    private String wheel;
    // 能源
    private String energy;
    // 颜色
    private String color;
    public Car(){}
    public Car(CarBuilder carBuilder){
        this.wheel = carBuilder.wheel;
        this.energy = carBuilder.energy;
        this.color = carBuilder.color;
    }

    @Override
    public String toString() {
        return "Car{" +
                "wheel='" + wheel + '\'' +
                ", energy='" + energy + '\'' +
                ", color='" + color + '\'' +
                '}';
    }

    public static class CarBuilder extends Builder {
        // 轮子
        private String wheel;
        // 能源
        private String energy;
        // 颜色
        private String color;
        @Override
        public CarBuilder buildWheel(String wheel) {
            this.wheel = wheel;
            return this;
        }

        @Override
        public CarBuilder buildEnergy(String energy) {
            this.energy = energy;
            return this;
        }

        @Override
        public CarBuilder buildColor(String color) {
            this.color = color;
            return this;
        }

        @Override
        public Car getCar() {
            return new Car(this);
        }
    }
```

## 问题
1. 为什么不在产品类中直接使用 `setProperty()`设置属性？
   
    在产品类中设置属性是在对象创建之后的操作，而在builder类中，可以在创建对象的时候就传入这些参数。

2. 

## 应用场景
1. 需要先传入参数，在创建对象，且传入的参数过多。
2. 


## 总结

最传统的建造者模式体现着设计模式中“可替换性”的思想，Director不知道自己使用的具体是哪个Builder的实现类，所以Builder的实现类具备可替换性，也就可以理解为是一个个的组件。写代码时使用设计模式并不会让代码简单或者好写，反而会更复杂，但是在维护或者迭代时，就会省下很多精力。

不在产品类频繁使用 `setProperty` 的原因是不够优雅, 专业的事情就要交给专业的人去做。
