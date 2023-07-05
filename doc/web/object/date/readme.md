# date 对象封装

支持antd5、antdv3、element ui2的datepicker组件，选择日期和选择范围。数据是通过输入框输入，不是从弹出的日期选择器中选择

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/date/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

datepicker和daterange 组件的主要操作包括：
- 输入数据、
- 取数据；

### antd5 

【[antd v5 的 datepicker 组件](https://ant-design.antgroup.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/date/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单组件==date==

# 选择日期
WebSetValue==date.请选择日期==20200903==
WebGetValue==请选择日期==date.请选择日期==

# 选择范围
WebSetValue==date.开始日期==20240401,20240901==
WebGetValue==开始日期==date.开始日期==

# 清空数据
WebSetValue==date.请选择日期==%EMPTY%==
WebSetValue==date.开始日期==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdDatePicker" desc="选择日期" className="cn.lz.web.plugin.antd.obj.AntdDatePicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>

<app-tag name="AntdDateRange" desc="选择日期范围" className="cn.lz.web.plugin.antd.obj.AntdDateRange" typeName="WebObject">
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

【[antd vue v3 的 datepicker 组件](https://www.antdv.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/date/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单==date==

# 选择日期
WebSetValue==date.请选择日期==20200903==
WebGetValue==请选择日期==date.请选择日期==

# 选择范围
WebSetValue==date.开始日期==20240401,20240901==
WebGetValue==开始日期==date.开始日期==

# 清空数据
WebSetValue==date.请选择日期==%EMPTY%==
WebSetValue==date.开始日期==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdvDatePicker" desc="选择日期" className="cn.lz.web.plugin.antdv.obj.AntdvDatePicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>

<app-tag name="AntdvDateRange" desc="选择日期范围" className="cn.lz.web.plugin.antdv.obj.AntdvDateRange" typeName="WebObject">
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

【[element ui 2 的 datepicker 组件](https://element.eleme.cn/#/zh-CN/component/date-picker)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/date/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==DatePicker==功能演示==DatePicker==

# 选择日期
WebSetValue==DatePicker.选择日期==%EMPTY%==
WebSetValue==DatePicker.选择日期==20200829==
WebGetValue==选择日期==DatePicker.选择日期==

WebSetValue==DatePicker.选择日期2==20220119==

// 复杂格式
WebDefPage==DatePicker2==功能演示==DatePicker2==
WebSetValue==DatePicker2.选择日期2==20200806==
WebGetValue==选择日期2==DatePicker2.选择日期2==

// 选择范围
WebDefPage==daterange==功能演示==daterange==
WebSetValue==daterange.日期范围==20200806,20200916==
WebSetValue==daterange.日期范围==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="EuiDatePicker" desc="选择日期" className="cn.lz.web.plugin.eui.obj.EuiDatePicker" typeName="WebObject">
   <node tag-name="div" class="el-date-editor--date">
      <node tag-name="input" type="text" class="el-input__inner"/>
      <node tag-name="span" class="el-input__prefix">
         <node tag-name="i" class="el-icon-date"/>
      </node>
   </node>
</app-tag>

<app-tag name="EuiDateRange" desc="选择日期区间" className="cn.lz.web.plugin.eui.obj.EuiDateRange" typeName="WebObject">
   <node tag-name="div" class="el-date-editor--daterange">
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

### datepicker

针对datepicker组件的特殊封装包括：setValue，getValue两个方法；没有封装isDisabled。

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
   
   // 清空
   if( value == null || value.isEmpty() ) {
      // 清空数据
      clear( webObj );
      return;
   }
   
   // 格式化日期
   String fmtDate = DateUtil.formatDate( value, dateFormat );
   
   // 输入后，检查值是否一致
   for( int i=0; i<6; i++ ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 输入值
      doInput( webObj, fmtDate );
       
       // 检查值是否一致
       if( isComplete(value) ) {
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


### daterange

针对daterange组件的特殊封装包括：setValue，getValue两个方法；过程和DatePicker类似


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取日期的值 |
| getItem | 输入日期的对象 |
| getClearItem | 清空下拉框的图标 |



