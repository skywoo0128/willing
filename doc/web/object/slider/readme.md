# Slider 对象封装

支持antd5、antdv3、element ui2的slider组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/slider/stuc.png "对象封装结构")

Slider组件从WebObject派生，可以是一个滑块，也可以是两个滑块。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Slider组件的主要操作包括：
- setValue（设置单个滑块，或两个滑块的位置）；
- getValue（取滑块的值）


### antd5 

【[antd v5 的 slider 组件](https://ant-design.antgroup.com/components/slider-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/slider/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==slider==表单组件==slider==
WebSetValue==slider.第一个滑块==42==
WebGetValue==第一个滑块==slider.第一个滑块==

WebSetValue==slider.垂直滑块==30,40==
WebGetValue==垂直滑块==slider.垂直滑块==
```

对象匹配规则
```xml
<app-tag name="AntdSlider" desc="滑块" className="cn.lz.web.plugin.antd.obj.AntdSlider" typeName="WebObject">
   <node tag-name="div" class="ant-slider">
      <node tag-name="div" class="ant-slider-handle" role="slider"/>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 slider 组件](https://www.antdv.com/components/slider-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/slider/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==slider==表单==slider==
WebSetValue==slider.第一个滑块==42==
WebGetValue==第一个滑块==slider.第一个滑块==

WebSetValue==slider.两个滑块==30,50==
WebGetValue==两个滑块==slider.垂直滑块==

WebSetValue==slider.垂直滑块==30,50==
WebGetValue==垂直滑块==slider.垂直滑块==
```

对象匹配规则
```xml
<app-tag name="AntdvSlider" desc="滑块" className="cn.lz.web.plugin.antdv.obj.AntdvSlider" typeName="WebObject">
   <node tag-name="div" class="ant-slider">
      <node tag-name="div" class="ant-slider-handle" role="slider"/>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 slider 组件](https://element.eleme.cn/#/zh-CN/component/slider)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/slider/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==slider==功能演示==slider==
WebSetValue==slider.自定义初始值==73==
WebGetValue==自定义初始值==slider.自定义初始值==

WebSetValue==slider.隐藏Tooltip==31==
WebGetValue==隐藏Tooltip==slider.隐藏Tooltip==
```

对象匹配规则
```xml
<app-tag name="EuiSlider" desc="滑块" className="cn.lz.web.plugin.eui.obj.EuiSlider" typeName="WebObject">
   <node tag-name="div" class="el-slider" role="slider">
      <node tag-name="div" class="el-slider__runway"/>
   </node>
</app-tag>
```

***

## 对象封装

### Slider

公共方法参见WebNode和WebObject

针对Slider组件的特殊封装包括：setValue，getValue两个方法。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   List<String> values = StringUtil.splitItems( value );
   for( int i=0; i<6; i++ ) {
      // 取主节点
      WebObject webObj = getH5Object();
      
      setValue2( webObj, values );
      
      // 判断是否已成功
      if( isComplete(webObj, values) ) {
         return;
      }
      
      MiscUtil.sleep( (i+1)*50 );
   }
   
   throw new LzException( "E603", "预期输入["+value+"]，实际输入["+getValue2()+"]" );
}

protected void setValue2( WebObject parent, List<String> values )
{
   String jsFile = getItemScript();
   
   for( int i=0; i<values.size(); i++ ) {
      Object obj = parent.getValueByScript( jsFile, i );
      SliderBox box = new SliderBox(obj);
      int value2 = Integer.parseInt( values.get(i) );
      
      WebUtil.scrollIntoView( box.node );
      if( box.width >= 28 ) {
         // 水平
         int pos = ((value2 - box.min)*box.width) / (box.max - box.min);

         pos = pos - box.width/2;
         WebFactory.$(box.slider).dragAndDropTo( box.node, pos, 1 );
      }
      else {
         // 垂直
         int pos = ((value2 - box.min)*box.height) / (box.max - box.min);

         pos = box.height/2 - pos;
         WebFactory.$(box.slider).dragAndDropTo( box.node, 0, pos );
      }
   }
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取Slider的内容，包括滑条，滑块 |
| getValue | 取值（滑块的位置，两个滑块用,分割） |





