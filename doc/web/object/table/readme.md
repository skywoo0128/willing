# table 对象封装

支持antd5、antdv3、element ui2的table组件。支持Jexcel表格及各种输入。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/table/stuc.png "对象封装结构")

antd中固定表头的表格，和普通的表格有较大的区别，所以封装了一个普通表格，还封装了一个固定行列的表格。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

table组件的主要操作包括：
- 读取单元格值
- 选中记录
- 点击行记录中的按钮。
 
次要操作包括：
- 读取行记录
- 判断记录是否存在
- 修改单元格的值。

表格的操作比较复杂，需要先根据关键字（列名称和单元格值）查找行，然后再根据行编号和目标列的标题名称查找单元格。

树形表格能自动展开节点。

### antd5 

【[antd v5 的 table 组件](https://ant-design.antgroup.com/components/table-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/table/antd.gif "执行效果")

**自动化脚本**
```
WebGetCellValue==第一个表格==table.第一个表格==Name==Jim Green==Address==
WebTableButtonClick==table.第一个表格==Name==Jim Green==Action==table.Invite--Jim Green==
WebTableButtonClick==table.第一个表格==Name==Joe Black==Action==Delete==

WebRowSetSelected==table.可选表格==Name==Jim Green==true==
WebRowSetSelected==table.可选表格==Name==Joe Black==true==
WebRowSetSelected==table.可选表格==Name==Jim Green==false==

WebGetCellValue==带表头表格==table.带表头表格==Name==Jim Green==Cash Assets==
WebGetCellValue==树形Table==table.树形Table==Name==John Brown sr./John Brown jr./Jimmy Brown==Address==

WebGetCellValue==固定表头==table.固定表头==FullName=Edward 4==Column 2==
```


***

### antd vue 3

【[antd vue v3 的 table 组件](https://www.antdv.com/components/table-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/table/antdv.gif "执行效果")

**自动化脚本**
```
WebGetCellValue==基础表格==table.基础表格==Name==Jim Green==Address==
WebGetCellValue==基础表格==table.基础表格==Name==Jim Green==Age==
WebTableButtonClick==table.基础表格==Name==Jim Green==Action==table.Invite--Jim Green==
WebTableButtonClick==table.基础表格==Name==Joe Black==Action==Delete==

WebGetCellValue==带表头的表格==table.带表头的表格==Name==Jim Green==CashAssets==

WebGetCellValue==树形表格==table.树形表格==Name==John Brown sr./John Brown jr./Jimmy Brown==Age==
WebRowSetSelected==table.树形表格==Name==John Brown sr./John Brown==true==
WebRowSetSelected==table.树形表格==Name==John Brown sr./John Brown jr./Jimmy Brown==true==

WebGetCellValue==固定表头==table.固定表头==FullName==Edrward 4==Column 2==
```



***

### element ui 2

【[element ui 2 的 table 组件](https://element.eleme.cn/#/zh-CN/component/table)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/table/eui.gif "执行效果")


**自动化脚本**
```
WebDefPage==table==功能演示==table==
WebGetCellValue==数据表格==table.数据表格==日期==2016-05-04==地址==

WebDefPage==多选表格==功能演示==多选表格==
WebRowSetSelected==多选表格.多选表格==日期==2016-05-02==true==
WebRowSetSelected==多选表格.多选表格==日期==2016-05-08==true==
WebRowSetSelected==多选表格.多选表格==日期==2016-05-07==true==

WebDefPage==固定table==功能演示==固定table==
WebGetCellValue==固定表格==固定table.固定表格==日期==2016-05-02==地址==

WebDefPage==树形表格==功能演示==树形表格==
WebGetCellValue==树形表格值==树形表格.树形表格==日期==2016-05-01/2016-05-01==地址==
```

***

## 对象封装

公共方法参见WebNode和WebTable

table组件有两个版本，一个版本表头和表格内容在一个table中。另一个版本表头和表格内容在两个table中。

### Table和FixedTable

针对table组件的特殊封装包括：

| 名称 | 说明 |
| --- | --- |
| getTableData | 取表格的内容，用于行列操作 |
| setRowSelected | 查找行，然后选中check |

单元格的根据getTableData得到的数据定位。

