# Tags 对象封装

支持antd5、antdv3、element ui2的tag组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tags/stuc.png "对象封装结构")

Tags组件从WebObject派生。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

tag组件的主要操作包括：
- setValue 设置标签
- getValue 取所有标签。

次要操作
- getOptions 取标签数组

setValue 的命令包括

| 名称 | 说明 |
| --- | --- |
| 增加 | 增加标签 |
| 删除 | 删除标签 |
| 修改 | 修改标签的标题 |
| 选择 | 可选的标签组，选择标签 |


getValue的命令包括

| 名称 | 说明 |
| --- | --- |
| 选择 | 可选的标签组，取选中的标签 |

### antd5 

【[antd v5 的 tag 组件](https://ant-design.antgroup.com/components/tag-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tags/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==tags==数据显示==tags==

# 增加或删除
WebSetValue==tags.Unremovable==Unremovable,Tag3,Tag7==
WebGetValue==Unremovable==tags.Unremovable==

# 增加
WebSetValue==tags.Unremovable==增加==Tag5==
WebGetValue==Unremovable==tags.Unremovable==

# 修改
WebSetValue==tags.Tag3==修改==ABC==
WebSetValue==tags.Unremovable--Tag5==修改==ABC==
```

对象匹配规则
```xml
<app-tag name="AntdTags" desc="标签组" className="cn.lz.web.plugin.antd.obj.AntdTags" typeName="WebObject">
   <node tag-name="span" class="ant-tag"/>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 tag 组件](https://www.antdv.com/components/tag-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tags/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==tags==数据显示==tags==
WebSetValue==tags.Unremovable==Unremovable,Tag2,Tag7==
WebGetValue==Unremovable==tags.Unremovable==

WebSetValue==tags.Unremovable==增加==Tag5==
WebGetValue==Unremovable==tags.Unremovable==
```

对象匹配规则
```xml
<app-tag name="AntdvTags" desc="标签组" className="cn.lz.web.plugin.antdv.obj.AntdvTags" typeName="WebObject">
   <node tag-name="span" class="ant-tag"/>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 tag 组件](https://element.eleme.cn/#/zh-CN/component/tag)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/tags/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==tags==功能演示==tags==

WebSetValue==tags.标签一==删除==标签二==
WebGetValue==标签一==tags.标签一==

WebSetValue==tags.标签一==增加==标签5==
WebGetValue==标签一==tags.标签一==

WebSetValue==tags.标签一==标签一,标签5,标签八,标签十==
```

对象匹配规则
```xml
<app-tag name="EuiTags" desc="标签" className="cn.lz.web.plugin.eui.obj.EuiTags" typeName="WebObject">
   <node tag-name="span" class="el-tag"/>
</app-tag>
```

***

## 对象封装

### tags

公共方法参见WebNode和WebObject

针对Tags组件的特殊封装包括：setValue，getValue，getOptions三个方法。

setValue的参考流程。

```java
protected boolean setValue2( WebObject parent, String value )
{
   boolean ret = false;
   
   // 取所有标签
   List<TagItem> tags = getAllItems( parent );
   
   // 计算增加的标签和删除的标签
   List<String> list = StringUtil.splitItems( value );
   
   // 需要删除的标签
   List<TagItem> delTags = new ArrayList<>();
   for( TagItem item : tags ) {
      boolean rc = list.remove( item.title );
      if( !rc ) {
         delTags.add( item );
      }
   }
   
   // 先删除
   if( !delTags.isEmpty() ) {
      ret = true;
      deleteTags( parent, delTags );
   }
   
   // 再增加
   if( !list.isEmpty() ) {
      ret = true;
      addTags( parent, list );
   }
   
   return ret;
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getAllItem | 取所有标签 |
| getButton | 增加标签的按钮 |
| getText | 增加标签的输入框 |
| getUpdateNode | 修改标签的输入框 |





