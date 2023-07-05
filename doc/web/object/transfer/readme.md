# Transfer 对象封装

支持antd5、antdv3、element ui2的transfer组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/transfer/stuc.png "对象封装结构")

Transfer组件从WebObject派生，左侧左侧List, Table和Tree，右侧左侧List和Table。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Transfer组件的主要操作包括：
- setValue（设置右侧的选项）；
- 从左侧增加到右侧
- 从右侧删除
- getValue（取右侧选择的选项）

次要操作包括：
- getOptions 取左右两侧的所有选项

setValue支持二级命令，包括：增加，删除。
可以独立操作以下部件：左侧选项, 右侧选项, 左侧区间, 右侧区间, 增加按钮, 删除按钮

| 指令 | 说明 |
| --- | --- |
| `WebSetValue==UI对象==字段列表==` | 置右侧的选项 |
| `WebSetValue==UI对象==增加==字段列表==` | 从左侧增加到右侧 |
| `WebSetValue==UI对象==删除==字段列表==` | 从右侧删除 |

### antd5 

【[antd v5 的 transfer 组件](https://ant-design.antgroup.com/components/transfer-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/transfer/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==transfer==表单组件==transfer==
WebSetValue==transfer.穿梭框==content2,content3,content14,content15,content16==
WebGetValue==穿梭框==transfer.穿梭框==

WebSetValue==transfer.穿梭框2==content2,content3,content5,content12,content18==
WebGetValue==穿梭框2==transfer.穿梭框2==

WebSetValue==transfer.表格==content2,content3,content4,content9,content12,content15==
WebGetValue==表格==transfer.表格==

WebSetValue==transfer.表格==增加==content6==
WebSetValue==transfer.表格==删除==content12==
WebGetValue==表格==transfer.表格==

WebSetValue==transfer.树结构==0-1/0-1-1,0-2,0-3==
WebGetValue==树结构==transfer.树结构==

WebSetValue==transfer.树结构==增加==0-1/0-1-0,0-4==
WebGetValue==树结构==transfer.树结构==
```

对象匹配规则
```xml
<app-tag name="AntdTransfer" desc="穿梭框" className="cn.lz.web.plugin.antd.obj.AntdTransfer" typeName="WebObject">
   <node tag-name="div" class="ant-transfer">
      <node tag-name="div" class="ant-transfer-list"/>
      <node tag-name="div" class="ant-transfer-operation"/>
      <node tag-name="div" class="ant-transfer-list"/>
   </node>
</app-tag>

<app-tag name="AntdTransList" desc="穿梭框的选项" className="cn.lz.web.plugin.antd.obj.AntdTransList" typeName="WebObject">
   <node tag-name="div" class="ant-transfer-list">
      <node tag-name="div" class="ant-transfer-list-body">
         <node tag-name="ul" class="ant-transfer-list-content"/>
      </node>
   </node>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 transfer 组件](https://www.antdv.com/components/transfer-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/transfer/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==transfer==表单==transfer==
WebSetValue==transfer.基本transfer==content2,content3,content14,content15,content17==
WebGetValue==基本transfer==transfer.基本transfer==

WebSetValue==transfer.transfer2==content2,content5,content9==
WebGetValue==transfer2==transfer.transfer2==

WebSetValue==transfer.表格transfer==content2,content6,content9==
WebGetValue==表格transfer==transfer.表格transfer==

WebSetValue==transfer.树结构==0-1/0-1-1,0-3==
WebGetValue==树结构==transfer.树结构==

WebSetValue==transfer.树结构==增加==0-1/0-1-0,0-0==
WebGetValue==树结构==transfer.树结构==
```

对象匹配规则
```xml
<app-tag name="AntdvTransfer" desc="穿梭框" className="cn.lz.web.plugin.antdv.obj.AntdvTransfer" typeName="WebObject">
   <node tag-name="div" class="ant-transfer">
      <node tag-name="div" class="ant-transfer-list"/>
      <node tag-name="div" class="ant-transfer-operation"/>
      <node tag-name="div" class="ant-transfer-list"/>
   </node>
</app-tag>

<app-tag name="AntdvTransList" desc="穿梭框的选项" className="cn.lz.web.plugin.antdv.obj.AntdvTransList" typeName="WebObject">
   <node tag-name="div" class="ant-transfer-list">
      <node tag-name="div" class="ant-transfer-list-body">
         <node tag-name="ul" class="ant-transfer-list-content"/>
      </node>
   </node>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 transfer 组件](https://element.eleme.cn/#/zh-CN/component/transfer)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/transfer/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==transfer==功能演示==transfer==
WebSetValue==transfer.列表transfer==备选项3,备选项4,备选项13==
WebGetValue==列表transfer==transfer.列表1013==
```

对象匹配规则
```xml
<app-tag name="EuiTransfer" desc="穿梭框" className="cn.lz.web.plugin.eui.obj.EuiTransfer" typeName="WebObject">
   <node tag-name="div" class="el-transfer">
      <node tag-name="div" class="el-transfer-panel"/>
      <node tag-name="div" class="el-transfer__buttons"/>
      <node tag-name="div" class="el-transfer-panel"/>
   </node>
</app-tag>
```

***

## 对象封装

### Transfer

公共方法参见WebNode和WebObject

针对Transfer组件的特殊封装包括：setValue，getValue，getOptions三个方法。antd中transfer的List组件需要额外封装；element ui的List组件使用CheckGroup组件

setValue的参考流程。

```java
protected void setValue2( String value, boolean isCheckEnable )
{
   // 查找对象
   WebObject webObj = getH5Object();

   // 需要选中的值
   List<String> selValues = StringUtil.splitItems( value );
   
   // 重试六次
   for( int i=0; i<6; i++ ) {
      List<String> values = new ArrayList<>();
      values.addAll( selValues );
      
      // 查找节点信息
       TransferBox box = getTransferBox( webObj );
       
       // 遍历右侧所有CheckBox，是否需要删除，返回删除节点的数量
       int delCount = deleteItems( box, values );
       
       // 可能页面有变更
       box = getTransferBox( webObj );
       
       // 左侧需要添加到右侧，返回增加节点的数量
       int addCount = addItems( box, values );
       
       // 是否有不存在的节点
       printError( box, values );
       
       // 增加
       if( addCount > 0 ) {
          WebFactory.$(box.btn2).click();
          MiscUtil.sleep( 5 );
       }
       
       // 删除
       if( delCount > 0 && box.btn != null ) {
          WebFactory.$(box.btn).click();
       }
       
       // 判断是否已完成
       if( isComplete(selValues) ) {
          return;
       }
       
       MiscUtil.sleep( (i+1)*50 );
   }
   
   // 错误
   LoggerUtil.writeLog( "预期选中的项["+value+"]，实际选中的项["+getValue2()+"]" );
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getValue | 取右侧的选项 |
| getAllItems | 取节点信息（包括左侧组件及所有选项，右侧组件及所有选项，两个按钮 |
| getButtons | 取按钮 |





