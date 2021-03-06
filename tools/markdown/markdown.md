## 复选框的使用

**`- [ ]`**

其含义为任务列表。

适用场景：
1. 项目组长统筹项目，分配好每个人应该完成的任务。
2. 对自己的项目实现步骤进行整理，则项目的实现顺序更明确。

## 生成目录

**`[toc]`**

## 图片对齐

图片可以用`<img>`中`align`标签或者`style`设置样式实现对齐方式

```
<img src='http://img2.imgtn.bdimg.com/it/u=4076814747,12025271&fm=26&gp=0.jpg' align='right' style=' width:300px;height:100 px'/>

<img src='http://img2.imgtn.bdimg.com/it/u=4076814747,12025271&fm=26&gp=0.jpg' style='float:right; width:300px;height:100 px'/>
```

## 删除线

\~\~这是删除的文本内容~~


## 设置大小、颜色

```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#008000>我是绿色</font>
<font color=Blue>我是蓝色</font>
<font size=5>我是尺寸</font>
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>

```



## 为文字添加背景色

<table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>





## 设置图片

### 设置百分比

```
<img src="http://pic11.photophoto.cn/20090626/0036036341009653_b.jpg" width="50%" height="50%">
```

<img src="http://pic11.photophoto.cn/20090626/0036036341009653_b.jpg" width="50%" height="50%">



### 设置大小

```
<img src="http://pic11.photophoto.cn/20090626/0036036341009653_b.jpg" width="251" height="350" >
```

<img src="http://pic11.photophoto.cn/20090626/0036036341009653_b.jpg" width="251" height="350" >



### 设置图片居中

```
<div align=center><img src="http://pic11.photophoto.cn/20090626/0036036341009653_b.jpg" width="50%" height="50%"></div>
```



<div align=center><img src="http://pic11.photophoto.cn/20090626/0036036341009653_b.jpg" width="50%" height="50%"></div>

### 跳转到文件的某个目录

跳转到 123.md 的 目录1 项

\[名称\](./doc/123.md#目录1)

### 注意

推送到github时，文件后缀名需为 `.md`。

若为README, github也可以将该文件内容作为提示文件，但github按`txt`文件类型解析它。


## 用途
* `>` 主要用于附加内容说明