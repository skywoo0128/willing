# tree 对象封装

支持antd5、antdv3、element ui2的tree组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tree/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

tree组件的主要操作包括：
- 点击目录节点
- 选中目录节点
- 取选中的节点。

次要操作包括：
- 点击节点中的按钮或图标。

只需要指定路径，组件能自动展开节点。

### antd5 

【[antd v5 的 tree 组件](https://ant-design.antgroup.com/components/tree-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tree/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==tree==数据显示==tree==
WebTreeSetChecked==tree.可选择的树==parent 1/parent 1-1/sss==true==
WebTreeIsChecked==isChecked==tree.可选择的树==parent 1/parent 1-1/sss==

WebTreeSetChecked==tree.可选择2==0-0/0-0-1/0-0-1-1,0-1/0-1-0-1==true==
WebTreeGetValue==可选择2==tree.可选择2==

WebTreeClick==tree.简单树==0-0/0-0-1/0-0-1-1==
```

对象匹配规则
```xml
<app-tag name="AntdTree" desc="树组件" className="cn.lz.web.plugin.antd.obj.AntdTree" typeName="WebTree">
   <node tag-name="div" class="ant-tree" role="tree">
      <node tag-name="div" class="ant-tree-list"/>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 tree 组件](https://www.antdv.com/components/tree-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tree/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==tree==数据显示==tree==
WebTreeSetChecked==tree.可选择的目录==parent 1/parent 1-1/sss==true==
WebTreeIsChecked==isChecked==tree.可选择的目录==parent 1/parent 1-1/sss==

WebTreeClick==tree.简单树==0-0/0-0-1/0-0-1-1==
WebTreeGetValue==简单树==tree.简单树==
```

对象匹配规则
```xml
<app-tag name="AntdvTree" desc="树组件" className="cn.lz.web.plugin.antdv.obj.AntdvTree" typeName="WebTree">
   <node tag-name="div" class="ant-tree" role="tree">
      <node tag-name="div" class="ant-tree-list"/>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 tree 组件](https://element.eleme.cn/#/zh-CN/component/tree)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tree/eui.gif "执行效果")

最后的增加和删除，执行速度太快，中间插入了300ms的延迟

**自动化脚本**
```
WebDefPage==tree==功能演示==tree==
WebTreeClick==tree.基本树==一级 2/二级 2-2/三级 2-2-1==
WebTreeGetValue==基本树==tree.基本树==

WebDefPage==tree选择==功能演示==tree选择==
WebTreeSetChecked==tree选择.可选择树==一级 1/二级 1-1/三级 1-1-2,一级 2/二级 2-2==true==
WebTreeIsChecked==isChecked==tree选择.可选择树==一级 1/二级 1-1/三级 1-1-2==

WebDefPage==tree内容==功能演示==tree内容==
WebTreeSetChecked==tree内容.自定义节点==一级 2/二级 2-2,一级 3/二级 3-2==true==

树节点中查找==tree内容.自定义节点==一级 2/二级 2-2==
WebClick==tree内容.树节点Append==

树节点中查找==tree内容.自定义节点==一级 2/二级 2-2==
WebClick==tree内容.树节点Append==

树节点中查找==tree内容.自定义节点==一级 2/二级 2-2==
WebClick==tree内容.树节点Append==

树节点中查找==tree内容.自定义节点==一级 3/二级 3-2==
WebClick==tree内容.树节点Delete==
```

对象匹配规则
```xml
<app-tag name="EuiTree" desc="树组件" className="cn.lz.web.plugin.eui.obj.EuiDataTree" typeName="WebTree">
   <node tag-name="div" class="el-tree" role="tree"/>
</app-tag>
```

***

## 对象封装

公共方法参见WebNode和WebTree

tree组件有两个版本，element ui实现的tree，采用父子节点方式表达层次关系。antd实现的tree，所有可见的节点是一个列表结构，采用缩进量表达层次关系。

### DataTree

针对DataTree组件的特殊封装包括：

| 名称 | 说明 |
| --- | --- |
| getTreeNodes | 取第一层节点 |
| getChildNodes | 取子节点 |
| getTreeNode | 查找第一层节点 |
| getChildNode | 查找子节点 |
| getSelectedNode | 取选中的节点 |

所有方法都是数据查询类的接口，操作类的接口都在底层的WebTree中实现，不需要扩展。

### IndentTree

针对IndentTree组件的特殊封装包括：

| 名称 | 说明 |
| --- | --- |
| getTreeNodes | 取第一层节点 |
| getChildNodes | 取子节点 |
| getTreeNode | 查找第一层节点 |
| getChildNode | 查找子节点 |
| getSelectedNode | 取选中的节点 |
| getRootNodes | 取第一层节点 |
| getAllNodes | 取所有树节点 |

IndentTree的封装比较简单，先读取所有树节点，然后按缩进量计算父子关系，生成一个完整的目录结构。

