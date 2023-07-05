# radio 对象封装

支持antd5、antdv3、element ui2的radio组件，根据显示方式不同，封装了RadioBox和RadioButton两种组件，已经相对应的两种组合对象RadioBoxGroup和RadioButtonGroup。

对象封装结构

RadioBoxGroup

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/radio/stuc.png "对象封装结构")

RadioBox

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/radio/stuc2.png "对象封装结构")

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。封装后包含两组对象，RadioBox和RadioButton。

radiogroup组件的主要操作包括：
- 选择Radio、
- 取选中的Radio；

次要操作包括：
- 取所有Radio的标题。

### antd5 

【[antd v5 的 radio 组件](https://ant-design.antgroup.com/components/radio-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/radio/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==radio==表单组件==radio==

# 两种操作，一种直接点击Radio，另一种在RadioGroup中选择一个
WebSetValue==radio.A==B==
WebSetValue==radio.AppleRadio==Pear==
WebSetValue==radio.Apple==Apple==
WebSetValue==radio.C==true==
WebSetValue==radio.Pear==true==

WebGetValue==A3==radio.A==
WebGetValue==AppleRadio3==radio.AppleRadio==
WebGetValue==Apple3==radio.Apple==
WebGetValue==C3==radio.C==
WebGetValue==Pear3==radio.Pear==
```

对象匹配规则
```xml
<app-tag name="AntdRadioGroup" desc="按钮组" className="cn.lz.web.plugin.antd.obj.AntdRadioGroup" typeName="WebObject">
   <node tag-name="div" class="ant-radio-group"/>
</app-tag>

<app-tag name="AntdRadioButtonGroup" desc="按钮组" className="cn.lz.web.plugin.antd.obj.AntdRadioButtonGroup" typeName="WebObject">
   <node tag-name="div" class="ant-radio-group"/>
</app-tag>

<app-tag name="AntdRadio" desc="单选按钮" className="cn.lz.web.plugin.antd.obj.AntdRadioBox" typeName="WebObject">
   <node tag-name="label" class="ant-radio-wrapper">
      <node tag-name="span" class="ant-radio">
         <node tag-name="input" type="radio"/>
      </node>
   </node>
</app-tag>

<app-tag name="AntdRadioButton" desc="单选按钮" className="cn.lz.web.plugin.antd.obj.AntdRadioButton" typeName="WebObject">
   <node tag-name="label" class="ant-radio-button-wrapper">
      <node tag-name="span" class="ant-radio-button">
         <node tag-name="input" type="radio"/>
      </node>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 radio 组件](https://www.antdv.com/components/radio-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/radio/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==radio==表单==radio==

# 两种操作，一种直接点击Radio，另一种在RadioGroup中选择一个
WebSetValue==radio.Hangzhou==Shanghai==
WebSetValue==radio.Beijing==true==
WebSetValue==radio.Apple==Pear==
WebSetValue==radio.A==C==

WebGetValue==Hangzhou==radio.Hangzhou==
WebGetValue==Beijing==radio.Beijing==
WebGetValue==Apple==radio.Apple==
WebGetValue==A==radio.A==
```

对象匹配规则
```xml
<app-tag name="AntdvRadioGroup" desc="按钮组" className="cn.lz.web.plugin.antdv.obj.AntdvRadioGroup" typeName="WebObject">
   <node tag-name="div" class="ant-radio-group"/>
</app-tag>

<app-tag name="AntdvRadioButtonGroup" desc="按钮组" className="cn.lz.web.plugin.antdv.obj.AntdvRadioButtonGroup" typeName="WebObject">
   <node tag-name="div" class="ant-radio-group"/>
</app-tag>

<app-tag name="AntdvRadio" desc="单选按钮" className="cn.lz.web.plugin.antdv.obj.AntdvRadioBox" typeName="WebObject">
   <node tag-name="label" class="ant-radio-wrapper">
      <node tag-name="span" class="ant-radio">
         <node tag-name="input" type="radio"/>
      </node>
   </node>
</app-tag>

<app-tag name="AntdvRadioButton" desc="单选按钮" className="cn.lz.web.plugin.antdv.obj.AntdvRadioButton" typeName="WebObject">
   <node tag-name="label" class="ant-radio-button-wrapper">
      <node tag-name="span" class="ant-radio-button">
         <node tag-name="input" type="radio"/>
      </node>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 radio 组件](https://element.eleme.cn/#/zh-CN/component/radio)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/radio/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==radio==功能演示==radio==

# 两种操作，一种直接点击Radio，另一种在RadioGroup中选择一个
WebSetValue==radio.北京==true==
WebGetValue==北京==radio.北京==

WebSetValue==radio.上海==广州==
WebGetValue==上海==radio.上海==
```

对象匹配规则
```xml
<app-tag name="EuiRadioGroup" desc="按钮组" className="cn.lz.web.plugin.eui.obj.EuiRadioGroup" typeName="WebObject">
   <node tag-name="div" class="el-radio-group" role="radiogroup"/>
</app-tag>

<app-tag name="EuiRadioButtonGroup" desc="按钮组" className="cn.lz.web.plugin.eui.obj.EuiRadioButtonGroup" typeName="WebObject">
   <node tag-name="div" class="el-radio-group" role="radiogroup"/>
</app-tag>

<app-tag name="EuiRadio" desc="单选按钮" className="cn.lz.web.plugin.eui.obj.EuiRadioBox" typeName="WebObject">
   <node tag-name="label" class="el-radio" role="radio">
      <node tag-name="span" class="el-radio__input"/>
      <node tag-name="span" class="el-radio__label"/>
   </node>
</app-tag>

<app-tag name="EuiRadioButton" desc="单选按钮" className="cn.lz.web.plugin.eui.obj.EuiRadioButton" typeName="WebObject">
   <node tag-name="label" class="el-radio-button" role="radio">
      <node tag-name="input" type="radio" class="el-radio-button__orig-radio"/>
      <node tag-name="span" class="el-radio-button__inner"/>
   </node>
</app-tag>
```

***

## 对象封装

公共方法参见WebNode和WebObject

### radiogroup

针对radiogroup组件的特殊封装包括：setValue，getValue和getOptions三个方法。没有封装isDisabled。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 查找主对象
   WebObject webObj = getH5Object();
   
   String jsFile = getItemScript();
   
   // 查找节点
   Object obj = webObj.getValueByScript( jsFile, value );
   if( obj == null ){
      throw new LzException( "E821", "单选按钮["+value+"]不存在" );
   }
   
   // 返回：node,icon,label,title,isChecked
   RadioItem item = new RadioItem( obj );
   
   // 是否已经选中
   String isChecked = item.isChecked;
   if( "true".equals(isChecked) ) {
      return;
   }
   
   for( int retry=0; retry<5; retry++ ) {
       // 点击图标
       WebFactory.$(item.icon).click();
      
       // 判断是否已选中
       obj = webObj.getValueByScript( jsFile, value );
       item = new RadioItem( obj );
      
       // 是否已经选中
       if( "true".equals(item.isChecked) ) {
          return;
       }
       
       MiscUtil.sleep( (1+retry)*50 );
   }
   
   // 没有选中
   throw new LzException( "E821", "单选按钮["+value+"]没有选中，当前选择项：" + getValue2() );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取选中节点的标题 |
| getItem | 查找节点(输入标题) |
| getAllItems | 取所有节点 |


### radio

radio组件封装了setValue，getValue两个方法。没有封装isDisabled。

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取Radio的可点击节点 |
| getValue | 取Radio的状态 |



