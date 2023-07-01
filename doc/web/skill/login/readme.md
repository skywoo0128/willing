**为什么要封装login**

和很多人聊过关于web自动化测试时，执行完一个脚本后怎么继续执行下一个脚本。大部分人都说把浏览器关了，然后再打开，再登录。

一开始我很惊讶，后来也就习惯了。有些系统的用户不允许多次登录，比如银行的柜面交易系统，有些信贷系统也这么控制，如果前一个脚本登出失败，意味着后面的测试样例都不能执行了。

而且打开浏览器，用户登录都是非常耗时的操作。尤其后台管理系统在登录时需要计算权限，并生成前端菜单和按钮，生成dashboard。所以很多系统要求早上分段登录。遇到过很多系统，单次登录需要接近2分钟，被迫把超时时间延迟到2分以上。

即使是最简单的系统，打开浏览器并完成用户登录，这个过程也需要5秒以上。就算一个中等规模的自动化用例库，1000个测试用例，折算成3000个自动化脚本，（绝大部分的测试用例都是交易链，至少需要挂接一个检查点脚本）。几乎要多浪费一台执行机一个晚上的执行时间。

**login中应该包含哪些内容**

如果只是封装一个login的表单，这个可能只需要3`5行代码就可以，但这显然没法满足脚本连续执行的要求。

如何才能让自动化脚本连续执行，封装一个login一般需要考虑以下这些内容。

- 关闭所有windows系统的对话框，如果上一个脚本在执行上传文件时发生了错误，所有可能有一个打开文件的窗口没有关闭
- 如果允许打开多个浏览器窗口，需要关闭其他窗口，只保留主窗口
- 判断当前页面是登录前的页面还是登录后的页面，或者说是不是已经有用户登录了
- 如果是登录后的页面，需要关闭所有已打开的窗口，包括js生成的弹窗、侧边窗口、交易页面窗口（如果应用可以打开多个窗口，通过页签切换）
- 如果左侧的功能目录是可以隐藏的，需要判断是否已隐藏，并恢复
- 如果页面是当前用户登录的，需要跳转到首页
- 如果页面的登录用户不是当前要登录的用户，需要登出前一个用户
- 用户登录前，先判断用户的cookie是否还有效，（用户的cookie可以是接口测试产生的，也可以是web页面登录时保存的，避免用户重复登录）。如果cookie有效，需要把cookie注入到浏览器然后刷新页面，检查是否能够进入到交易页面。
- 以上尝试都失败后，启用表单登录
- 登录成功后保存cookie，以便后面的测试用例，或接口测试能够使用这个登录信息。



**一个样例**

这是一个vue+element UI的web应用，只要底层封装的足够，封装完一个login的流程，后面基本上都可以复用。

```java
public static void login( String 用户名, String 密码 )
{
   // 关闭所有系统弹出的对话框 #32770，比如上传文件的弹窗
   LoggerUtil.writeLog( "关闭所有对话框 #32770" );
   long browseWnd = IWebTools.getInstance().getBrowseHwnd( 0 );
   if( browseWnd != 0 ) {
      // windows 系统才会返回值
      HWND parentWnd = new HWND( new Pointer(browseWnd) );
      WindowUtil.closeAllDialog( parentWnd );
   }
   
   // 等待页面加载完成
   WebClient.waitPageLoaded( 10000 );
   
   // 检查是否已经登录
   LoggerUtil.writeLog( "检查用户是否已登录" );
   WebEnvImpl env = WebEnvImpl.getInstance();
   String oldName = env.getUserName();
   if( $("img.user-avatar").findInLayer(false).exists() ) {
      // 页面已加载完成
      WebClient.waitPageLoaded( 20000 );

      // 关闭所有打开的页面（可以打开多个交易页面）
      WebClient.closeAllPage();
      
      // 判断是否已登入
      if( 用户名.equals(oldName) ) {
         // 左侧的菜单
         $("div.sidebar-container>#AtpSwitch").findInLayer(false).waitLocated( 3000 );
         if( !$label("固定为侧边栏", "#AtpSwitch").findInLayer(false).exists() ) {
            WebObject obj = $("div.sidebar-container>#AtpSwitch").findInLayer(false);
            obj.click();
         }
         
         // 返回到首页
         $title("首页", "li").click();
         
         // 刷新session的时间
         MiscUtil.setCookieTime( env.getLoginUrl(), 用户名 );
         
         return;
      }
      else {
         // 退出当前用户
         clickLogout();
      }
   }
   
   // 查找用户的session
   boolean isLogined = false;
   List<CookieItem> list = MiscUtil.getCookie( env.getLoginUrl(), 用户名 );
   if( list != null && !list.isEmpty() ) {
      // 用户已登录，只需要注入cookie，不需要再登录
      WebClient.injectCookie( list );
      
      // 刷新页面
      WebClient.refresh();
      
      // 判断是否已登录（或者进入首页，或者停留在login页面）
      WebClient.waitObjectLoaded( 20000, $title("首页", "li"), $hint("用户名", "input") );
      if( $title("首页", "li").findInLayer(false).exists() ) {
         isLogined = true;
      }
   }
   
   // 用户登录
   if( !isLogined ) {
      $hint("用户名", "input").setValue( 用户名 );
      $hint("密码", "input").setValue( 密码 );
      $title("登录", "button").click();
      LoggerUtil.writeLog( "用户登录[" + 用户名 + "]" );
   }
   else {
      LoggerUtil.writeLog( "用前一次的Session登录[" + 用户名 + "]" );
   }
   
   // 保存用户名
   env.setUserName( 用户名 );
   
   // 监控网络
   WebClient.waitPageLoaded( -1 );
   WebClient.netMonitor();
   
   // 登录时保存session
   if( !isLogined ) {
      $title("首页", "li").findInLayer(false).waitLocated( 6000 );
      WebClient.saveCookie();
   }
   else {
      // 刷新session的时间
      MiscUtil.setCookieTime( env.getLoginUrl(), 用户名 );
   }
}
```

**执行效果**
![执行效果图](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/skill/login/login.gif "执行效果图")

login以前，被测系统打开了一个上传文件的窗口、一个js生成的弹窗，打开了一个功能页面，左侧功能菜单被折叠。login执行完成后，系统成功被复原，跳转到了首页。

