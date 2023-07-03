# Matrix 自动化测试工具

matrix是一个支持WEB、APP、接口测试和银行柜面系统的手工和自动化测试工具。matrix提供简洁的对象管理，快速脚本开发和调试能力。支持react、vue、antd、antdv、element ui等WEB前端架构和组件库的自动化测试。

通过对各种通用UI组件库 【[antd v5 组件库](https://ant-design.antgroup.com/components/overview-cn)】、【[antd vue v3 组件库](https://www.antdv.com/components/overview-cn)】、【[element ui2 组件库](https://element.eleme.cn/#/zh-CN/component/)】 的封装，matrix能完美支持react、vue等单页应用的自动化测试。

## 架构图
![架构图](https://raw.gitmirror.com/skywoo0128/willing/main/doc/img/framework.png "架构图")

工具使用了四层结构。
- 驱动层：包括selenium、appium、opencv等自动化基础组件。柜面驱动类似selenium，完成柜面系统的自动化接口。接口网关完成tuxedo、MQ等自动化通讯协议的扩展。win32驱动完成windows下图形界面的自动化接口。
- 引擎层：包含WEB测试、APP测试、接口测试和柜面测试的自动化测试封装、脚本执行和调试引擎。数据池管理、随机数管理和脚本参数化数据的管理和存取。SQL模块完成数据库的操作，可以同时支持各种数据库和各种版本。插件容器完成扩展程序和自动化项目插件的装载和热替换等功能。
- 扩展成：包括扩展程序和插件程序。扩展程序完成一些特殊被测系统的技术封装，包括web UI框架的封装（比如antd, element ui等）。或者接口报文和通讯协议的扩展等。插件程序完成特殊测试项目的扩展，比如封装login和logout等。
- 工具层：统一的对象库管理和脚本开发调试工具。以及测试用例分析工具、测试数据管理工具、自动化任务的执行和调度功能等。

## 主要功能
- **WEB自动化**：对象库维护、对象有效性检查、脚本开发和调试、流程编排、造数流程和测试数据生成、单节点自动化用例、流程自动化用例。

## 总体说明

[matrix概述](./doc/frame/abs/readme.md)


## WEB自动化

### 执行效果

*脚本执行效果图*

demo测试网站：[antd pro](https://preview.pro.ant.design/)

![脚本执行效果图](https://raw.gitmirror.com/skywoo0128/willing/main/doc/frame/abs/execute.gif "脚本执行效果图")

### 组件库（对象）封装

[组件封装概述](./doc/web/object/overview/readme.md)

#### 表单

[select 和 selects 组件](./doc/web/object/select/readme.md)

[cascader 和 cascaders 级联选择](./doc/web/object/cascader/readme.md)

[transfer 穿梭框](./doc/web/object/transfer/readme.md)

[treeSelect 和 treeSelects 组件](./doc/web/object/treeselect/readme.md)

[upload 上传文件](./doc/web/object/upload/readme.md)

[switch 开关](./doc/web/object/switch/readme.md)

[slider 滑块](./doc/web/object/slider/readme.md)

[rate 评分](./doc/web/object/rate/readme.md)

#### 日期和时间

[datepicker 选择日期](./doc/web/object/date/readme.md)

[monthpicker 选择月份](./doc/web/object/month/readme.md)

[weekpicker 选择周](./doc/web/object/week/readme.md)

[timepicker 选择时间](./doc/web/object/time/readme.md)

[datetimepicker 选择日期和时间](./doc/web/object/datetime/readme.md)

#### 数据显示

[tree(树形结构) 组件](./doc/web/object/tree/readme.md)

[table(表格) 组件](./doc/web/object/table/readme.md)

[paging 评分](./doc/web/object/paging/readme.md)

[card 评分](./doc/web/object/card/readme.md)

[tags 标签组](./doc/web/object/tags/readme.md)

#### 导航

[menu(NavMenu) 菜单组件](./doc/web/object/navmenu/readme.md)

[dropdown 下拉菜单按钮](./doc/web/object/dropdown/readme.md)

[tabs和Tab 页签](./doc/web/object/tabs/readme.md)

[link 评分](./doc/web/object/link/readme.md)

### WEB测试小技巧

[封装login，让自动化脚本可以连续执行](./doc/web/skill/login/readme.md)

[selenide和selenium的对象查找方法](./doc/web/object/locator/readme.md)

[selenide和selenium的关系和定位](./doc/web/skill/selenide/readme.md)

[如何提高脚本的执行稳定性](./doc/web/skill/wait/readme.md)

[提高selenium的鼠标操作速度](./doc/web/skill/click/readme.md)

[js中判断对象是否隐藏](./doc/web/skill/hidden/readme.md)

[WebElement.clear无效的问题](./doc/web/skill/clear/readme.md)

[innerText取不到内容](./doc/web/skill/innertext/readme.md)





