# Upload 对象封装

支持antd5、antdv3、element ui2的upload组件。

对象封装结构

![对象封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/upload/stuc.png "对象封装结构")

Upload组件从WebObject派生，支持文件上传，检查文件上传是否完成。

## 基本用法

兼容多个不同的UI组件库，使用相同的关键字和对象定位方法。

Upload组件的主要操作包括：
- 点击按钮上传文件；
- 删除文件

次要操作包括：
- 取文件列表
- 预览文件。

setValue支持二级命令，包括：删除，预览。

| 指令 | 说明 |
| --- | --- |
| `WebSetValue==UI对象==文件名称==` | 上传文件 |
| `WebSetValue==UI对象==删除==文件名称==` | 删除文件 |
| `WebSetValue==UI对象==预览==文件名称==` | 预览文件 |

### antd5 

【[antd v5 的 upload 组件](https://ant-design.antgroup.com/components/upload-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/upload/antd.gif "执行效果")

**自动化脚本**
```
WebDefPage==upload==表单组件==upload==
WebSetValue==upload.上传文件==E:/tmp/add3.png==
WebGetValue==ClicktoUpload==upload.上传文件==

WebSetValue==upload.上传文件2==E:/tmp/add3.png==
WebGetValue==上传文件2==upload.上传文件2==
WebSetValue==upload.上传文件2==删除==xxx.png==

WebSetValue==upload.文件上传3==预览==image.png==
```

对象匹配规则
```xml
<app-tag name="AntdUpload" desc="文件上传" className="cn.lz.web.plugin.antd.obj.AntdUpload" typeName="WebObject">
   <node tag-name="span" class="ant-upload-wrapper"/>
</app-tag>
```


***

### antd vue 3

【[antd vue v3 的 upload 组件](https://www.antdv.com/components/upload-cn)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/upload/antdv.gif "执行效果")

**自动化脚本**
```
WebDefPage==upload==表单==upload==

WebSetValue==upload.上传文件==E:/tmp/add3.png==
WebGetValue==上传文件名称==upload.上传文件==
WebSetValue==upload.上传文件==删除==xxx.png==

WebSetValue==upload.图片上传==E:/tmp/add3.png==
WebGetValue==上传文件2==upload.图片上传==
WebSetValue==upload.图片上传==删除==xxx.png==

WebGetValue==上传图片文件名称==upload.上传图片文件==
WebSetValue==upload.上传图片文件==预览==image.png==
```

对象匹配规则
```xml
<app-tag name="AntdvUpload" desc="文件上传" className="cn.lz.web.plugin.antdv.obj.AntdvUpload" typeName="WebObject">
   <node tag-name="div" class="ant-upload"/>
</app-tag>
```



***

### element ui 2

【[element ui 2 的 upload 组件](https://element.eleme.cn/#/zh-CN/component/upload)】的执行效果

![执行效果](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/upload/eui.gif "执行效果")

**自动化脚本**
```
WebDefPage==upload==功能演示==upload==
WebSetValue==upload.上传图片==E:/tmp/add3.png==
#WebSetValue==upload.上传图片==E:/tmp/image.png==
WebGetValue==点击上传==upload.上传图片==

WebSetValue==upload.上传文件==E:/tmp/add3.png==
WebGetValue==上传文件==upload.上传文件==

WebSetValue==upload.上传图片==删除==food2.jpeg==
WebGetValue==点击上传==upload.上传图片==
```

对象匹配规则
```xml
<app-tag name="EuiUpload" desc="文件上传" className="cn.lz.web.plugin.eui.obj.EuiUpload" typeName="WebObject">
   <node tag-name="div" class="el-upload"/>
</app-tag>
```

***

## 对象封装

### Upload

公共方法参见WebNode和WebObject

针对Upload组件的特殊封装包括：setValue，getValue两个方法。

setValue的参考流程。

```java
// 上传文件
protected void uploadFiles( String value )
{
   if( value == null || value.isEmpty() ) {
      return;
   }

   // 需要上传的文件
   List<String> fileNames = StringUtil.splitItems( value );
   
   // 查找对象
   WebObject webObj = getH5Object();

   // 上传文件
   uploadFiles( webObj, fileNames );
   
   // 检查文件状态
   waitEcho( webObj, fileNames );
}

// 上传文件
protected void uploadFiles( WebObject parent, List<String> fileNames )
{
   UploadBox box = getUploadBox( parent );
   for( String fileName : fileNames ) {
      // 重试三次，页面初始化期间点击按钮无效
      for( int i=0; i<3; i++ ) {
         try {
             // 点击图标
             WebFactory.$(box.button).click();
             
             // 上传文件
             WebUtil.uploadFile( fileName );
             
             break;
         }
         catch( LzException e ) {
            if( "选择文件的窗口不存在".equals(e.getMessage()) ) {
               // click 没有触发，需要重试
               continue;
            }
            
            throw e;
         }
      }
   }
}
```

**开放接口**

不同的UI组件库，需要定制封装的部分。

| 名称 | 说明 |
| --- | --- |
| getAllItems | 取节点信息，包括按钮、文件(状态、删除按钮)等 |
| getFileItem | 取文件节点，包括状态、删除按钮等 |





