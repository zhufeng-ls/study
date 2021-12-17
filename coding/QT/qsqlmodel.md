### QSqlQueryModel

提供了一个sql查询结果的只读数据模型。它从查询QSqlQueryModel获取数据。

可以方便的用于在QListView, QTableView, QTreeView等各种view上展示数据。

但它是只读的，不能编辑。

 

### QSqlTableMode

继承于QSqlQueryModel，与QSqlQueryModel功能相似。

比QSqlQueryModel的限制在于不能是任意sql语句，只是对单个数据表操作。

拓展在于在各种view上展示表格数据的同时，还允许用户进行编辑操作。