# Pagination 对象封装

支持antd5、antdv3、element ui2的pagination组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/paging/stuc.png "对象封装结构")

Pagination组件从WebObject派生。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Pagination组件的主要操作包括：
- setValue（页面跳转）；
- getValue（取当前页，总页数等）

setValue 的命令包括

| 名称 | 说明 |
| --- | --- |
| 前往 | 直接跳转到页 |
| 下一页 | 跳转到下一页 |
| 上一页 | 跳转到上一页 |
| 第一页 | 跳转到第一页 |
| 下一页 | 跳转到下一页 |
| 每页记录数 | 设置每页记录数 |


getValue的命令包括

| 名称 | 说明 |
| --- | --- |
| 总记录数 | 总记录数 |
| 总页数 | 总页数 |
| 每页记录数 | 每页记录数 |
| 当前页 | 当前页 |


### antd5 

【[antd v5 的 pagination 组件](https://ant-design.antgroup.com/components/pagination-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/paging/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==paging==导航==paging==
WebGetValue==总记录数2==paging.分页条==总记录数==
WebGetValue==总页数2==paging.分页条==总页数==
WebGetValue==每页记录数2==paging.分页条==每页记录数==
WebGetValue==当前页2==paging.分页条==当前页==

WebSetValue==paging.分页条==前往==7==
WebSetValue==paging.分页条==下一页==1==
WebSetValue==paging.分页条==上一页==1==
WebSetValue==paging.分页条==最后一页==1==
WebSetValue==paging.分页条==第一页==1==
WebSetValue==paging.分页条==每页记录数==20 条/页==
```


***

### antd vue 3

【[antd vue v3 的 pagination 组件](https://www.antdv.com/components/pagination-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/paging/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==paging==扩展组件==paging==
WebGetValue==总记录数2==paging.分页条==总记录数==
WebGetValue==总页数2==paging.分页条==总页数==
WebGetValue==每页记录数2==paging.分页条==每页记录数==
WebGetValue==当前页2==paging.分页条==当前页==

WebSetValue==paging.分页条==下一页==1==
WebSetValue==paging.分页条==上一页==1==
WebSetValue==paging.分页条==最后一页==1==
WebSetValue==paging.分页条==第一页==1==
WebSetValue==paging.分页条==每页记录数==10 条/页==
```



***

### element ui 2

【[element ui 2 的 pagination 组件](https://element.eleme.cn/#/zh-CN/component/pagination)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/paging/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==paging==功能演示==paging==
WebGetValue==总记录数2==paging.分页条==总记录数==
WebGetValue==总页数2==paging.分页条==总页数==
WebGetValue==每页记录数2==paging.分页条==每页记录数==
WebGetValue==当前页2==paging.分页条==当前页==

WebSetValue==paging.分页条==前往==3==
WebSetValue==paging.分页条==下一页==1==
WebSetValue==paging.分页条==上一页==1==
WebSetValue==paging.分页条==最后一页==1==
WebSetValue==paging.分页条==第一页==1==
WebSetValue==paging.分页条==每页记录数==200 条/页==
```

封装element ui组件时，出现了一个意外，通过WebElement.clear和sendkeys时，会产生下面的现象。

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/paging/eui2.gif "执行效果")

这是放慢四倍的效果，调用WebElement.clear后，输入框被强制设置成了1，所以输入3变成了13。幸好selenide也支持通过js修改值。脚本如下

```
WebSetValue==paging.分页条==前往==3==
```

***

## 对象封装

### pagination

公共方法参见WebNode和WebObject

针对Pagination组件的特殊封装包括：setValue，getValue两个方法。

setValue如果是跳转，找到输入框并直接修改。其他情况时找到按钮并点击。


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取操作对象，上一页、下一页等 |
| getValue | 取当前页等 |





