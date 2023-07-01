# select 对象封装

支持antd5、antdv3、element ui2的select组件，单选和多选。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/select/stuc.png "对象封装结构")


## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

select组件的主要操作包括：
- 选择数据、
- 取数据；

次要操作包括：
- 取下拉列表的值。

### antd5 

【[antd v5 的 select 组件](https://ant-design.antgroup.com/components/select-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/select/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==select==表单组件==select==
WebSetValue==select.单选框==yiminghe==
WebGetValue==单选框==select.单选框==

WebSetValue==select.复选框==b11,d13==
WebGetValue==复选框==select.复选框==
WebSetValue==select.复选框==%EMPTY%==

WebSetValue==select.复选框9==b11,d13==
WebGetValue==复选框9==select.复选框9==

WebSetValue==select.复选框10==b11,d13,kkk==
WebGetValue==复选框10==select.复选框10==
```
最后一个对象是通过键盘输入的，感觉有点慢。


***

### antd vue 3

【[antd vue v3 的 select 组件](https://www.antdv.com/components/select-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/select/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==select==表单==select==
WebSetValue==select.单选==yiminghe==
WebGetValue==单选==select.单选==

WebSetValue==select.多选==b2,e5==
WebGetValue==多选==select.多选==
```



***

### element ui 2

【[element ui 2 的 select 组件](https://element.eleme.cn/#/zh-CN/component/select)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/select/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==selects==功能演示==selects==
WebSetValue==selects.script==CSS;ABC==
WebGetValue==script==selects.script==

WebDefPage==select2==功能演示==select2==
WebSetValue==select2.请选择==双皮奶==
WebGetValue==请选择==select2.请选择==
```

***

## 对象封装

公共方法参见WebNode和WebObject

### select

针对select组件的特殊封装包括：setValue，getValue和getOptions三个方法；没有封装isDisabled。

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 只比较格式化后的数据
   String title = HintWindow.formatTitle( value );
   
   if( value.startsWith("%RAND_SELECT:") ) {
      // 随机选择，如果有内容就不需要操作
      if( isNotEmpty() ) {
         return;
      }
   }
   else if( isComplete(title) ) {
      // 目标值和输入值一致
      return;
   }
   
   // 查找主对象
   WebObject webObj = getH5Object();
   
   // 清空
   if( title.isEmpty() ) {
      clear( webObj );
      return;
   }
   
   int retry = 0;
   while( retry++ < 6 ) {
      // 打开下拉框
      dropDown( webObj );
      
      // 等待下拉框打开
      int delay = (retry - 1) * 70;
      if( delay > 0 ) {
         MiscUtil.sleep( delay );
      }

      // 随机选择
      if( value.startsWith("%RAND_SELECT:") ) {
         // 从下拉框中取选项，并随机取一个
         String randValue = getRandomValue();
         doSelect( webObj, randValue );
         
         // 检查输入框中有值，不判断和[randValue]是否一致
         String value2 = getValue();
         if( value2 != null && !value2.isEmpty() ) {
            saveSelectRandValue( value, value2 );
            return;
         }
      }
      else {
         // 选择
         doSelect( webObj, value );
         
         // 判断是否已经输入
         if( isComplete(title) ) {
            return;
         }
      }
   }
   
   // 从下拉框选择数据错误
   throw new LzException( "E908", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 下拉列表中根据字符串取对象 |
| getOptions | 取下拉列表的所有选项 |
| getValue | 取下拉框的值 |
| getOpenItem | 打开下拉框的图标 |
| getClearItem.txt | 清空下拉框的图标 |
| getPopupWindow | 取弹出窗口 |


### selects

针对selects组件的特殊封装包括：setValue，getValue和getOptions三个方法；没有封装isDisabled。setValue支持随机选择（比如随机选择省市县等）

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 格式化数据，生成数组
   List<String> values = formatValues( value );

   // 查找主对象
   WebObject webObj = getH5Object();
   
   for( int retry=0; retry<6; retry++ ) {
      // 已经选中的值
      List<SelectsItem> items = getItems( webObj );
      
      // 检查值是已输入
      if( isComplete(values, items) ) {
         return;
      }
      
      // 清空
      if( value == null || value.isEmpty() ) {
         boolean rc = clear( webObj );
         if( rc ) {
         	// 需要检查数据是否已清空，rc=false，右侧没有清空图标
            continue;
         }
      }
      
      // 打开下拉框
      dropDown( webObj );
      
      // 等待窗口打开
      if( retry > 0 ) {
         MiscUtil.sleep( retry*30 );
      }
      
      // 不选中
      deselectValues( webObj, values, items );
      
      // 选中
      selectValues( webObj, values, items );
      
      // 关闭选择框
      WebUtil.pressEscape();
   }
   
   // 从下拉框选择数据错误
   throw new LzException( "E908", "预期输入["+value+"]，实际输入["+getValue()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getItem | 下拉列表中根据字符串取对象 |
| getOptions | 取下拉列表的所有选项 |
| getSelectedItems | 输入框中当前选中的项 |
| getOpenItem | 打开下拉框的图标 |
| getClearItem.txt | 清空下拉框的图标 |
| getPopupWindow | 取弹出窗口 |



