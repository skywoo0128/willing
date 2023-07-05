# 对象和关键字封装

自从V8引擎发布以后，js的执行效率至少提高了几十倍，原先前端不敢做、不能做的工作现在都可以在浏览器中实现。尤其像VS Code这种IDE也能用js实现，并且运行的非常流畅。

于是前端也出现了各种各样的架构，包括bootstrap、Extjs、angular、react、vue等框架、Antd、AntdV、Element UI等各种组件库。早前的WEB应用大部分都是用标准HTML组件开发，所以对象操作和对象查找都比较统一。新的架构和组件库很少用标准H5组件，或者是因为标准H5组件不支持，比如transfer、cascader等组件；或者是因为标准H5组件不够美观。反正新的WEB框架，无论是对象定位，还是对象操作都比以前复杂的多。

能不能把各种杂七杂八的组件也按标准封装呢。使得无论用哪种架构，或者哪种组件库，都能提供一个统一的对象定位方法，和对象操作方法。基于这种想法，matrix尝试了一种新的对象和关键字封装方法，符合以下这些基本需求。
- 接口稳定统一，无论使用哪种组件库、同一种类型的组件，需要提供一致的操作接口和定位函数。不能因为不同的组件库，让脚本开发人员重新学习一套操作接口。
- 符合业务思维，每个功能都由单一的接口实现，每个接口都要实现一个完整的功能。
- 接口简单，提供尽可能少的接口（关键字）满足所有自动化的需求。同一种组件的同一种操作只应该有一个接口。
- 完备性，能够实现组件的所有操作要求。
- 稳定性，接口能够稳定执行。

# 对象和关键字封装

