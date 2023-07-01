**innerText取不到内容**

用js取Element的内容，一直都是用Element.innerText。虽然一直警告innerText是IE的特产，但是其他浏览器都可以使用，一直没有问题。

今天在测试【[element ui 2 的 table 组件](https://element.eleme.cn/#/zh-CN/component/table)】时，神奇的事情发生了。**固定列和表头** 那个表格，第一列的内容全是空值（表头和表格内容），后面几列的内容正常。

**bing 的回答**

在 `bing` 上问了一下。

`why can not get innerText`
```
innerText may not work for several reasons. One reason is that it is the "old Internet Explorer" way of doing it, and it is better to use textContent instead.
Another reason is that innerText won't work cross-browser, so it is better to use the standard way to put text with textContent.
If you are using jQuery, you can use jQuery("#the_id_here").text()
Finally, innerText gets only the string inside the element, so you should use the element instead of just only the string
```

`why innerText return empty`
```
innerText represents the rendered text on a page, which means applying the element’s white-space rules. The effect in your case is that surrounding whitespace is stripped off
innerText is not a standard property and was introduced by Microsoft into Internet Explorer, but no other browsers are guaranteed to support it
Use element.textContent or rethink your approach
textContent acts like a PRE element and gets the source code from the HTML, including tabs, spaces, lack of spaces, and line breaks
```

没解释什么原因，只是说做个api太老，不兼容，改成 `textContent`


**改成textContent有没有副作用**

没敢大范围修改，按上面的说法，textContent和innerText返回的值本来就有差别，所以出问题的地方改成了 `textContent` ，其他的先将就吧。




