**关于selenium和selenide的关系和定位**

为什么有了selenium还要再出一个selenide（据说这两个工具都是同一批人贡献的）。难道selenium功能不够吗，或者为什么不把这两个工具合并呢。

**selenium** 定位在SDK层，SDK需要绝对的完备性，和最大的灵活性，易用性是是一个比较靠后的主表。selenium把对象定位和对象操作拆分成完全独立的两个过程。通过对By的封装和级联使用，可以满足绝大部分的使用场景。对象操作比较简单，输入层面实现了鼠标和键盘的模拟输入，通过js脚本修改对象属性或CSS属性；输出层面可以通过接口读取对象属性和CSS属性。selenium也支持js脚本，api做不到的事情，通过js都可以完成。在应用层面，selenium只能说提供了一套API 可以驱动浏览器，如果想做成一个稳定的自动化脚本，或者执行速度非常慢，或者脚本非常复杂。在SDK层面上开发应用，都会面临极大的挑战（麻烦）。

**selenide** 定位在工具成，是selenium的一个封装（一个浅层次的封装，到不了QT和win32 SDK的这种关系）。工具的特点是很容易被其他工具使用，并且也简化了SDK的使用难度，并且降低了SDK的灵活性。selenide虽然是一个简单的封装，但是实现方式非常完美，对外的接口也非常简单。selenide封装了一个虚的WebElement对象，把By直接变成了WebElement，把Action封装成了Element的方法，把WebDriverWait.until封装成了Element的一系列waitUntil和should方法。selenide把一切都封装到了SelenideElement里，并且自带错误重试，极大的简化了语句。

selenium需要面对各种应用场景，selenide只是一个简化自动化测试的工具。因为selenide只是一层浅封装，所以完全可以把selenium和selenide结合起来使用，在脚本里可能都无法区分哪个是selenium的功能，哪个是selenide的功能。

**关于UI自动化测试的原理**

无论是哪种自动化测试工具或平台，底层的逻辑都很简单：对象定位、对象识别和对象操作。绝大部分的自动化工具只操作DOM的最小单位 WebElement，所以不需要对象识别这个过程。比如selenium，Playwright等这些工具都只有对象定位和对象操作这两种功能。

**对象定位：** 直白的说就是查找UI对象。查找对象的方式有很多，不同的定位方式需要不同的参数。比如下面这三种都是指向同一个对象：GPS(39.8648536559,116.3792896271)、北京市丰台区永外大街车站路12号、北京南站。早期的自动化工具用第一种方式确定对象；xpath相当于第二种方式，区别在于地址是稳定的，而xpath经常变更。这是题外话了。如果存在用第三种方式操作对象那肯定是最理想的，即使把车站从丰台搬到顺义也能找得到。

**对象操作：** 驱动一个对象完成一个动作。可以是模拟鼠标键盘操作；也可以是通过js触发对象的event；或者从对象中读取了一个属性。单个Action，基本上问题不大。

**selenium是怎么实现自动化的**

抛开各种辅助的功能，selenium提供了两个核心功能，By和Action。By完成对象定位，Action完成对象操作。selenium提供了8种对象查找方法（内置的By），这个在 `org.openqa.selenium.By` 类里有说明，就不在这里啰嗦了。Action比较简单，就是一些鼠标操作和键盘操作功能。

