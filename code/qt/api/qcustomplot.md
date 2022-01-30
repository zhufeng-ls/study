## 记录:

```
QStyleOption类型的变量option的作用：

QStyleOption类的作用就是用于显示用的。该类的成员变量有，QRect，QFont等。都是用于控制item的显示使用的。

QRect，用于控制item的大小。

QFont，用于控制item的字体。

QPalette，用于控制item的颜色。
```



----

## QDelegate



### 	createEditor

```
该控件用于编辑model中的数据。
若我想让第二列显示editor控件。
f(   index.column() == 2  )

{
QButton *btn = new QButton( parent );

}

else

{
return   QItemDelegate::creatorEditor(   parent,option,index   );

}
```

### setEditorData

```
从model中获取数据，并赋值给editorWidget ，和 createEditorWidget 配套使用
```

### setModelData

```
从EditorWidget中获取数据,并将数据放置到模型中
```





-----

## QModel

```
自定义模型的基本原理及步骤如下
①、数据：实际数据可使用QList、数组、整型、或单独的一个类来保存，数据可存放在模型中，也可存放在文件等其他地方。
②、columnCout()、rowCount()、index()、parent()这4个函数用于共同设计模型的结构，因为使用索引表示模型中的某个数据项的位置，因此设计模型索引的结构就是设计模型的结构。具何如下
 行数和列数的设计：比如对于3行4列的表格结构，columnCout()应返回4，rowCount()应返回3；对于列表结构，则因为列表只需要1列，所以columncout()应总是返回1，rowCount()返回该列表的行数；对于树形结构模型，则更复杂，需要根据当前父节点的情况进行判断，以返回该父节点拥有的列数和行数。
```



### data(const QModelIndex &index, int role)

```
返回模型中给定索引的数据，如果没有值，则返回一个无效的QVariant(),而不是返回0.
```

### rowCount(const QModelIndex &parent)

```
返回行数，在treeView中，parent 返回是当前选中行在当前父节点中的排行，从下标0开始
```

### columnCount(const QModelIndex &parent)

```
返回列数
```

### flags(const QModelIndex &index)

```
返回被给予索引项的属性，即他们是否能被编辑，是否能被拖动
```

### parent

```
因为表格结构中的所有单元格都属于同一个父索引之下，所以可把所有单元格都视为顶级节点，因此他们的父索引可以以无效模型索引作为父索引，因此parent()可以返回一个无效模型索引；对于列表结构的模型，同样只需返回一个无效模型索引即可；对树形结构模型，此步骤比较复杂，可以通过获取当前节点的父节点及其行号和列号，然后使用createIndex()创建该父节点的索引。
```

### index

```
index()函数的设计：该函数用于为模型中的每个数据项创建索引，创建索引需要使用createIndex()函数，对于表格结构，只需向createIndex()函数传递当前数据项所在的行号、列号及使用的数据的指针即可；对于列表结构，则列号始终为0，其余同表格结构；对于树形结构，需要向该函数传递当前数据项位于父索引中的行号、列号及使用的数据的指针。

```

### data

```
data()函数的返回值决定了视图上应显示的数据，也就是说在界面上用户看到的数据是由该函数返回的，若返回不当的值，则数据无法正常显示在视图上，下面以使用内置的标准视图类为例来讲解怎样设计此函数。data()函数会被视图类调用多次，视图每次都会向data传递一个不同的role(角色)参数值，然后视图根据data返回的值，设置该role的数据， 因此在设计data函数的返回值时，需要根据role的不同值返回不同的数据，以使视图正确的显示。
```

### insertRows(int row, int count, const QModelIndex &parent)

```
parent 是父级在父父级中的所有位置，从0开始0
```



### drag 和drop

```
drag为拖动操作，drop为放置操作，当拖动时，需要把拖动的数据进行存储，存储数据这一步骤称为编码，数据以MIME类型(见6.6节)存储为QMimeData类型的对象(称为放置数据)，当执行放下操作时，需要对放置数据进行处理，若需要使用这些数据，则需要把存储的数据读取出来(即解码)，然后进行处理；当然，若不需要这些放置数据，也可以直接丢弃。
```

