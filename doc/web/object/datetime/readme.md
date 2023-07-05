# datetime 对象封装

支持antd5、antdv3、element ui2的datetimepicker组件，选择日期时间、或选择日期时间范围。数据是通过输入框输入，不是从弹出的日期选择器中选择

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/datetime/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

datetimepicker和datetimerange 组件的主要操作包括：
- 输入数据、
- 取数据；

需要配置时间格式，缺省格式：yyyy-MM-dd hh:mm:ss

### antd5 

【[antd v5 的 datetimepicker 组件](https://ant-design.antgroup.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/datetime/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单组件==date==

# 选择日期时间
WebSetValue==date.请选择日期时间==20230714-103953==
WebGetValue==请选择日期时间==date.请选择日期时间==

# 选择范围
WebSetValue==date.开始日期时间==20230714-103953,20240914-102053==
WebGetValue==开始日期时间==date.开始日期时间==

# 清空数据
WebSetValue==date.请选择日期时间==%EMPTY%==
WebSetValue==date.开始日期时间==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdDateTimePicker" desc="选择日期时间" className="cn.lz.web.plugin.antd.obj.AntdDateTimePicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>

<app-tag name="AntdDateTimeRange" desc="选择日期时间范围" className="cn.lz.web.plugin.antd.obj.AntdDateTimeRange" typeName="WebObject">
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

【[antd vue v3 的 datetimepicker 组件](https://www.antdv.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/datetime/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单==date==

# 选择日期时间
WebSetValue==date.请选择日期时间==20230714-103953==
WebGetValue==请选择日期时间==date.请选择日期时间==

# 选择范围
WebSetValue==date.开始日期时间==20230714-103953,20240914-102053==
WebGetValue==开始日期时间==date.开始日期时间==

# 清空数据
WebSetValue==date.请选择日期时间==%EMPTY%==
WebSetValue==date.开始日期时间==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdvDateTimePicker" desc="选择日期时间" className="cn.lz.web.plugin.antdv.obj.AntdvDateTimePicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>

<app-tag name="AntdvDateTimeRange" desc="选择日期时间范围" className="cn.lz.web.plugin.antdv.obj.AntdvDateTimeRange" typeName="WebObject">
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

【[element ui 2 的 datetimepicker 组件](https://element.eleme.cn/#/zh-CN/component/datetime-picker)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/datetime/eui.gif "执行效果")

**自动化脚本**
```
# 选择日期时间
WebDefPage==datetime==功能演示==datetime==
WebSetValue==datetime.默认==20230714-103953==
WebGetValue==默认==datetime.默认==

# 选择范围
WebSetValue==datetime.默认区间==20230714-113953,20240914-102054==
WebGetValue==默认区间==datetime.默认区间==
WebSetValue==datetime.默认区间==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="EuiDateTimePicker" desc="选择日期和时间" className="cn.lz.web.plugin.eui.obj.EuiDateTimePicker" typeName="WebObject">
   <node tag-name="div" class="el-date-editor--datetime">
      <node tag-name="input" class="el-input__inner"/>
   </node>
</app-tag>

<app-tag name="EuiDateTimeRange" desc="选择日期和时间区间" className="cn.lz.web.plugin.eui.obj.EuiDateTimeRange" typeName="WebObject">
   <node tag-name="div" class="el-date-editor--datetimerange">
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

### datetimepicker

针对datetimepicker组件的特殊封装包括：setValue，getValue两个方法；没有封装isDisabled。

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
   
   String fmtDate = DateUtil.formatDateTime( value, dateFormat );
   
   // 输入后，检查值是否一致
   for( int i=0; i<6; i++ ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 输入值
      doInput( webObj, fmtDate );
       
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


### datetimerange

针对datetimerange组件的特殊封装包括：setValue，getValue两个方法；过程和DateTimePicker类似


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取日期的值 |
| getItem | 输入日期的对象 |
| getClearItem | 清空下拉框的图标 |



