# month 对象封装

支持antd5、antdv3 的weekpicker组件，选择周和选择周范围。数据是通过输入框输入，不是从弹出的日期选择器中选择。

element ui2的weekpicker组件不支持输入框输入，暂时没有实现。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/week/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

weekpicker和weekrange 组件的主要操作包括：
- 输入数据、
- 取数据；

### antd5 

【[antd v5 的 weekpicker 组件](https://ant-design.antgroup.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/week/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单组件==date==

# 选择周
WebSetValue==date.请选择周==2023-9周==
WebGetValue==请选择周==date.请选择周==

# 选择范围
WebSetValue==date.开始周==2022-38周,2023-3周==
WebGetValue==开始周==date.开始周==

# 清空数据
WebSetValue==date.请选择周==%EMPTY%==
WebSetValue==date.开始周==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdWeekRange" desc="选择周范围" className="cn.lz.web.plugin.antd.obj.AntdWeekRange" typeName="WebObject">
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

<app-tag name="AntdWeekPicker" desc="选择周" className="cn.lz.web.plugin.antd.obj.AntdWeekPicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 weekpicker 组件](https://www.antdv.com/components/date-picker-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/week/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==date==表单==date==

# 选择周
WebSetValue==date.请选择周==2023-9周==
WebGetValue==请选择周==date.请选择周==

# 选择范围
WebSetValue==date.开始周==2022-38周,2023-3周==
WebGetValue==开始周==date.开始周==

# 清空数据
WebSetValue==date.请选择周==%EMPTY%==
WebSetValue==date.开始周==%EMPTY%==
```

对象匹配规则
```xml
<app-tag name="AntdvWeekRange" desc="选择周范围" className="cn.lz.web.plugin.antdv.obj.AntdvWeekRange" typeName="WebObject">
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

<app-tag name="AntdvWeekPicker" desc="选择周" className="cn.lz.web.plugin.antdv.obj.AntdvWeekPicker" typeName="WebObject">
   <node tag-name="div" class="ant-picker">
      <node tag-name="div" class="ant-picker-input">
         <node tag-name="input" type="text"/>
         <node tag-name="span" class="ant-picker-suffix">
            <node tag-name="span" class="anticon-calendar"/>
         </node>
      </node>
   </node>
</app-tag>
```



***

## 对象封装

公共方法参见WebNode和WebObject

### weekpicker

针对weekpicker组件的特殊封装包括：setValue，getValue两个方法；没有封装isDisabled。

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
   
   // 输入后，检查值是否一致
   for( int i=0; i<6; i++ ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 输入值
      doInput( webObj, value );
       
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


### weekrange

针对weekrange组件的特殊封装包括：setValue，getValue两个方法；过程和WeekPicker类似


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取日期的值 |
| getItem | 输入日期的对象 |
| getClearItem | 清空下拉框的图标 |



