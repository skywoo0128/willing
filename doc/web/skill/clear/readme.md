**如何解决WebElement.clear无效的问题**

输入input.text的值以前，需要先清空原来的值。

前段时间封装element ui组件库，操作input.text都很正常。

```java
private void setValueForTextInput(WebElement element, String text)
{
   if (text == null || text.isEmpty()) {
      element.clear();
   }
   else if (WebConf.isFastSetValue()){
      // ie
      String error = setValueByJs(element, text);
      if (error != null){
         throw WebErrors.invalidState(error);
      }
      else {
         // ie9以下不支持oninput
         events.fireEvent(element, "keydown", "keypress", "input", "keyup", "change");
      }
   }
   else {
      // chrome、firefox等
      element.clear();
      element.sendKeys(text);
   }
}
```

后来封装antd的组件库时，发现 WebElement.clear 不起作用。网上查了一下发现有很多解决方案，最终都需要通过 `sendKeys( DELETE )` 来清空内容，有点不甘心，毕竟凭空多出了这么多指令。

后来去查了一下官方版（selenide）的实现方法，虽然也是`sendKeys( DELETE )` ，但感觉要优雅一点。网上的解决方案都需要先鼠标 click，然后键盘 delete；selenide只需要键盘操作，并且支持windows和MAC。而且鼠标操作要比键盘费时。

代码如下
```java
new Actions(WebDriver)
   .sendKeys(input, "0")
   .keyDown(modifierKey).sendKeys("a").keyUp(modifierKey).sendKeys(Keys.DELETE)
   .perform();

// modifierKey 在 windows和MAC中不一样
// selenide判断平台是通过浏览器的 navigator.platform 实现
// 这里的 Platform 是 jna 中的功能
CharSequence modifierKey = Platform.isMac() ? Keys.COMMAND : Keys.CONTROL;
```



