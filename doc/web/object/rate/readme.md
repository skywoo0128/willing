# Rate 对象封装

支持antd5、antdv3、element ui2的rate组件,支持半星选择。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/rate/stuc.png "对象封装结构")

Rate组件从WebObject派生，可以是半星。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Rate组件的主要操作包括：
- setValue（设置评分）；
- getValue（取评分值）


### antd5 

【[antd v5 的 rate 组件](https://ant-design.antgroup.com/components/rate-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/rate/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==rate==表单组件==rate==
WebSetValue==rate.normal==5==
WebGetValue==normal==rate.normal==

WebSetValue==rate.选中半星==0.5==
WebGetValue==选中半星==rate.选中半星==
```


***

### antd vue 3

【[antd vue v3 的 rate 组件](https://www.antdv.com/components/rate-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/rate/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==rate==表单==rate==
WebSetValue==rate.normal==1==
WebGetValue==normal==rate.normal==

WebSetValue==rate.半星==4.5==
WebGetValue==半星==rate.半星==
```



***

### element ui 2

【[element ui 2 的 rate 组件](https://element.eleme.cn/#/zh-CN/component/rate)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/rate/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==rate==功能演示==rate==
WebSetValue==rate.评分==4==
WebGetValue==评分==rate.评分==
```

***

## 对象封装

### rate

公共方法参见WebNode和WebObject

针对Rate组件的特殊封装包括：setValue，getValue两个方法。

setValue的参考流程。

```java
// value=序号（评分），比如 1-5
protected void setValue2( String value, boolean isCheckEnable )
{
   // 原始值
   if( isComplete(value) ) {
      return;
   }
   
   // 是否清空，点击同一个节点清空
   String value2 = value;
   if( value == null || value.isEmpty() || "0".equals(value) ) {
      value2 = getValue();
   }
   
   // 取主节点
   WebObject webObj = getH5Object();
   
   String jsFile = getItemScript();
   
   for( int i=0; i<5; i++ ) {
      ElementByScript byScript = new ElementByScript( jsFile, webObj, value2 );
      WebFactory.$(byScript).click();
      
      // 是否已完成
      if( isComplete(value) ) {
         return;
      }
      
      MiscUtil.sleep( (1+i)*50 );
   }
   
   throw new LzException( "E612", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 取可点击节点,半星返回一半 |
| getValue | 取评分 |





