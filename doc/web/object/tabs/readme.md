# tabs/tab 对象封装

支持antd5、antdv3、element ui2的tabs组件，包括两个组件：页签组和页签。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tabs/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

tabs组件的主要操作包括：
- 选择页签、
- 取当前选中的页签；

次要操作包括：
- 取所有页签。
- 增加页签
- 删除页签

setValue支持二级命令，包括：删除，预览。

| 指令 | 说明 |
| --- | --- |
| `WebSetValue==UI对象==页签名称==` | 选择页签 |
| `WebSetValue==UI对象==close==页签名称==` | 删除页签 |
| `WebSetValue==UI对象==add==页签名称==` | 增加页签 |


### antd5 

【[antd v5 的 tabs 组件](https://ant-design.antgroup.com/components/tabs-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tabs/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==tabs==数据显示==tabs==
WebSetValue==tabs.Tab1==Tab2==

WebSetValue==tabs.Tab21==Tab2==
WebSetValue==tabs.Tab21==add==Tab5==
WebSetValue==tabs.Tab21==close==Tab2==
WebSetValue==tabs.Tab21--Tab1==close==
```


***

### antd vue 3

【[antd vue v3 的 tabs 组件](https://www.antdv.com/components/tabs-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tabs/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==tabs==导航==tabs==
WebSetValue==tabs.Tab1==Tab2==
WebGetValue==Tab1==tabs.Tab1==

WebSetValue==tabs.Tab12==Tab2==
WebSetValue==tabs.Tab12==add==Tab5==
WebSetValue==tabs.Tab12==close==Tab2==
WebSetValue==tabs.Tab12--Tab1==close==
WebGetValue==Tab12==tabs.Tab12==
```



***

### element ui 2

【[element ui 2 的 tabs 组件](https://element.eleme.cn/#/zh-CN/component/tabs)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tabs/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==tabs==功能演示==tabs==
WebSetValue==tabs.用户管理==角色管理==
WebGetValue==用户管理==tabs.用户管理==

WebSetValue==tabs.用户管理2==角色管理==
WebGetValue==用户管理2==tabs.用户管理2==

WebDefPage==tabs2==功能演示==tabs2==
WebSetValue==tabs2.Tab1==add==Tab4==
WebSetValue==tabs2.Tab1==Tab 1==
WebSetValue==tabs2.Tab1--Tab1==close==
```

***

## 对象封装

公共方法参见WebNode和WebObject

### tabs 页签组

针对tabs组件的特殊封装包括：setValue，getValue和getOptions三个方法；没有封装isDisabled。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 查找对象
   WebObject webObj = getH5Object();
   
   // 判断值是否相同
   if( isComplete(value) ) {
      return;
   }
   
   // 选择
   String jsFile = getItemScript();
   
   for( int retry=0; retry<5; retry++ ) {
      ElementByScript byScript = new ElementByScript( jsFile, webObj, value );
      WebFactory.$(byScript).click();
      
      if( isComplete(value) ) {
         return;
      }
      
      MiscUtil.sleep( (retry+1)*50 );
   }
   
   throw new LzException( "E908", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 页签中取对象 |
| getOptions | 取页签的所有选项 |
| getValue | 取选中的页签 |
| getCloseIcon | 取关闭页签的对象 |
| getAddIcon | 取增加页签的按钮 |


### tab 单个页签

针对selects组件的特殊封装包括：click

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取页签项信息 |



