# QWT学习总结



1.QwtSeriesData< QPointF >
该类对应QwtSeriesData< T >、QwtSeriesData< QwtSetSample >、QwtSeriesData< QwtIntervalSample >。
这几个类都是对一系列数据的包装，用于绘制曲线。
使用：在工程中继承QwtSeriesData实现一个类包装一组数据处理，设置到QwtPlotCurve中绘制。

使用了面向对象的单一职责？代码更清晰。
QwtPlotCurve使用入参为QwtSeriesData类型的setSamples接口函数，可以让代码更好维护清晰。



2、QwtScaleMap 比例映射
该类的作用：一个比例尺映射，提供了比例尺坐标系统向线性的绘图设备坐标系统的转换，反之亦然。
该类组合了QwtTransform，转换器分为三种类型QwtLogTransform对数、QwtNullTransform线性、
QwtPowerTransform指数，通过这三种转化器将log对数轴、线性轴、指数坐标系映射到绘图坐标系统。
在Qwt中，QwtScaleMap所占的地位是相当重要的，几乎涵盖了Qwt绘图系统的任何角落。

3、QwtScaleEngine 比例引擎
比例引擎分为：线性linear、对数Log（QwtLinearScaleEngine、QwtLogScaleEngine）。
用于在QwtPlot类setAxisScaleEngine函数中设置，设置坐标系中轴是log对数轴或linear线性轴。
一般不做修改，直接使用（QwtLogScaleEngine类中有最小刻度变成线性问题，需要修改来使用）。

4、QwtScaleDiv 比例划分-刻度
该类主要设置刻度，一个轴由上边界、下边界、主要或次要或中等的刻度组成。
其显示的刻度线通过setTicks函数、setInterval函数设置，刻度位置可以通过ticks函数获取出来。
一般设置步骤：
QwtPlot::setAxisScaleEngine(QwtPlot::xBottom, new QwtLinearScaleEngine);
QwtPlot::setAxisScaleDiv(QwtPlot::xBottom, LinearScaleDiv(xMin, xMax - xMin));
QwtPlot::setAxisScaleDiv的设置其实有三个类型接口，看头文件。

5、QwtPlot 绘制的视窗
QwtPlot继承自QFrame和QwtPlotDict，它是一个二维绘图部件，只是一个视图窗口，真正的绘图设备
是中心部件画布QwtPlotCanvas。画布上可以显示不限数量的基地图元项，图元项可以是QwtPlotCurve，
QwtPlotMarker，QwtPlotGrid或任意从QwtPlotItem派生出的子类；
QwtPlotItem决定自己被添加到哪个QwtPlot: attatch(xxx); detach();

6、QwtPlotCanvas 画布

给QwtPlot设置一个画布，画布设置前景色、背景色、有无焦点即可。

7、QwtPlotGrid
绘制网格线，通过走读源代码可知：绘制网格线的数据来自QwtScaleDiv::ticks函数（D指针传递）。
如果有特殊绘制样式，可以继承该Grid类，重新实现虚函数draw。

8、绘制曲线等
QwtPlotCurve 曲线图，各种样式
QwtPlotIntervalCurve 绘制两条曲线之间的间隔区域
QwtPlotHistogram 直方图，柱状统计图
QwtPlotSpectroCurve 三维散点图，用颜色表示Z轴
QwtPlotSvgItem 显示SVG格式图片数据的图元
QwtPlotSpectrogram 频谱图图元，继承自QwtPlotRasterItem显示栅格数据。
qwt例程效果图：https://blog.csdn.net/a826319028/article/details/11820157

9、QwtPlotMarker
QwtSymbol

10、QwtPolarPlot:

setAutoplot : 设置为false，只能通过replot 进行更新，默认为false。

setAzimuthOrigin: 设置方位角原点，

setScale ： 设置半径，方位角的刻度，可将起始角，结束角替换，达到逆转的效果。

11、QwtPolarPanner:

提供极坐标画布的平移

12、QwtPolarMagnifier：

提供画布的缩小或放大。

13、QwtText

  ***带有一系列属性的能够被文本引擎渲染的文本***

* QWtText::TextFormot:
  > AutoText:按顺序自己选择文本引擎，如果没有文本引擎，则选择QwtText::PlainText 


关键术语：
Plot：情景
Grid：网格
Canvas：画布
Curve：曲线
Axis：轴
Scale：比例尺、刻度
Legend：图例
Div：划分
Map：映射
radius : 半径
canvas：画布
origin： 起源
azimuth: 方位角
rectangle: 长方形，矩形。





