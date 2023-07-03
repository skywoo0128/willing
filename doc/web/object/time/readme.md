# time 对象封装

支持antd5、antdv3、element ui2的timepicker组件，选择时间和选择时间范围。数据是通过输入框输入，不是从弹出的时间选择器中选择

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/time/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

timepicker和timerange 组件的主要操作包括：
- 输入数据、
- 取数据；

需要设置时间格式，缺省格式为：hh:mm:ss

### antd5 

【[antd v5 的 timepicker 组件](https://ant-design.antgroup.com/components/time-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/time/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==time==表单组件==time==

# 选择时间
WebSetValue==time.请选择时间==%time.请选择时间%==
WebGetValue==请选择时间==time.请选择时间==
WebSetValue==time.请选择时间==%EMPTY%==

# 选择范围
WebSetValue==time.开始时间==%time.开始时间%==
WebGetValue==开始时间==time.开始时间==
WebSetValue==time.开始时间==%EMPTY%==
```


***

### antd vue 3

【[antd vue v3 的 timepicker 组件](https://www.antdv.com/components/time-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/time/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==time==表单==time==

# 选择时间
WebSetValue==time.选择时间==200310==
WebGetValue==选择时间==time.选择时间==
WebSetValue==time.选择时间==%EMPTY%==

# 选择范围
WebSetValue==time.时间范围==090310,103030==
WebGetValue==时间范围==time.时间范围==
WebSetValue==time.时间范围==%EMPTY%==
```



***

### element ui 2

【[element ui 2 的 timepicker 组件](https://element.eleme.cn/#/zh-CN/component/time-picker)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/time/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==timepicker==功能演示==timepicker==

# 选择时间
WebSetValue==timepicker.选择时间==091507==
WebGetValue==选择时间==timepicker.选择时间==

WebSetValue==timepicker.任意时间点==184607==
WebGetValue==任意时间点==timepicker.任意时间点==

# 选择范围
WebDefPage==timerange==功能演示==timerange==
WebSetValue==timerange.开始时间==200510;223003==
WebGetValue==开始时间==timerange.开始时间==
WebSetValue==timerange.开始时间==%EMPTY%==
```

***

## 对象封装

公共方法参见WebNode和WebObject

### timepicker

针对timepicker组件的特殊封装包括：setValue，getValue两个方法；没有封装isDisabled。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 查找对象
   WebObject webObj = getH5Object();
   
   // 判断值是否相同
   if( isComplete(webObj, value) ) {
      return;
   }
   
   // 清空
   if( value == null || value.isEmpty() ) {
      // 清空数据
      clear( webObj );
      return;
   }
   
   String fmtDate = DateUtil.formatTime( value, timeFormat );
   
   // 输入后，检查值是否一致
   for( int i=0; i<6; i++ ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 输入值
      doInput( webObj, fmtDate );
       
       // 检查值是否一致
       if( isEquals(webObj, value) ) {
          break;
       }
       
       MiscUtil.sleep( (i+1)*50 );
   }
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取日期的值 |
| getItem | 输入日期的对象 |
| getOpenItem | 打开下拉框的对象 |
| getClearItem | 清空下拉框的图标 |


### timerange

针对timerange组件的特殊封装包括：setValue，getValue两个方法；过程和TimePicker类似


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取日期的值 |
| getItem | 输入日期的对象 |
| getClearItem | 清空下拉框的图标 |