By可以级联，Action也可以级联，甚至可以在By中使用Action（没有副作用的那种Action）。因此任何一个自动化的指令，最终都可以抽象成一个By和一个Action，比如要在【[select](https://element.eleme.cn/#/zh-CN/component/select)】中选择一个值，可以封装一个By包括三个步骤：查找Select，用鼠标点击Select，在弹出窗口中找到目标项。

```java
// 自定义By的伪代码
public class BySelectItem extends By
{
    private String xpath;
    public ByClearIcon( String xpath )
    {
        this.xpath = xpath;
    }

    @Override
    public WebElement findElement(SearchContext context)
    {
        // 先查找select对象，然后 click
        WebElement selectNode = By.xpath(xpath).findElement(context)
        Actions.moveToElement(selectNode).click().build().perform();

        // 查找弹窗
        WebElement popupWindow = By.cssSelector(弹窗路径).findElement(context);

        // 在弹窗中查找item
        return By.cssSelector(可以是text).findElement(popupWindow)
    }
}

// 不需要循环，不需要 try-catch
ByClearIcon itemBy = new BySelectItem( select路径 );
WebDriverWait.until( itemBy );
WebElement itemNode = itemBy.findElement(WebDriver)
Actions.moveToElement(itemNode).click()
```

为什么要封装（复杂）By，因为UI自动化测试脚本和我们平时的程序开发有很大的差异。自动化脚本和被测系统（这里是浏览器）是并行执行的，甚至浏览器内部也有大量的并行处理（异步处理）。所以说脚本在找到对象和执行对象这段时间，浏览器并没有消停下来，这中间大概率页面发生了变化，脚本找到的那个对象已经不是原来的那个对象了。

```java
// 示例代码，省略了 WebDriverWait.until
// 1、找到对象
WebElement selectNode = By.xpath(select路径).findElement(context);

// 2、然后点击
Actions.moveToElement(selectNode).click().build().perform();

// 3、查找弹窗
WebElement popupWindow = By.cssSelector(弹窗路径).findElement(context);

// 4、在弹窗中查找item并点击
WebElement itemNode = By.cssSelector(可以是text).findElement(popupWindow)

// 5、点击目标元素
Actions.moveToElement(itemNode).click().build().perform();
```

这个脚本大概率不能稳定执行，今天能行，明天可能会失败。
- 前两步一般不会错，因为select对象是页面上的静态内容，第二步使用的对象基本上就是第一步找到对象。
- 后面三步操作的都是动态内容，第四步用了第三步找到的对象，第五步用了第四步的对象，期间页面一直在变化，所以不保证是同一个对象，尤其是加了效果的UI组件。

那么封装成一个By会有什么好处。把前四步封装成一个BySelectItem，WebDriverWait.until会负责这个By的错误重试，把前四步捏成一个整体，要么全对，要么全错，这显然会增加脚本的执行稳定性。（第五步的漏洞没法填补，除非2-3之间加延迟，等待弹窗全部打开）。

所以说这种模式其实很优雅，但技术很复杂，也很难驾驭。有多少人能够抽象并构造自己的By，又或者按这种方式实施自动化的代价有多大呢。

**最后说明：** 即使封装成BySelectItem，错误也迟早会发生。即使弹窗成功打开，并且成功找到item。因为5操作的是动态元素，此时弹窗一直在伸展，找到的时候第三个对象还在第一个位置，点的时候popup全展开了，点错的概率很大。我用了前面那段代码，一开始还可以，但是有一天全部出现了错误。

**selenide做了哪些工作**

在自动化脚本中实现封装By和Action是不太现实的，因为难度太大，而且脚本很长很难维护。

那么selenide做了什么呢。selenide把By、Action和Wait封装在了一起，而且很优雅，并且是开放的，扩展能力很强，对二次开发很友好，并且是MIT liscene。

selenide的结构图

![selenide的结构图](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/skill/selenide/selenide.png "selenide的结构图")

SelenideElementProxy是一个proxy，并不是一个实现类。SelenideElement提供了很多api，这些api都会调用SelenideElementProxy中的同一个方法（invoke），SelenideElementProxy在invoke实现了总控和错误重试，根据api的名称查找对应的Command，并执行Command。

selenide封装了一个SelenideElement对象，这是一个 **伪 WebElement** 对象，具有WebElement的功能，但又不同于WebElement。WebElement对应浏览器中一个实实在在的DOM对象，而SelenideElement只是定义了一套查找对象的规则（By）。所以在创建SelenideElement时他不是WebElement，他只是一个简单的java对象，在用到SelenideElement时，他会自动去浏览器查找对象，变成一个真正的WebElement，这个过程脚本不需要关心。SelenideElement还有一个好处是，每次使用都会重新去浏览器查找对象，为错误重试提供了比较方便的途径。这么做的最大好处是，可以先构造几个Element并把他们级联起来（父对象中查找子对象），执行动作时这些Element被当作一个整体执行，相当于实现了By的级联。相当于java的stream处理，用到才执行。这就给了SelenideElement一种串联的可能。

selenide把By封装成了SelenideElement，把Action封装成了SelenideElement的方法，把WebDriverWait.until封装成了SelenideElement中的一系列waitUntil和should方法。selenide把一切都封装到了SelenideElement里。并且可以无限串联，并且可以串联Action。比如： WebDriver.查找对象().查找子节点().再查找子节点().doAction()。selenium内置的By不能串联，只能先找到父对象、再用父节点查找子对象；selenide的这种串联，只有当doAction被执行时，才会执行前面的By，这个语句是一个整体，不是四段，或者四个操作，这里只有一个操作。

selenide在每个Action里都统一增加了超时和错误重试机制，如果By错误或Action错误都会重试整个指令。相当于把findElement和Actions包装成了一个语句，并且在外层加了超时和错误处理。

selenide还间接修改了WebDriverWait.until。原始的until一般会等待一个较长的时间，比如等待20秒等等，但是在until期间程序是没有控制器的，如果你想做个调试工具，这个时候脚本没法中断执行，只能傻傻的一直等到until结束。selenide对SelenideElement的每个动作，做了一个统一的循环和超时处理，所以在调用WebDriverWait.until时用了一个极小的超时时间，给了程序一个中断执行的机会。

具体看一下selenide的脚本，针对单个Action，selenide能提高执行稳定性。但是多个Action配合执行时，无能为力。

```java
// 上面的例子，在select中选择内容
SelenideDriver.$x(select路径).click();
SelenideDriver.$(弹窗路径).$(选择项特征).click();
```

selenide提供了一些简洁的写法
| 语句 | 说明 |
| --- | --- |
| $ | 相当于 css selector，也支持直接传入By，支持传入WebElement，支持index |
| $x | 相当于 xpath，支持index |
| $$ | 相当于 css selector 的 findElements |
| $$x | 相当于 xpath 的 findElements |

selenide把Action封装到了SelenideElement中，但是做了兼容性和适应性处理，比如setValue可以操作input.text，也可以操作select，input.check等，或者通过js操作IE的input。每个Action都有错误重试功能，在一些场景里，如果想自己控制超时，可以用SelenideElement.waitUtil在Action以前先检查对象是否可用。

对象的主要Action
| 语句 | 说明 |
| --- | --- |
| setValue | 执行前判断对象类型，支持text、select和radio等，如果是ie环境，setValue用js实现。如果是text,赋值前先清空内容，用了Element.clear和sendKeys(Keys.DELETE)双重机制清空数据，赋值用 sendKeys。select和radio特殊处理 |
| append | 和setValue一样，只是不清空数据 |
| click, contextClick, doubleClick | 鼠标左键、右键或双击，ie用js模拟实现 |
| hover | moveToElement的封装 |
| dragAndDropTo | dragAndDrop 封装 |
| toString | 把attrs读出来，再生成一个 `<tag ...>` 调试的时候有用 |
| attr, innerText, innerHtml, cssValue, name, text | 取属性，或css属性等 |
| getValue, getText | 取值 |
| clear | 清空 |
| pressEnter, pressCancel 等| 键盘快捷键 |
| shoud, waitUntil deng | 一系列的等到，和状态检查函数 |
| execute | 执行自己的 Command |

浏览器的主要函数，封装在SelenideDriver里
| 语句 | 说明 |
| --- | ------------------ |
| open | 打开浏览器和页面 |
| refresh, back, forward | 刷新页面等 |
| zoom | 放大缩小 |
| executeJavaScript | 执行 js |
| title, url, source | 浏览器标题、地址、页面内容等 |
| switchTo | alert等弹窗 |
| screenshot | 截图 |
| download, upload | 上传下载，没有试过 |
| getClipboard | 剪贴板 |
| getSessionStorage, getLocalStorage, getSessionId | 本地数据 |

看上去内容不是很多，如果有selenium的基础，这个半天就全搞定了。

selenide简化了自动化脚本的语句，针对单个动作增加了错误重试机制，但是脚本整体的稳定性还是需要在程序中对selenide进行二次封装。

