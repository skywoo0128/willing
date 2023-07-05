# month 对象封装

支持antd5、antdv3、element ui2的datepicker组件，选择月份和选择月范围。数据是通过输入框输入，不是从弹出的日期选择器中选择

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/month/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

monthpicker和monthrange 组件的主要操作包括：
- 输入数据、
- 取数据；

### antd5 

【[antd v5 的 monthpicker 组件](https://ant-design.antgroup.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/month/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单组件==date==

# 选择月份
WebSetValue==date.请选择月份==202907==
WebGetValue==请选择月份==date.请选择月份==

# 选择范围
WebSetValue==date.开始月份==202402,202501==
WebGetValue==开始月份==date.开始月份==

# 清空数据
WebSetValue==date.请选择月份==%EMPTY%==
WebSetValue==date.开始月份==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdMonthPicker" desc="选择月" className="cn.lz.web.plugin.antd.obj.AntdMonthPicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>

<app-tag name="AntdMonthRange" desc="选择月范围" className="cn.lz.web.plugin.antd.obj.AntdMonthRange" typeName="WebObject">
   <node tag-name="div" class="ant-picker-range">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
      </node>
      <node tag-name="div" class="ant-picker-input"/>
      <node tag-name="span" class="ant-picker-suffix">
         <node tag-name="span" class="anticon-calendar"/>
      </node>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 monthpicker 组件](https://www.antdv.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/month/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单==date==

# 选择月份
WebSetValue==date.请选择月份==202907==
WebGetValue==请选择月份==date.请选择月份==

# 选择范围
WebSetValue==date.开始月份==202402,202501==
WebGetValue==开始月份==date.开始月份==

# 清空数据
WebSetValue==date.请选择月份==%EMPTY%==
WebSetValue==date.开始月份==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdvMonthPicker" desc="选择月" className="cn.lz.web.plugin.antdv.obj.AntdvMonthPicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>

<app-tag name="AntdvMonthRange" desc="选择月范围" className="cn.lz.web.plugin.antdv.obj.AntdvMonthRange" typeName="WebObject">
   <node tag-name="div" class="ant-picker-range">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
      </node>
      <node tag-name="div" class="ant-picker-input"/>
      <node tag-name="span" class="ant-picker-suffix">
         <node tag-name="span" class="anticon-calendar"/>
      </node>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 monthpicker 组件](https://element.eleme.cn/#/zh-CN/component/date-picker)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/month/eui.gif "执行效果")

**自动化脚本**
```
# 选择月份
WebDefPage==DatePicker==功能演示==DatePicker==
WebSetValue==DatePicker.选择月==202209==
WebGetValue==选择月==DatePicker.选择月==

# 选择范围
WebDefPage==daterange==功能演示==daterange==
WebSetValue==daterange.月份范围==202209,202211==
WebGetValue==月份范围==daterange.月份范围==
WebSetValue==daterange.月份范围==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="EuiMonthPicker" desc="选择月" className="cn.lz.web.plugin.eui.obj.EuiMonthPicker" typeName="WebObject">
   <node tag-name="div" class="el-date-editor--month">
      <node tag-name="input" type="text" class="el-input__inner"/>
      <node tag-name="span" class="el-input__prefix">
         <node tag-name="i" class="el-icon-date"/>
      </node>
   </node>
</app-tag>

<app-tag name="EuiMonthRange" desc="选择月份区间" className="cn.lz.web.plugin.eui.obj.EuiMonthRange" typeName="WebObject">
   <node tag-name="div" class="el-date-editor--monthrange">
      <node tag-name="i" class="el-range__icon"/>
      <node tag-name="input" type="text" class="el-range-input"/>
      <node tag-name="span" class="el-range-separator"/>
      <node tag-name="input" type="text" class="el-range-input"/>
   </node>
</app-tag>
```

***

## 对象封装

公共方法参见WebNode和WebObject

### monthpicker

针对monthpicker组件的特殊封装包括：setValue，getValue两个方法；没有封装isDisabled。

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
   
   String fmtMonth = DateUtil.formatMonth( value, dateFormat );
   
   // 输入后，检查值是否一致
   for( int i=0; i<6; i++ ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 输入值
      doInput( webObj, fmtMonth );
       
       // 检查值是否一致
       if( isComplete(webObj, value) ) {
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


### monthrange

针对monthrange组件的特殊封装包括：setValue，getValue两个方法；过程和MonthPicker类似


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取日期的值 |
| getItem | 输入日期的对象 |
| getClearItem | 清空下拉框的图标 |



