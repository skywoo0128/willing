# Anchor 对象封装

支持antd5、antdv3、element ui2的anchor组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/link/stuc.png "对象封装结构")

Switch组件从WebButton派生。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Switch组件的主要操作包括：
- click 点击链接；

次要操作
- getValue（取标题）


### antd5 

【[antd v5 的 anchor 组件](https://ant-design.antgroup.com/components/anchor-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/link/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==link==导航==link==
WebClick==link.Staticdemo==
```

对象匹配规则
```xml
<app-tag name="AntdLink" desc="锚点" className="cn.lz.web.plugin.antd.obj.AntdLink" typeName="WebButton">
   <node tag-name="div" class="ant-anchor-link">
      <node tag-name="a" class="ant-anchor-link-title"/>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 anchor 组件](https://www.antdv.com/components/anchor-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/link/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==link==扩展组件==link==
WebClick==link.Basicdemo==
```

对象匹配规则
```xml
<app-tag name="AntdvLink" desc="锚点" className="cn.lz.web.plugin.antdv.obj.AntdvLink" typeName="WebButton">
   <node tag-name="div" class="ant-anchor-link">
      <node tag-name="a" class="ant-anchor-link-title"/>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 link 组件](https://element.eleme.cn/#/zh-CN/component/link)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/link/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==link==功能演示==link==
WebClick==link.成功链接==

WebButtonGetCaption==主要链接==link.主要链接==
WebButtonIsEnable==isEnabled==link.主要链接==

WebClick==link.默认链接==
```

对象匹配规则
```xml
<app-tag name="EuiLink" desc="锚点" className="cn.lz.web.plugin.eui.obj.EuiLink" typeName="WebButton">
   <node tag-name="a" class="el-link">
      <node tag-name="span" class="el-link--inner"/>
   </node>
</app-tag>
```

***

## 对象封装

### anchor（link）

公共方法参见WebNode和WebButton

针对Anchor组件的特殊封装包括：isEnabled，getValue两个方法。这两个函数的封装比较简单


**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取Anchor的标题 |