关键字六层封装，组件采用四层封装
![关键字封装结构](https://raw.gitmirror.com/skywoo0128/willing/main/doc/web/object/overview/obj-stuc.png "关键字封装结构")

1. 关键字：提供一套完备的自动化关键字，包括组件（对象）操作关键字、selenide封装的WebElement操作关键字、弹窗和iframe等窗口操作关键字、截图和输入文件等系统UI关键字、SQL操作关键字、以及数据处理等非UI的关键字。包括180个WEB关键字和80个非UI关键字，常用关键字不超过10个。
2. 抽象层：所有组件的根对象（WebNode）、以及所有脚本中可以使用的封装函数。
3. 基本对象：在WebNode的基础上，封装了4种对象。WebTable处理各种表格，包括折叠表格；WebTree处理各种树状结构，包括左侧的功能功能树，TreeSelect弹出的下拉框等；WebObject处理各种有状态的对象，比如input、tags等；WebButton只处理按钮类组件，比如button、img等。还封装了一个处理各种js弹窗的WebHint对象，包括alert、popup message等。
4. 标准组件库：在四个对象的基础上，根据组件的外观和行为，封装了50多个标准UI对象，基本上包含各种UI组件库的可操作组件。
5. 组件库封装：在标准组件库的基础上，根据各种UI组件库进行差异化封装，目前主要支持antd和element ui组件库，以及其他一些零散的组件，比如code mirror，vue tree select，Jexcel等。
6. 项目封装：在项目中主要封装login和logout接口。login接口需要判断是否已登录、登录用户名和当前用户是否一致、自动logout并切换用户、以及最主要的是自动复原到网页的初始状态，以便下一个脚本能够正确执行，而不需要关闭浏览器并重新登录。

## 关键字封装
关键字只对 WebNode, WebTable, WebTree, WebObject, WebButton和WebHint六个对象进行封装。因此无论在组件库中封装了哪种组件，都不会影响关键字。WEB的主要关键字在工具内部已封装，开箱即用。

## 标准组件库（完善中）

| 组件     | 组件     | 组件     | 组件     | 组件     |
| -------- | -------- | -------- | -------- | -------- |
| InputGroup | InputAffix | Select | Selects |
| DatePicker | MonthPicker | WeekPicker | DateTimePicker | TimePicker |
| DateRange | MonthRange | WeekRange | DateTimeRange | TimeRange |
| CheckBox | CheckGroup | RadioBox | RadioGroup | Transfer |
| TreeSelect | TreeSelects | Cascader | Cascaders | Tags |
| Upload | Slider | Switch | Rate | Tag |
| Card | Tabs | Tab | TagsView | TagView |
| Paging | Link | DropDown | PopupMenu | NavMenu |
| Tree（基类） | DataTree | IndentTree | NavTree |
| Table |
| TreeSelectMenu | ListSelectMenu |


## Element UI2 组件库
| 组件     | 组件     | 组件     | 组件     | 组件     |
| -------- | -------- | -------- | -------- | -------- |
| EuiInputGroup | EuiInputAffix | EuiSelect | EuiSelects |
| EuiDatePicker | EuiMonthPicker |  | EuiDateTimePicker | EuiTimePicker |
| EuiDateRange | EuiMonthRange | | EuiDateTimeRange | EuiTimeRange |
| EuiCheckBox | EuiCheckGroup | EuiCheckButton | EuiCheckButtonGroup |  |
| EuiRadioBox | EuiRadioGroup | EuiRadioButton | EuiRadioButtonGroup | EuiTransfer |
|  |  | EuiCascader | EuiCascaders | EuiTags |
| EuiUpload | EuiSlider | EuiSwitch | EuiRate |  |
| EuiCard | EuiTabs | EuiTab | EuiTagsView | EuiTagView |
| EuiPaging | EuiLink | EuiDropDown |  | EuiNavMenu |
|  | EuiDataTree | | EuiNavTree |
| EuiTable |


## Antd5 组件库
| 组件     | 组件     | 组件     | 组件     | 组件     |
| -------- | -------- | -------- | -------- | -------- |
| AntdInputGroup | AntdInputAffix | AntdSelect | AntdSelects |
| AntdDatePicker | AntdMonthPicker | AntdWeekPicker | AntdDateTimePicker | AntdTimePicker |
| AntdDateRange | AntdMonthRange | AntdWeekRange | AntdDateTimeRange | AntdTimeRange |
| AntdCheckBox | AntdCheckGroup | | |  |
| AntdRadioBox | AntdRadioGroup | AntdRadioButton | AntdRadioButtonGroup | AntdTransfer |
| AntdTreeSelect | AntdTreeSelects | AntdCascader | AntdCascaders | AntdTags |
| AntdUpload | AntdSlider | AntdSwitch | AntdRate |  |
| AntdCard | AntdTabs | AntdTab |  |  |
| AntdPaging | AntdLink | AntdDropDownLink | AntdDropDownButton | AntdNavMenu |
|  |  | AntdTree | AntdNavTree |
| AntdTable | AntdFixedTable |
| AntdTreeSelectMenu | | AntdTransList |


## Antdv3 组件库
| 组件     | 组件     | 组件     | 组件     | 组件     |
| -------- | -------- | -------- | -------- | -------- |
| AntdvInputGroup | AntdvInputAffix | AntdvSelect | AntdvSelects |
| AntdvDatePicker | AntdvMonthPicker | AntdvWeekPicker | AntdvDateTimePicker | AntdvTimePicker |
| AntdvDateRange | AntdvMonthRange | AntdvWeekRange | AntdvDateTimeRange | AntdvTimeRange |
| AntdvCheckBox | AntdvCheckGroup | | |  |
| AntdvRadioBox | AntdvRadioGroup | AntdvRadioButton | AntdvRadioButtonGroup | AntdvTransfer |
| AntdvTreeSelect | AntdvTreeSelects | AntdvCascader | AntdvCascaders | AntdvTags |
| AntdvUpload | AntdvSlider | AntdvSwitch | AntdvRate |  |
| AntdvCard | AntdvTabs | AntdvTab |  |  |
| AntdvPaging | AntdvLink | AntdvDropDownLink | AntdvDropDownButton | AntdvNavMenu |
|  |  | AntdvTree | AntdvNavTree |
| AntdvTable | AntdvFixedTable |
| AntdvTreeSelectMenu | | AntdvTransList |


## 其他一些组件封装
**code mirror**
CodeMirror 代码编辑器

**vue tree select**
VueTreeSelect
VueTreeSelects
VueTreeSelectMenu

**jexcel**
JexcelTable（支持各种输入，选择图片、选择日期、下拉框输入等）
JexcelMenu

**项目中的部分封装**
基于 PopupMenu 的 各种弹出菜单
基于 ListSelectMenu 的 各种popup window选择值


# 常用关键字
| 关键字     | 说明     | 
| -------- | -------- | 
|`WebSetValue==对象名称==对象值==`|一个关键字搞定所有对象的输入，支持复杂对象中的子对象操作|
|`WebSetValue==对象名称==参数名称==对象值==`|带指令名称的赋值指令，比如对Tabs组件的操作，参数名称可以是：增加、修改、删除|
|`WebGetValue==变量名称==对象名称==`|从对象中取值|
| `WebClick==对象名称==` | 点击按钮等 |
| `WebTreeClick==对象名称==节点标题==` | 点击树节点，包括功能树 |
| `WebGetCellValue==变量名称==对象名称==行编号==值标题==` | 取单元格的值，支持行查询，比如【行编号】=【工号(00001)】，会查询工号列中内容是00001的行 |
| `WebSetCellValue==对象名称==行编号==值标题==输入框类型==输入值==` |修改单元格的值，需要输入单元格的组件类型，比如AntdSelect等|

只需要极少数的关键字就可以满足自动化测试中绝大部分的需求。下面是一个自动化脚本的样例。


## 一个普通的脚本
```
WebOpenBrowse==https://preview.pro.ant.design/==

WebDefPage==高级表单==AntPro==高级表单==
WebTreeClick==高级表单.左侧菜单==%高级表单.左侧菜单%==

#仓库管理
WebSetValue==高级表单.仓库名==%高级表单.仓库名%==
WebSetValue==高级表单.仓库域名==%高级表单.仓库域名%==
WebSetValue==高级表单.仓库管理员==%高级表单.仓库管理员%==
WebSetValue==高级表单.审批人==%高级表单.审批人%==
WebSetValue==高级表单.生效日期==%高级表单.生效日期%==
WebSetValue==高级表单.仓库类型==%高级表单.仓库类型%==

#任务管理
WebSetValue==高级表单.任务名==%高级表单.任务名%==
WebSetValue==高级表单.任务描述==%高级表单.任务描述%==
WebSetValue==高级表单.执行人==%高级表单.执行人%==
WebSetValue==高级表单.责任人==%高级表单.责任人%==
WebSetValue==高级表单.任务生效日期==%高级表单.任务生效日期%==
WebSetValue==高级表单.任务类型==%高级表单.任务类型%==

#修改成员
WebTableButtonClick==高级表单.成员管理==工号(00002)==操作==编辑==
WebSetCellValue==高级表单.成员管理==工号(00002)==成员姓名==input==%高级表单.修改成员姓名%==
WebSetCellValue==高级表单.成员管理==2==工号==input==%高级表单.修改工号%==
WebSetCellValue==高级表单.成员管理==2==所属部门==input==%高级表单.修改所属部门%==
WebTableButtonClick==高级表单.成员管理==2==操作==保存==

#增加成员
WebClick==高级表单.添加一行数据==
WebSetCellValue==高级表单.成员管理==4==成员姓名==input==%高级表单.增加成员姓名%==
WebSetCellValue==高级表单.成员管理==4==工号==input==%高级表单.增加工号%==
WebSetCellValue==高级表单.成员管理==4==所属部门==input==%高级表单.增加所属部门%==
WebTableButtonClick==高级表单.成员管理==4==操作==保存==

WebClick==高级表单.提交==
```

这个脚本能操作【[antd pro 的高级表单](https://preview.pro.ant.design/form/advanced-form)】，页面中包含了各种组件，但是脚本中只用到了：WebTreeClick，WebSetValue，WebTableButtonClick，WebSetCellValue，WebClick等几个关键字。

