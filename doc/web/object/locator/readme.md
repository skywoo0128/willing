# selenium的对象定位

selenium通过By查找对象，然后用Action操作对象。selenium原生提供了8种对象查找方法，这个在 `org.openqa.selenium.By` 类里有说明，就不在这里啰嗦了。

By是一种很优雅的设计模式，通过级联By可以设计出很复杂的对象定位逻辑，甚至在By中也可以调用Action。在串联的By中，每个By只是定义了对象查找逻辑，但他并不会立刻执行，只有当最外层的By调用了 `By.findElement` 方法时，整个链路才会逐步往里传导，直到最内层的By被调用并返回对象。但是selenium提供的八种By并不直接支持By级联（需要通过WebDriverWait.until）。

为什么说这是一种很好的模式，看下面这个例子。我需要点击 【[select](https://element.eleme.cn/#/zh-CN/component/select)】组件(可清空单选)的clear图标，用来清空select的内容。但是正常显示时，select只显示下拉箭头，只有当鼠标移到select组件上时，才显示clear图标。

**原生selenium脚本**
```java
// 具体xpath就不写了
// 先查找select对象
WebElement selectNode = WebDriver.findElement( By.xpath(select路径) );
Actions.moveToElement( selectNode )

// 在select对象内部查找 clear 图标，并点击
WebElement iconNode = selectNode.findElement( By.cssSelector(icon的className等) );
Actions.moveToElement( iconNode ).click()
```

这段脚本可能会很不稳定，需要加个显示等待。

**稍微稳定一些的selenium脚本**

```java
// 先查找select对象
ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfElementLocated( By.xpath(select路径) );
WebElement selectNode = WebDriverWait.until( cond );
Actions.moveToElement( selectNode )

// 在select对象内部查找 clear 图标，并点击
ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfNestedElementLocatedBy( selectNode, By.cssSelector(icon的className等) );
WebElement iconNode = WebDriverWait.until( cond );
Actions.moveToElement( iconNode ).click()
```
自动化指令出错的概率非常高。第一个moveToElement如果没起作用，会导致clear图标不显示。或者查找到的iconNode可能还在下拉箭头下（查找速度太快，clear图标的opacity=0，还没显示出来），点击一个透明的，或被覆盖的对象也会报错。如果想稳定（稍微）的执行上面这段代码，外面还需要加一个大循环，再加一个try-catch语句，并且需要判断是否超时。

**修改以后的语句**
```java
while( 直到超时 ){
    try{
       // 先查找select对象
       ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfElementLocated( By.xpath(select路径) );
       WebElement selectNode = WebDriverWait.until( cond );
       Actions.moveToElement( selectNode )

        // 在select对象内部查找 clear 图标，并点击
        ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfNestedElementLocatedBy( selectNode, By.cssSelector(icon的className等) );
        WebElement iconNode = WebDriverWait.until( cond );
        Actions.moveToElement( iconNode ).click()
    }
    catch( RuntimeException e ){
        if( 如果没有超时 ){
            // 继续尝试上面的过程
            wait( 50毫秒 )；
        }
        else{
            throw e;
        }
    }
}
```


## 封装一个 By
**这个处理看上去太复杂了，封装一个By会怎么样？**

```java
// 查找clear图标的 By
public class ByClearIcon extends By
{
    private String xpath;
    public ByClearIcon( String xpath )
    {
        this.xpath = xpath;
    }

    @Override
    public WebElement findElement(SearchContext context)
    {
        // 先查找select对象，然后 hover，再查找clear图标
        WebElement selectNode = By.xpath(xpath).findElement(context)
        Actions.moveToElement( selectNode )

        // 在select对象内部查找 clear 图标
        return By.cssSelector(图标的tagName.className).findElement(selectNode)
    }
}

// 不需要循环，不需要 try-catch
WebElement iconNode = WebDriverWait.until( new ByClearIcon(select路径) );
Actions.moveToElement(iconNode).click()
```
如果需要，还可以在ByClearIcon中加个 `xpath( String xpath )` 之类的静态方法

通过封装以后，在脚本里清空select内容就清爽多了。而且ByClearIcon大概率能返回对象，因为如果产生错误会在until中重试直到超时。这个自定义的By，把两个不相干的By和一个Action组装成了一个整体。

但这样有个很大的问题，总不能到处封装By吧。有没有好一点的解决方法。

# selenide怎么处理这个问题

selenide是官方提供的selenium封装，主要就解决了一个问题（其他一些快捷函数，比如SwitchTo等等并不重要），By串联的问题。

selenide把By封装成了伪WebElement，相当于去掉了By.findElement这一层。selenide封装了一个SelenideElement对象，把By封装成了SelenideElement，把Action封装成了SelenideElement的方法。并且SelenideElement具有WebElement的功能，并且可以无限串联，并且可以串联Action。

```java
// selenide的处理代码
SelenideDriver.$x(select路径).hover().$(图标的css定位).click()

// 可能写成这样更容易理解
SelenideElement selectNode = SelenideDriver.$x(select路径).hover();
selectNode.$(图标的css定位).click()
```

这个是不是很简洁，`$x和$` 只是定义了对象查找规则，因此并不需要waitUntil处理。selenide在每个动作中自动增加了waitUntil、超时处理和错误重试功能，确保hover和click两个动作各自成功处理（动作触发了，但是浏览器有没有处理不能确定，这个在[如何提高脚本的执行稳定性](../../skill/wait/readme.md)）有说明。

但是这个并没有解决hover虽然执行了，但不一定产生效果，导致click一直在等待图标的出现。当然如果hover起了作用，click一定能找到图标并点击。这个问题主要是因为hover和click是两个完全独立的动作，每个selenide的动作只能重试离他最近的那些By，并不能重试前一个动作。

```java
// 比如这个语句包含了两个By和一个动作，如果click错误会重试前两个By。
SelenideDriver.$x(select路径).$(图标的css定位).click()

// 注意，这个语句中的$x和$都是不会立刻执行的，只有当调用click时，才会执行
// 所以和下面的程序有本质的区别，下面这个语句，在click以前对象已经查找过了。
WebElement selectNode = By.xpath(select路径).findElement(WebDriver)
WebElement iconNode = By.cssSelector(icon的className等).findElement(selectNode)
Actions.moveToElement( iconNode ).click()
```

selenide虽然解决了By串联和重试的问题，但是也存在一个问题，hover和click这些动作语句中，selenide自动增加了一个错误重试和超时处理的过程。如果 `$(图标的css定位).click()` 中找不到clear图标，会一直等待直到超时。所以即使在这个语句外加了循环和错误重试，也是不起作用，因为click超时意味着整个语句超时了。除非设置click的超时时间，click超时后立刻重试整个过程。可以在click以前加一个SelenideElement.waitUntil，当对象出现后再click。

在selenide中，hover和click都是封装成Command，我们也可以自己在程序里封装一个Command，然后交给selenide执行，借用他的错误处理机制。这样的话我们需要像封装ByClearIcon一样封装一个ClearSelectCommand。这还不如封装一个By，在selenide中也可以用自定义By，比如

```java
// 在Selenide中使用自定义By
ByClearIcon by = new ByClearIcon(select路径);
SelenideDriver.$(by).click();
```
这个语句会一直重试直到click成功。


## selenide补充的对象定位方法

selenide原生的8种对象定位方法比较容易理解，因为那是W3C的标准，浏览器实现的功能，做过前端的人都会比较熟悉。虽然有8种对象识别的方法，但是selenide也只封装了 `By.xpath和By.cssSelector` 两种，因为其他六种非常不常用。

但是这八种方法中，只有 `By.id，By.name，By.linkText，By.partialLinkText` 能使用；`By.tagName和By.className` 一般返回数组，查找单个对象需要在极其罕见的情况下才能使用；`By.xpath和By.cssSelector` 需要一串很长的路径，既难阅读也没有容错能力。

但是在单页应用中，页面是通过变量生成，或者说页面依赖于变量，而变量不依赖于页面，所以页面中不会出现id和name这些属性，因为js不需要操作WebElement，所以也不需要在页面上查找对象，设置设置了很高的围栏禁止在js中查找对象。所以 `By.id，By.name` 也不能用了，而 `By.linkText和By.partialLinkText` 的使用范围及有限。所以用selenium测试单页程序基本上是给自己找不自在。

selenide封装了哪些对象查找方法。
|    By    |    说明  |
| -------- | -------- |
|ByAttribute| 这是一个By.cssSelector的语法糖，用attrName和attrValue生成了一个cssSelector，相当于[name="value"]|
|ByTagAndText| `.//{tagName}/text()[normalize-space = {text}]/parent::*"` 。此处有省略，因为normalize-space的写法太复杂，这个是text相同 |
| ByText | 一个简化的ByTagAndText，tagName=* |
| WithTagAndText | `.//{tagName}/text()[contains(normalize-space = {text})]/parent::*"` 。此处也有省略，这个是包含文本 |
| WithText | 一个简化的WithTagAndText，tagName=* |
| ByTextCaseInsensitive | 不区分大小写的text匹配 |
| WithTextCaseInsensitive | 不区分大小写，包含text |
| ByShadow | 查找shadow dom中的节点，只是浏览器的新特性，一个封闭的DOM树。这个可能接触的比较少，看上去像一个轻量级的iframe |

看上去这些方法也没有实质性解决对象查找的问题，只是简化了一些写法。

# 最后
`WebDriverWait.until 和 ExpectedConditions` 的写法非常啰嗦，我偶尔也需要通过 `findElement` 查找对象，所以干脆就封装了三个方法。如果不想直接调用 findElement获取对象，这三个方法可以参考。

```java
// 获取对象，直到对象可用
public static WebElement waitLocated( WebDriver driver, By by, int timeOut )
{
   if( by == null ){
      throw new NonExistException( "by is null");
   }
   
   // 查找对象
   ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfElementLocated(by);
   WebElement element = waitLocated( cond, timeOut );
   if( element == null ) {
      throw new NonExistException( "对象["+by.toString()+"]不存在,timeOut["+timeOut+"]");
   }
   
   return element;
}

// 获取子对象，直到对象可用
public static WebElement waitLocated( By parent, By by, int timeOut )
{
   if( parent == null || by == null ){
      throw new NonExistException( "by is null");
   }
   
   // 查找对象
   ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfNestedElementLocatedBy(parent, by);
   WebElement element = waitLocated( cond, timeOut );
   if( element == null ) {
      throw new NonExistException( "对象["+by.toString()+"]不存在,timeOut["+timeOut+"]");
   }
   
   return element;
}

// 获取子对象，直到对象可用
public static WebElement waitLocated( WebElement parent, By by, int timeOut )
{
   if( parent == null || by == null ){
      throw new NonExistException( "by is null");
   }
   
   // 查找对象
   ExpectedCondition<WebElement> cond = ExpectedConditions.presenceOfNestedElementLocatedBy(parent, by);
   WebElement element = waitLocated( cond, timeOut );
   if( element == null ) {
      throw new NonExistException( "对象["+by.toString()+"]不存在,timeOut["+timeOut+"]");
   }
   
   return element;
}

// 获取对象，直到对象可用
protected static WebElement waitLocated( ExpectedCondition<WebElement> cond, int timeOut )
{
   // 需要重试
   boolean retry = true;
   long endTime = Calendar.getInstance().getTimeInMillis() + timeOut;
   while( retry ){
      // 检查超时时间
      if( Calendar.getInstance().getTimeInMillis() >= endTime ){
         break;
      }
      
      try{
          WebDriverWait wait2 = ByUtils.newWait( timeOut );
          return wait2.until( cond );
      }
      catch( NoSuchWindowException ex ){
         throw new LzException(ErrConst.EXEC_ERR_CODE,"窗口已经关闭", ex);
      }
      catch( UnhandledAlertException ex ){
         Boolean rc = AlertHandle.processUnhandledAlert(ex);
         if( !rc ){
            throw new LzException(ErrConst.EXEC_ERR_CODE, "有未处理的弹窗", ex);
         }
         else{
            LoggerUtil.writeLog( "正在处理消息框，需要重新取对象" );
         }
      }
      catch( StaleElementReferenceException e ){
         LoggerUtil.writeLog( "等待更新引用", e );
      }
      catch( NullPointerException e ){
         LoggerUtil.writeLog( "等待更新引用", e );
      }
      catch( Exception e ){
         retry = false;
         LoggerUtil.writeLog( "未知错误", e );
      }
   }

   return null;
}
```



**题外话**

用selenium做过UI自动化测试的人，认为xpath对象识别就是个深坑，各种惨疼的教训和血泪史，建议后人不要用太长的xpath路径定位对象。而小白看到selenium能让网页自动填数、自动提交时，会无比兴奋，认为终于找到了银弹，自动化测试将一马平川。:blush::blush::blush:


