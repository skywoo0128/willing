# DropDown 对象封装

支持antd5、antdv3、element ui2的slider组件，根据外观不同，封装了两种对象：DropDownButton、DropDownLink。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/dropdown/stuc.png "对象封装结构")

DropDown组件从WebObject派生，可以是按钮现状，也可以是链接现状。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

DropDown组件的主要操作包括：
- setValue 点击按钮，然后选择下拉菜单项
- click 点击按钮
- getOptions 取下拉菜单项列表


### antd5 

【[antd v5 的 dropdown 组件](https://ant-design.antgroup.com/components/dropdown-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/dropdown/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==dropdown==导航==dropdown==
WebSetValue==dropdown.Hoverme==1st menu item==
SwitchToWindow==0==

WebSetValue==dropdown.Dropdown==2nd menu item==
WebObjectClick==dropdown.Dropdown==
```


***

### antd vue 3

【[antd vue v3 的 dropdown 组件](https://www.antdv.com/components/dropdown-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/dropdown/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==dropdown==导航==dropdown==
WebSetValue==dropdown.Hoverme==1st menu item==
SwitchToWindow==0==

WebSetValue==dropdown.Dropdown==2nd menu item==
WebObjectClick==dropdown.Dropdown==
```



***

### element ui 2

【[element ui 2 的 dropdown 组件](https://element.eleme.cn/#/zh-CN/component/dropdown)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/dropdown/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==dropdown==功能演示==dropdown==
WebSetValue==dropdown.更多菜单==螺蛳粉==

WebDefPage==dropdown2==功能演示==dropdown2==
WebSetValue==dropdown2.下拉菜单==螺蛳粉==
WaitAutoCloseHint==click on item c==3000==
```

***

## 对象封装

### DropDownButton、DropDownLink

公共方法参见WebNode和WebObject，两个组件的方法相同

针对Dropdown组件的特殊封装包括：setValue，click，getOptions两个方法。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 查找对象
   WebObject webObj = getH5Object();
   
   // 打开弹出菜单
   dropDown( webObj );
   
   // 选择
   doSelect( webObj, value );
}

// 选择弹出菜单项
protected void doSelect( WebObject webObj, String value )
{
   String jsFile = getMenuItemScript();
   
   List<String> list = StringUtil.split( value,  '/' );
   int count = list.size();
   for( int i=0; i<count; i++ ) {
      // 点击菜单
      ElementByScript byScript2 = new ElementByScript( jsFile, null, i, list.get(i) );
      WebUtil.dynItemClick( byScript2, false );
      
      if(i != count-1 ) {
         MiscUtil.sleep( 5 );
      }
   }
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取按钮信息，包括左侧按钮和右侧下箭头 |
| getPopupWindow | 取弹出窗口 |
| getMenuItem | 弹出菜单中取对象 |
| getOptions | 取弹出菜单的选项 |





