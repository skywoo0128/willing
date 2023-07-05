# Switch 对象封装

支持antd5、antdv3、element ui2的switch组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/switch/stuc.png "对象封装结构")

Switch组件从WebObject派生。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Switch组件的主要操作包括：
- setValue（打开或关闭）；
- getValue（取开关状态）


### antd5 

【[antd v5 的 switch 组件](https://ant-design.antgroup.com/components/switch-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/switch/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==switchPage==表单组件==switch==
WebSetValue==switchPage.最简单的用法==false==
WebGetValue==基本最简单的用法==switchPage.最简单的用法==
```

对象匹配规则
```xml
<app-tag name="AntdSwitch" desc="开关" className="cn.lz.web.plugin.antd.obj.AntdSwitch" typeName="WebObject">
   <node tag-name="button" class="ant-switch">
      <node tag-name="div" class="ant-switch-handle"/>
      <node tag-name="span" class="ant-switch-inner"/>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 switch 组件](https://www.antdv.com/components/switch-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/switch/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==switchPage==表单==switch==
WebSetValue==switchPage.基本用法==true==
WebGetValue==基本用法==switchPage.基本用法==

WebSetValue==switchPage.文字开关==false==
WebGetValue==文字开关==switchPage.文字开关==
```

对象匹配规则
```xml
<app-tag name="AntdvSwitch" desc="开关" className="cn.lz.web.plugin.antdv.obj.AntdvSwitch" typeName="WebObject">
   <node tag-name="button" class="ant-switch">
      <node tag-name="div" class="ant-switch-handle"/>
      <node tag-name="span" class="ant-switch-inner"/>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 switch 组件](https://element.eleme.cn/#/zh-CN/component/switch)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/switch/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==switchPage==功能演示==switch==
WebSetValue==switchPage.基本用法==false==
WebGetValue==基本用法==switchPage.基本用法==

WebSetValue==switchPage.按年付费==false==
WebGetValue==按年付费==switchPage.按年付费==
```

对象匹配规则
```xml
<app-tag name="EuiSwitch" desc="开关" className="cn.lz.web.plugin.eui.obj.EuiSwitch" typeName="WebObject">
   <node tag-name="div" class="el-switch" role="switch"/>
</app-tag>
```

***

## 对象封装

### switch

公共方法参见WebNode和WebObject

针对Switch组件的特殊封装包括：setValue，getValue两个方法。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 状态是否一致
   if( isComplete(value) ) {
      return;
   }
   
   // 查找对象
   WebObject webObj = getH5Object();
   
   // 取主节点
   String jsFile = getItemScript();
   
   for( int retry=0; retry<5; retry++ ) {
      // 文件不存在，不需要查找主节点
      if( jsFile == null ){
         webObj.click();
      }
      else {
         ElementByScript byScript = new ElementByScript( jsFile, webObj );
         WebFactory.$(byScript).click();
      }
      
      // 状态是否一致
      if( isComplete(value) ) {
         return;
      }
      
      MiscUtil.sleep( (1+retry)*50 );
   }
   
   throw new LzException( "E612", "预期输入["+value+"]，实际输入["+getValue2()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取Switch的可点击节点 |
| getValue | 取Switch的状态 |





