# Card 对象封装

支持antd5、antdv3、element ui2的card组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/card/stuc.png "对象封装结构")

Switch组件从WebObject派生。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Card组件的主要操作包括：
- WebObjectClick 点击卡片，或卡片中的内容；
- 对象中查找 在卡片中查找对象
- 取卡片的内容

卡片内容分为四个部分

| 名称 | 说明 |
| --- | --- |
| head | 卡片标题 |
| cover | 卡片内的图片 |
| body | 卡片内容 |
| actions | 额外的按钮 |
| 按钮名称，或图标名称 | 按钮 |

比如 `WebObjectClick==UI对象名称--cover==` 点击的是图片部分。

### antd5 

【[antd v5 的 card 组件](https://ant-design.antgroup.com/components/card-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/card/antd.gif "执行效果")

**自动化脚本**
```
# 点击对象中的内容
对象中查找==card.Cardtitle--head==
WebSetValue==card.tab1==tab2==

# 点击图片
WebObjectClick==card.带图片的卡片--cover==

# 点击按钮
WebObjectClick==card.更多内容--edit==
```


***

### antd vue 3

【[antd vue v3 的 card 组件](https://www.antdv.com/components/card-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/card/antdv.gif "执行效果")

**自动化脚本**
```
# 点击对象中的内容
对象中查找==card.Cardtitle--head==
WebSetValue==card.CardtitleTabs==tab2==

# 点击图片
WebObjectClick==card.带图片的卡片--cover==

# 点击按钮
WebObjectClick==card.更多内容--edit==
```



***

### element ui 2

【[element ui 2 的 card 组件](https://element.eleme.cn/#/zh-CN/component/card)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/card/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==card==功能演示==card==
WebObjectClick==card.卡片名称==操作按钮==

WebDefPage==card2==功能演示==card2==
对象中查找==card2.带图片--body==
WebClick==card2.操作按钮==
```

***

## 对象封装

### card

公共方法参见WebNode和WebObject

针对Card组件的特殊封装包括：click，getValue两个方法。click操作不能错误重试。

click的参考流程。

```java
public void click( String title )
{
   // 查找对象
   WebObject webObj = getH5Object();
   CardBox group = getCardBox( webObj );
   
   // 点击区间
   WebElement childNode = null;
   if( "head".equals(opNode) ) {
      childNode = group.head;
   }
   else if( "body".equals(opNode) ) {
      childNode = group.body;
   }
   else if( "cover".equals(opNode) ) {
      childNode = group.cover;
   }
   else if( "actions".equals(opNode) ) {
      childNode = group.actions;
   }
   
   if( childNode != null ) {
      WebFactory.$(childNode).click();
      return;
   }
   
   // 点击按钮
   for( int i=0; i<count; i++ ) {
      CardButton btn = group.buttons.get( i );
      if( title.equals(btn.name) ) {
         WebFactory.$(btn.node).click();
         break;
      }
   }
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取卡片内容 |
| getAllItems | 取节点信息，包括四个区域和按钮 |





