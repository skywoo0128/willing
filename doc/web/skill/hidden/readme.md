**js中怎么判断对象是否可见，未解决**

查找对象时，经常要判断对象是否可见，比如判断cascader的下拉框有没有打开，Message的弹窗是否可见等等。

以前写了一段js代码，在很多对象查询程序中用到，一直以来用的都没有问题，代码如下

```javascript
// 对象是否可见
function _isHidden(obj){
   if(obj === document.body) return false;
   var attrs = obj.currentStyle ? obj.currentStyle : document.defaultView.getComputedStyle(obj, null);
   if(attrs.position !== 'fixed' && obj.offsetParent === null){
      return true;
   }
   
   return (attrs['display'] === 'none' || attrs.visibility == 'hidden');
}
```

在封装Antd的【[upload](https://ant-design.antgroup.com/components/upload-cn)】组件时，有两个小图标，预览文件和删除文件，使用上面这段代码判断是可见的，但是在click这两个图标时，一直报对象不可见。

可能是因为[opacity=0]的缘故吧。我在js里没有判断opacity属性，是因为这个属性需要逐级遍历对象，一直到body节点才能最终计算出值，担心这个过程比较耗时。

最后的解决办法，在取到对象后，再通过selenium的函数 `WebElement.isDisplayed` 判断对象是否可见。

