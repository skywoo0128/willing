# tree select 对象封装

支持antd5、antdv3的treeselect组件，vue tree select组件。单选和多选。

对象封装结构

TreeSelect和TreeSelects

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/treeselect/stuc.png "对象封装结构")

弹出下拉框的树组件

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/treeselect/stuc2.png "对象封装结构")

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

treeSelect组件的主要操作包括：
- 选择单个数据、
- 选择多个数据
- 取数据；

次要操作包括：
- 取下拉列表的值。

### antd5 

【[antd v5 的 tree-select 组件](https://ant-design.antgroup.com/components/select-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/treeselect/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==treeselect==表单组件==treeselect==
WebSetValue==treeselect.单选择框==parent1/parent1-0/leaf1==
WebGetValue==单选择框==treeselect.单选择框==

WebSetValue==treeselect.复选框==parent1/parent1-0/myleaf,parent1/parent1-0/yourleaf==
WebGetValue==复选框==treeselect.复选框==

WebSetValue==treeselect.复选框2==Node1/Child Node1,Node2/Child Node4==
WebGetValue==复选框2==treeselect.复选框2==
```


***

### antd vue 3

【[antd vue v3 的 tree-select 组件](https://www.antdv.com/components/tree-select-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/treeselect/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==treeselect==表单==treeselect==
WebSetValue==treeselect.单选择框==parent1/parent1-0/your leaf==
WebGetValue==单选择框==treeselect.单选择框==

WebSetValue==treeselect.复选框==parent1/parent1-0/myleaf,parent1/parent1-0/yourleaf==
WebGetValue==复选框==treeselect.复选框==

WebSetValue==treeselect.复选框2==Node1/Child Node1,Node2/Child Node4==
WebGetValue==复选框2==treeselect.复选框2==
```



***

### element ui 2

【[vue tree select 组件](https://www.vuetreeselect.cn/)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/treeselect/vuets.gif "执行效果")

**自动化脚本**
```
WebDefPage==treemenu==功能演示==treemenu==
WebSetValue==treemenu.树形选择框==A/AA,B/BB,B/BA==
WebGetValue==树形选择框==treemenu.树形选择框==

WebSetValue==treemenu.单选==Asia/Bahrain==
WebGetValue==单选==treemenu.单选==
```

***

## 对象封装

公共方法参见WebNode和WebObject

### tree-select

针对tree-select组件的特殊封装包括：setValue，getValue和getOptions三个方法；没有封装isDisabled。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 判断值是否相同
   if( isComplete(value) ) {
      return;
   }
   
   // 查找对象
   WebObject webObj = getH5Object();
   
   // 清空
   if( value == null || value.isEmpty() ) {
      clear( webObj );
      return;
   }
   
   int retry = 0;
   while( retry++ < 6 ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 等待下拉框打开
      int delay = (retry - 1) * 70;
      if( delay > 0 ) {
         MiscUtil.sleep( delay );
      }
      
      // 选择
      doSelect( webObj, value );
      
      // 比较结果
      if( isComplete(value) ) {
         return;
      }
   }
   
   throw new LzException( "E908", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取下拉框的值 |
| getOpenItem | 打开下拉框的图标 |
| getClearItem | 清空下拉框的图标 |


### tree-selects

针对tree-selects组件的特殊封装包括：setValue，getValue和getOptions三个方法；没有封装isDisabled。setValue支持随机选择（比如随机选择省市县等）

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 转换数据
   List<String> values = StringUtil.split(value, ',', ';');
   
   // 查找对象
   WebObject webObj = getH5Object();
   
   // 判断值是否相同
   if( isComplete(values) ) {
      return;
   }
   
   // 需要错误重试
   int retry = 0;
   while( retry++ < 6 ) {
      // 先清空
      clear( webObj );
      if( value == null || value.isEmpty() ) {
         return;
      }
      
      // 打开下拉框
      dropDown( webObj );
      
      // 等待下拉框打开
      int delay = (retry - 1) * 70;
      if( delay > 0 ) {
         MiscUtil.sleep( delay );
      }
      
      // 选择
      doSelect( webObj, value );
      
      // 是否已输入
      if( isComplete(values) ) {
         // 关闭弹窗
         WebClient.pressEscape();
         
         return;
      }
   }
   
   throw new LzException( "E908", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItems | 取选中的值(Tags) |
| getOpenItem | 打开下拉框的图标 |
| getClearItem | 清空下拉框的图标 |


### TreeSelectMenu

TreeSelectMenu是一个WebTree组件，封装包括：

| 名称 | 说明 |
| --- | --- |
| getTreeNodes | 取第一层节点 |
| getChildNodes | 取子节点 |
| getTreeNode | 查找第一层节点 |
| getChildNode | 查找子节点 |
| getSelectedNode | 取选中的节点 |

所有方法都是数据查询类的接口，操作类的接口都在底层的WebTree中实现，不需要扩展。



