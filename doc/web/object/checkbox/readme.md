# check 对象封装

支持antd5、antdv3、element ui2的check组件，根据显示方式不同，封装了CheckBox和CheckButton两种组件，已经相对应的两种组合对象CheckBoxGroup和CheckButtonGroup。

对象封装结构

CheckBoxGroup

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/checkbox/stuc.png "对象封装结构")

CheckBox

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/checkbox/stuc2.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。封装后包含两组对象，CheckBox和CheckButton。

checkgroup组件的主要操作包括：
- 选择Check、
- 取选中的Check；

次要操作包括：
- 取所有Check的标题。

### antd5 

【[antd v5 的 checkbox 组件](https://ant-design.antgroup.com/components/checkbox-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/checkbox/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==checkBox==表单组件==checkBox==

# 两种操作，一种直接点击CheckBox，另一种在CheckGroup中选择
WebSetValue==checkBox.Checkbox==true==
WebGetValue==Apple2==checkBox.Apple==

WebSetValue==checkBox.Apple==Pear,Orange==
WebSetValue==checkBox.Apple--Apple==true==
WebGetValue==Checkbox2==checkBox.Checkbox==
```

对象匹配规则
```xml
<app-tag name="AntdCheckBox" desc="复选框" className="cn.lz.web.plugin.antd.obj.AntdCheckBox" typeName="WebObject">
   <node tag-name="label" class="ant-checkbox-wrapper">
      <node tag-name="span" class="ant-checkbox">
         <node tag-name="input" type="checkbox"/>
      </node>
   </node>
</app-tag>

<app-tag name="AntdCheckGroup" desc="复选框组" className="cn.lz.web.plugin.antd.obj.AntdCheckGroup" typeName="WebObject">
   <node tag-name="div" class="ant-checkbox-group"/>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 checkbox 组件](https://www.antdv.com/components/checkbox-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/checkbox/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==checkBox==表单==checkbox==

# 两种操作，一种直接点击CheckBox，另一种在CheckGroup中选择
WebSetValue==checkBox.Checkbox==true==
WebGetValue==Checkbox==checkBox.Checkbox==

WebSetValue==checkBox.组合选择==Pear,Orange==
WebSetValue==checkBox.组合选择--Apple==true==
WebGetValue==组合选择==checkBox.组合选择==
```

对象匹配规则
```xml
<app-tag name="AntdvCheckBox" desc="复选框" className="cn.lz.web.plugin.antdv.obj.AntdvCheckBox" typeName="WebObject">
   <node tag-name="label" class="ant-checkbox-wrapper">
      <node tag-name="span" class="ant-checkbox">
         <node tag-name="input" type="checkbox"/>
      </node>
   </node>
</app-tag>

<app-tag name="AntdvCheckGroup" desc="复选框组" className="cn.lz.web.plugin.antdv.obj.AntdvCheckGroup" typeName="WebObject">
   <node tag-name="div" class="ant-checkbox-group"/>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 checkbox 组件](https://element.eleme.cn/#/zh-CN/component/checkbox)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/checkbox/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==checkbutton==功能演示==checkbutton==

# 两种操作，一种直接点击CheckBox，另一种在CheckGroup中选择
WebSetValue==checkbutton.北京==true==
WebGetValue==上海2==checkbutton.北京==

WebSetValue==checkbutton.上海==北京,广州==
WebGetValue==上海==checkbutton.上海==

WebDefPage==checkbox==功能演示==checkbox==
WebSetValue==checkbox.选择城市==北京,广州==
WebGetValue==选择城市==checkbox.选择城市==
```

对象匹配规则
```xml
<app-tag name="EuiCheckGroup" desc="复选框组" className="cn.lz.web.plugin.eui.obj.EuiCheckGroup" typeName="WebObject">
   <node tag-name="div" class="el-checkbox-group" role="group"/>
</app-tag>

<app-tag name="EuiCheckButtonGroup" desc="复选框组" className="cn.lz.web.plugin.eui.obj.EuiCheckButtonGroup" typeName="WebObject">
   <node tag-name="div" class="el-checkbox-group" role="group"/>
</app-tag>

<app-tag name="EuiCheckBox" desc="复选框" className="cn.lz.web.plugin.eui.obj.EuiCheckBox" typeName="WebObject">
   <node tag-name="label" class="el-checkbox">
      <node tag-name="span" class="el-checkbox__input"/>
      <node tag-name="span" class="el-checkbox__label"/>
   </node>
</app-tag>

<app-tag name="EuiCheckButton" desc="多选按钮" className="cn.lz.web.plugin.eui.obj.EuiCheckButton" typeName="WebObject">
   <node tag-name="label" class="el-checkbox-button" role="checkbox">
      <node tag-name="input" type="checkbox" class="el-checkbox-button__original"/>
      <node tag-name="span" class="el-checkbox-button__inner"/>
   </node>
</app-tag>
```

***

## 对象封装

公共方法参见WebNode和WebObject

### checkgroup

针对checkgroup组件的特殊封装包括：setValue，getValue和getOptions三个方法。没有封装isDisabled。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   WebObject webObj = getH5Object();
   
   String jsFile = getAllItemsScript();
   
   // 查找节点
   Object obj = webObj.getValueByScript( jsFile );
   if( obj == null ){
      throw new LzException( "E821", "复选框不存在" );
   }
   
   List<CheckItem> nodes = toItems( obj );
   
   // 需要选中的值
   List<String> values = StringUtil.splitItems( value );
   
   // 遍历所有CheckBox
   for( CheckItem item : nodes ) {
      String str = item.title;
      int pos = values.indexOf( str );
      if( pos >= 0 ) {
         values.remove( pos );
      }
      
      boolean flag = (pos >= 0);
      boolean flag2 = ("true".equals(item.isChecked));
      if( flag != flag2 ) {
          // 点击图标
          WebElement icon = item.icon;
          WebFactory.$(icon).click();
      }
   }
   
   // 是否有不存在的节点
   if( !values.isEmpty() ) {
      LoggerUtil.writeLog( "CheckBox节点["+values.toString()+"]不存在" );
   }
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取选中节点的标题(数组) |
| getAllItems | 取所有节点 |


### checkbox

checkbox组件封装了setValue，getValue两个方法。没有封装isDisabled。

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取CheckBox的可点击节点 |
| getValue | 取CheckBox的状态 |