```
1、自定义拖放操作的步骤
(1)、启用视图的拖放支持
标准视图自动支持内部拖放，默认情况下，这些视图的拖放功能未启用，要启用视图的拖放支持，需进行以下设置：
 设置视图的dragEnabled属性为true以启用项目拖放。
 拖放的模式：即视图是否支持拖放，是否接受放置数据，是否接受来自自身的移动操作等。需要设置视图的dragDropMode属性。
 若要允许用户在视图内放置内部或外部项目，需将视图的viewport()的acceptDrops属性设置为true。
 若要向用户显示拖动的项目放置位置(通常该位置会以不同的形式显示，比如以阴影形式显示等)，此时需设置视图的showDropIndicator属性。

v->setDragEnabled(1);           		//启用视图的拖放支持
v->setAcceptDrops(1); 				//接受放置数据
v->setDropIndicatorShown(1);		//显示放置位置
v->setDragDropMode(QAbstractItemView::DragDrop);   //设置拖放模式为支持拖放
(2)、启用数据项的拖放支持
重新实现QAbstractItemModel::flags()函数以提供合适的标志来指示哪些项目可以被拖动，哪些项目将接受放置(Drop)。
(3)、编码数据
重新实现QAbstractItemModel::mimeData()函数，把编码后的数据保存在该函数返回的QMimeData对象中。
(4)、 处理放置数据
1)、内置模型对放置数据的处理
不同类型的模型以不同的方式处理放置数据。列表模型和表格模型只提供了存储数据项的平面结构。因此，他们可能会在数据放入视图中的现有项目时插入新行（和列），或者可能会使用提供的一些数据覆盖模型中项目的内容。树模型通常能够将包含新数据的子项添加到其基础数据存储中，因此就用户而言，其行为将更具可预测性。
2)、对放置数据的自行处理
通过重新实现QAbstractItemModel::dropMimeData()函数来处理放置数据，此时需要对放置数据进行解码(即读取出QMimeData对象存储的数据的内容)，并将其插入模型的底层数据结构中(或进行其他处理)，若该函数修改了数据项或模型的尺寸，则必须注意确保发出所有相关的信号。因为该函数需要插入或删除等操作，所以简单的调用QAbstractItemModel子类中的已经实现了的setData()，insertRows()和insertColumns()等函数会更方便。另外，还可以使用dropMimeData()函数的默认实现来处理放置数据，dropMimeData()函数的默认实现不会覆盖模型中的任何数据，它会将放置数据作为项目的同胞插入，或者作为该项目的子项插入。若要使用该函数的默认实现，就需要重新实现以下函数：insertRows()、insertColumns()、setData()、setItemData()。
```

```
2、自定义拖放操作的其他设置
(1)、模型能接受的拖放操作的类型
重新实现QAbstractItemModel::supportedDropActions() 函数可以指示模型能接受的拖放操作的类型(比如移动、复制等)，虽然可以给出Qt::DropActions的值的任意组合，但需要编写代码来支持它们，比如，对于Qt::MoveAction(移动)操作，则模型就需要提供QAbstractItemModel :: removeRows()的实现。
(2)、对多种MIME类型的支持
默认情况下，内置模型和视图只支持一种MIME类型，即
application/x-qabstractitemmodeldatalist。
若要让模型支持多种MIME类型，则需要重新实现QAbstractItemModel::mimeTypes()，该函数返回拖放操作时模型可以处理的MIME类型列表，注意，自定义数据类型必须声明为元对象。
(3)、禁止在项目上放置数据
重新实现QAbstractItemModel:: canDropMimeData()函数可禁止在项目上放置数据。
```

### mineData

```
把编码后的数据保存在该函数返回的QMimeData对象中
```

### dropMimeData

```
处理放置数据，此时需要对放置数据进行解码(即读取出QMimeData对象存储的数据的内容)，并将其插入模型的底层数据结构中(或进行其他处理)，若该函数修改了数据项或模型的尺寸，则必须注意确保发出所有相关的信号
```

### dragMineData

```
处理拖放数据
```

### supportedDropActions

```
示模型能接受的拖放操作的类型(比如移动、复制等)
```

### mimeTypes

```
拖放操作时模型可以处理的MIME类型列表
```

### headerData(int section, Qt::Orientation orientation, int role)

```
设置tableView 的表格头，orientation表示是行头还是列头，role表示通道，表示设置或获取对应通道的数据
```

### beginResetData  、endResetData

```
开始重置模型数据，结束重置模型数据，重置后，整个模型都会刷新
```

