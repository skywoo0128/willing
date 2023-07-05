# input 对象封装

支持antd5、antdv3、element ui2的input组件，封装了前后带图标和输入组件的组合对象。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/input/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。封装后包含两个对象，InputAffix（前后带图标）、InputGroup（前后带输入框）

input组件的主要操作包括：
- 输入数据、
- 取数据；
- 前后输入框中输入数据

次要操作包括：
- 点击前后图标。

可以独立操作以下部件：左侧插件，右侧插件，左侧图标，右侧图标

### antd5 

【[antd v5 的 input 组件](https://ant-design.antgroup.com/components/input-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/input/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==input==表单组件==input==
WebSetValue==input.第一个==第一个==
WebGetValue==第一个==input.第一个==

WebSetValue==input.第二个==第二个==
WebGetValue==第二个==input.第二个==

WebSetValue==input.第二个--左侧插件==https://==
WebGetValue==第二个==input.第二个--左侧插件==

WebSetValue==input.第二个--右侧插件==.jp==
WebGetValue==第二个==input.第二个--右侧插件==

WebSetValue==input.largesize==largesize==
WebGetValue==largesize==input.largesize==

WebSetValue==input.第4个==%input.第4个%==
```

对象匹配规则
```xml
<app-tag name="AntdInputGroup" desc="组合输入框" className="cn.lz.web.plugin.antd.obj.AntdInputGroup" typeName="WebObject">
   <node tag-name="span" class="ant-input-group">
      <node tag-name="input" class="ant-input"/>
   </node>
</app-tag>

<app-tag name="AntdInputGroup2" desc="组合输入框" className="cn.lz.web.plugin.antd.obj.AntdInputGroup" typeName="WebObject">
   <node tag-name="span" class="ant-input-group">
      <node tag-name="span" class="ant-input-affix-wrapper">
         <node tag-name="input" class="ant-input"/>
      </node>
   </node>
</app-tag>

<app-tag name="AntdInputAffix" desc="带图标的输入框" className="cn.lz.web.plugin.antd.obj.AntdInputAffix" typeName="WebObject">
   <node tag-name="span" class="ant-input-affix-wrapper">
      <node tag-name="input" class="ant-input"/>
   </node>
</app-tag>

<app-tag name="AntdInputCompact" desc="紧凑型组合输入框" className="cn.lz.web.plugin.antd.obj.AntdInputCompact" typeName="WebObject">
   <node tag-name="span" class="ant-input-group-compact"/>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 input 组件](https://www.antdv.com/components/input-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/input/antdv.gif "执行效果")

**自动化脚本**
```
WebSetValue==input.组合输入框1==组合输入框1==
WebGetValue==第一个==input.组合输入框1==

WebSetValue==input.组合输入框2==组合输入框2==
WebGetValue==第二个==input.组合输入框2==

WebSetValue==input.组合输入框2--左侧插件==Https://==
WebGetValue==第二个==input.组合输入框2--左侧插件==

WebSetValue==input.组合输入框2--右侧插件==.jp==
WebGetValue==第二个==input.组合输入框2--右侧插件==

WebSetValue==input.前后缀输入框==前后缀输入框==
WebGetValue==前后缀输入框==input.前后缀输入框==
```

对象匹配规则
```xml
<app-tag name="AntdvInputGroup" desc="组合输入框" className="cn.lz.web.plugin.antdv.obj.AntdvInputGroup" typeName="WebObject">
   <node tag-name="span" class="ant-input-group">
      <node tag-name="input" class="ant-input"/>
   </node>
</app-tag>

<app-tag name="AntdvInputGroup2" desc="组合输入框" className="cn.lz.web.plugin.antdv.obj.AntdvInputGroup" typeName="WebObject">
   <node tag-name="span" class="ant-input-group">
      <node tag-name="span" class="ant-input-affix-wrapper">
         <node tag-name="input" class="ant-input"/>
      </node>
   </node>
</app-tag>

<app-tag name="AntdvInputAffix" desc="带图标的输入框" className="cn.lz.web.plugin.antdv.obj.AntdvInputAffix" typeName="WebObject">
   <node tag-name="span" class="ant-input-affix-wrapper">
      <node tag-name="input" class="ant-input"/>
   </node>
</app-tag>

<app-tag name="AntdvInputCompact" desc="紧凑型组合输入框" className="cn.lz.web.plugin.antdv.obj.AntdvInputCompact" typeName="WebObject">
   <node tag-name="span" class="ant-input-group-compact"/>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 input 组件](https://element.eleme.cn/#/zh-CN/component/input)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/input/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==input==功能演示==input==
WebSetValue==input.复合输入框==复合输入框==
WebGetValue==com==input.复合输入框==

WebSetValue==input.复合输入框--左侧插件==订单号==
WebGetValue==com==input.复合输入框--左侧插件==

WebSetValue==input.请输入内容==请输入内容==
WebGetValue==请输入内容==input.请输入内容==

WebDefPage==input2==功能演示==input2==
WebSetValue==input2.请选择日期==20050928==
WebGetValue==请选择日期==input2.请选择日期==
```

对象匹配规则
```xml
<app-tag name="EuiInputGroup" desc="组合输入框" className="cn.lz.web.plugin.eui.obj.EuiInputGroup" typeName="WebObject">
   <node tag-name="div" class="el-input-group">
      <node tag-name="input" class="el-input__inner"/>
   </node>
</app-tag>

<app-tag name="EuiInputAffix" desc="带图标的输入框" className="cn.lz.web.plugin.eui.obj.EuiInputAffix" typeName="WebObject">
   <node tag-name="div" class="el-input--suffix">
      <node tag-name="input" class="el-input__inner"/>
   </node>
</app-tag>

<app-tag name="EuiInputAffix2" desc="带图标的输入框" className="cn.lz.web.plugin.eui.obj.EuiInputAffix" typeName="WebObject">
   <node tag-name="div" class="el-input--prefix">
      <node tag-name="input" class="el-input__inner"/>
   </node>
</app-tag>
```

***

## 对象封装

公共方法参见WebNode和WebObject

### input（InputAffix、InputGroup）

针对input组件的特殊封装包括：setValue，getValue两个方法；没有封装isDisabled。缺省输入到input.text组件中，可以选择操作前后图标或前后输入框。如果是可选择的输入框，可以自定义弹出下拉框。

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
      clear( webObj );
      return;
   }
   
   // 输入并重试
   int retry = 0;
   while( retry++ < 6 ) {
      if( typePopup != null && !typePopup.isEmpty() ) {
         // 打开下拉框
         dropDown( webObj );
         
         // 选择
         doSelect( webObj, value );
      }
      else {
         // 查找对象
         InputGroupBox inputGroup = getInputGroup( webObj );
         
         // 输入值
         WebFactory.$(inputGroup.inputNode).setValue( value );
      }
      
      if( isComplete(webObj, value) ) {
         return;
      }
      
      MiscUtil.sleep( retry*50 );
   }
   
   throw new LzException( "E908", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItems | 取组合输入框的所有子对象 |



