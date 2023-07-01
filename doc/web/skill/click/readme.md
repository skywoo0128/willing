**如何修改moveToElement的延迟时间**

selenium的鼠标操作非常慢、如何缩短selenium的鼠标操作时间

selenium的鼠标操作主要包括5个动作：mouse move，click，context click，double click，drag&drop。后面四个指令首先需要用到moveToElement。比如

```java
new Actions(WebDriver).moveToElement(element, offsetX, offsetY).click().build().perform();
new Actions(WebDriver).click(element).build().perform();
```

`click(WebElement)`在实现时也调用了`moveToElement`，代码如下

```java
public Actions click(WebElement target) {
   return moveInTicks(target, 0, 0).clickInTicks(LEFT);
}

// moveToElement就是调用了moveInTicks
private Actions moveInTicks(WebElement target, int xOffset, int yOffset) {
    return tick(getActivePointer().createPointerMove(
        Duration.ofMillis(100),
        Origin.fromElement(target),
        xOffset,
        yOffset));
}
```

但是在Actions中实现的moveToElement速度很慢，上面的代码中调用了 `Duration.ofMillis(100)` ，意味着每个鼠标操作至少需要100毫秒。而dragAndDropBy更是需要250毫秒以上。

```java
public Actions dragAndDropBy(WebElement source, int xOffset, int yOffset) {
    return moveInTicks(source, 0, 0)
        .tick(getActivePointer().createPointerDown(LEFT.asArg()))
        .tick(getActivePointer().createPointerMove(Duration.ofMillis(250), Origin.pointer(), xOffset, yOffset))
        .tick(getActivePointer().createPointerUp(LEFT.asArg()));
}
```

正常情况下，一个指令100多毫秒勉强也可以接受，但是遇到一些特殊的组件，需要大量的鼠标操作，比如一个支持多选的【[cascader](https://element.eleme.cn/#/zh-CN/component/cascader)】组件，一个指令就需要10多个鼠标点击动作。我在封装组件时，发现一个支持多选的【[treeselect](https://www.vuetreeselect.cn/)】组件，选择三个值据然用了1.5秒。

进到Actions类，才发现每个click都非常耗时，目前不能确定selenium为什么要这么设计。

**修改后的Actions**

```java
public static class MyActions extends Actions
{
   public MyActions(WebDriver driver)
   {
      super( driver );
   }
   
   public Actions moveToElement(WebElement target, int xOffset, int yOffset, long millis)
   {
      return tick(getActivePointer().createPointerMove(
            Duration.ofMillis(millis),
              Origin.fromElement(target),
              xOffset,
              yOffset));
   }

   public Actions moveByOffset( int xOffset, int yOffset, long millis )
   {
      return tick(
            getActivePointer().createPointerMove(Duration.ofMillis(millis),
                  Origin.pointer(),
                  xOffset,
                  yOffset)
            );
   }
   
   public Actions dragAndDrop( WebElement source, WebElement target, int offsetX, int offsetY )
   {
      MyActions e = (MyActions)moveToElement(source, 0, 0, 10)
            .tick(getActivePointer().createPointerDown(LEFT.asArg()));
      
      return e.moveToElement(target, offsetX, offsetY)
            .tick(getActivePointer().createPointerUp(LEFT.asArg()));
   }
}
```

最后一个dragAndDrop函数，是为了支持把对象拖拽到目标对象的指定位置。在【[slider](https://ant-design.antgroup.com/components/slider-cn)】组件中需要用到这个功能。这个slider组件支持多个滑块，所以想在slider上通过click来设置滑块的位置，基本不可能。

修改后的click指令
```java
new MyActions(WebDriver).moveToElement(element, offsetX, offsetY, 20).click().build().perform();
```
我把Duration的时间缩短到20毫秒，暂时没有发现副作用，速度提高了很多。

对于需要使用大量鼠标操作的组件，比如 cascader、treeselect、不能修改值的datepicker、transfer 等组件，这个修改还是很有必要的。

