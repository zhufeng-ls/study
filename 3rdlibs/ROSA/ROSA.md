[toc]



# ROSA

---

## 理解:



## 基础概念

1. 组件—— 划分应用组件以及调用系统组件实现应用组件的功能

2. 技能—— 机器人应用核心组件

3. 决策 —— 对机器人行为进行决策

   

### 组件

各种应用类型的接口，通过调用这些组件来实现我们的功能，比如速度，控制接口，ROSA也提供了技能(skill)、 自定义服务组件(service)、事件接收者(EventReceiver)3种抽象的接口类型，通过继承和添加，可以实现自己的组件。

#### 上下文

把这些共同的外部变量和外部操作提取成一个对象，借助这个对象，各程序段访问共同的外部变量和执行共同的外部操作将变得更为便捷。这种类型的对象，一般就是编程世界中的上下文（Context）对象。

#### ROSA 内部组件

根据是否在应用进程之间共享资源和状态，ROSA内部组件分为普通组件和系统服务组件2种。

普通组件的程序运行在应用进程，并且在每个使用它的应用进程之间都存在一份。

系统组件分为系统服务进程和系统服务代理两部分，系统服务进程管理应用间共享的资源和状态，借助RPC 技术，系统服务代理程序和系统服务进程的程序互相调用。通过调用服务代理，进而调用系统服务组件。



### 技能

用来接收指令并执行对应的功能。



### 决策

```
<?xml version="1.0" encoding="utf-8"?>
<decision-list>
    [<decision>
        <if>
            <{condition_name} [{condition_parameter_name}="{condition_parameter_value}"]... />
        </if>
        <then>
            <{fulfillment_name} [{fulfillment_parameter_name}="{fulfillment_parameter_value}"]... />
            ...
        </then>
    </decision>]
    ...
</decision-list>
```





## 英文单词

motion: 运动、动作

companion：配套，伴侣

decision：决策、决定

condition：条件

ultrasonic： 超声波

Chassis： 底盘、机架

orientation： 方向

duration： 持续、持续期间

clockwise： 顺时针方向
anti-clockwise： 逆时针方向

Microphone： 麦克风

rotate： 使旋转、使转动

servo： 舵机

specify： 明确指出、具体说明


