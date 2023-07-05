## 公共对象封装

公共对象
| 名称 | 说明 |
| --- | --- |
| WebNode | UI对象的公共方法，比如：waitLocated、exist等  |
| WebObject | 主要是有状态的输入项，比如：select、input等 |
| WebButton | 简单的鼠标点击组件，比如：button、anchor等。 遗留的对象，可以并入WebObject |
| WebTable | 表格组件 |
| WebTree | 树状组件 |


### WebNode

| 方法 | 输入参数 | 返回参数 | 说明 |
| --- | --- | --- | --- |
| exist |  | boolean | 判断对象是否存在 |
| waitLocated | tmOut | void | 等待对象加载完成 |
| waitPageLocated | tmOut | void | 等待页面加载完成 |
| retryOrObjectLocated | millis | void | 如果对象不存在，重试前一个指令 |
| sendKeys | text | void | 模拟键盘输入字符串 |
| pressKey | 组合键 | void | 模拟键盘输入组合键(比如 ALT+F1) |
| pressEnter | | void | 输入ENTER |
| pressEscape | | void | 输入ESC |
| pressTab | | void | 输入TAB |
| dragAndDropTo | target | void | 拖拽到目标对象 |
| hover | | void | 鼠标悬停 |
| text | | String | 取对象的text |
| attr | name | String | 取对象的属性 |
| cssValue | name | String | 取对象的css属性 |
| innerHtml | | String | 取对象的innerHtml |
| innerText | | String | 取对象的innerText |
| outerHtml | | String | 取对象的outerHtml |
| isVisible | | boolean | 判断对象是否可见 |
| checkCssValue | propertyName; expectedValue; exception | boolean | 检查CSS值是否为预期值 |
| checkCssClass | expectedCssClass; exception | boolean | 检查class是否包含预期值 |
| checkVisible | flag; exception | boolean | 检查对象预期可见 |
| checkAttribute | attributeName; expectedAttributeValue; exception | boolean | 检查属性值是否为预期值 |


### WebObject

| 方法 | 输入参数 | 返回参数 | 说明 |
| --- | --- | --- | --- |
| setValue | name; paraValue | void | 对象赋值 |
| setValue | paraValue | void | 对象赋值 |
| setValue_ifEnable | paraValue | void | 如果可用给对象赋值 |
| getValue | name | String | 对象取值 |
| getValue | | String | 对象取值 |
| retryOrWaitValue | exceptValue; millis | void | 如果不是预期值，重试前一个指令 |
| waitValue | name | String | 等待对象被赋值 |
| waitValue | | String | 等待对象被赋值 |
| getOptions | | String[] | 取对象的选择项 |
| isReadOnly | | boolean | 判断对象是否只读 |
| isEnabled | | boolean | 判断对象是否可用 |
| isSelected | | boolean | radio, checkbox是否被选中 |
| setSelected | flag | void | 选中单选或复选框 |
| dblClick | | void | 双击按钮 |
| click | title | void | 点击子对象 |
| click | | void | 点击对象 |
| click | x; y | void | 点击对象2 |
| rClick | | void | 右键点击按钮 |
| focus | | void | 设置焦点 |
| blur | keyCode | void | 导航到下一个对象 |
| checkOptions | optValues; exception | boolean | 检查对象的选择项是否为预期值 |
| checkOption | optValue; exception | boolean | 检查对象的选择项是否包含预期值 |
| checkValue | 比较方式; exceptValue; exception | boolean | 检查对象值是否为预期值 |
| checkValue | exceptValue; exception | boolean | 检查对象值是否为预期值 |
| checkEnable | flag; exception | boolean | 检查状态是否为预期值 |



### WebButton

| 方法 | 输入参数 | 返回参数 | 说明 |
| --- | --- | --- | --- |
| click | | void | 点击按钮 |
| click | x; y | void | 点击按钮2 |
| rClick | | void | 右键点击按钮 |
| dblClick | | void | 双击按钮 |
| click_ifEnable | | void | 按钮可用时点击按钮 |
| isEnabled | | boolean | 判断按钮是否可用 |
| setValue | value | void | 修改按钮标题 |
| getValue | | String | 取按钮标题 |
| focus | | void | 设置焦点 |
| blur | keyCode | void | 导航到下一个对象 |
| checkValue | exceptValue; exception | boolean | 检查标题是否为预期值 |
| checkEnable | flag; exception | boolean | 检查状态是否为预期值 |



### WebTable

| 方法 | 输入参数 | 返回参数 | 说明 |
| --- | --- | --- | --- |
| getCellValue | row; title | String | 取单元格的值 |
| getCellValue | row; col | String | 取单元格的值 |
| getCellValue | colName; colValue; title | String | 取单元格的值 |
| setCellValue | row; title; tagName; value | void | 设置单元格的值 |
| setCellValue | colName; colValue; title; tagName; value | void | 设置单元格的值 |
| cellClick | colName; colValue; title | void | 点击单元格 |
| cellClick | row; col | void | 点击单元格 |
| cellClick | row; title | void | 点击单元格 |
| cellContextClick | row; col | void | 右键点击单元格 |
| cellContextClick | row; title | void | 右键点击单元格 |
| cellContextClick | colName; colValue; title | void | 右键点击单元格 |
| cellDblClick | row; col | void | 双击单元格 |
| cellDblClick | row; title | void | 双击单元格 |
| cellDblClick | colName; colValue; title | void | 双击单元格 |
| cellBtnClick | row; title; btnName | void | 点击单元格中的按钮 |
| cellBtnClick | colName; colValue; title; btnName | void | 点击单元格中的按钮 |
| btnClick | colName; colValue; btnName | void | 点击行中的按钮 |
| btnClick | row; btnName | void | 点击行中的按钮 |
| getColValues | col | String[] | 取表格列的值 |
| getColValues | colName | String[] | 取表格列的值 |
| getRowValues | row | String[] | 取表格行的值 |
| getRowValues | colName; colValue | String[] | 取表格行的值 |
| getSelectedRows | checkTitle; valueTitle | String | 取所有选中行 |
| rowClick | colName; colValue | void | 点击行 |
| rowClick | row | void | 点击行 |
| rowContextClick | row | void | 右键点击行 |
| rowContextClick | colName; colValue | void | 右键点击行 |
| rowDblClick | colName; colValue | void | 点击行 |
| rowDblClick | row | void | 点击行 |
| isRowSelected | colName; colValue | boolean | 行是否选中 |
| isRowSelected | colName; colValue; title | boolean | 行是否选中 |
| setRowSelected | colName; colValue; flag | void | 选中行 |
| setRowSelected | colName; colValue; title; flag | void | 选中行 |
| rowExist | colName; colValue | boolean | 检查记录是否存在 |
| getRecordSet | | cn.lz.test.func.RecordSet | 取表格记录集 |
| getTitles | | String[] | 取标题行的值 |
| getPath | row | String | 取单元格的值 |
| getPath | colName; colValue | String | 取行的全路径 |
| isEnabled | | boolean | 判断对象是否可用 |
| checkTitles | optValues; exception | boolean | 检查标题行是否为预期值 |
| checkTitle | optValue; exception | boolean | 检查标题行是否包含预期值 |




### WebTree

| 方法 | 输入参数 | 返回参数 | 说明 |
| --- | --- | --- | --- |
| click | nodeText | void | 点击节点 |
| rClick | nodeText | void | 右键点击节点 |
| dblClick | nodeText | void | 双击节点 |
| getValue | | String | 取选中节点的标题 |
| setChecked | nodes; flag | void | 选择checkBox |
| isChecked | nodeText | boolean | 子节点的checkBox是否选中 |
| allChecked | flag | void | 第一层节点全选或全不选 |
| allChecked | nodeText; flag | void | 子节点全选或全不选 |
| getNodes | nodeText | String[] | 取所有子节点 |
| getNodes | | String[] | 取第一层节点 |
| isEnabled | | boolean | 判断树是否可用 |
| checkValue | exceptValue; exception | boolean | 检查选中节点是否为预期值 |
| checkSelected | nodeText; exception | boolean | 检查CheckBox是否选中 |
| checkOptions | nodeText; opts; exception | boolean | 检查子节点是否为预期值 |
| checkOption | nodeText; opt; exception | boolean | 检查子节点是否包含预期值 |
| checkEnable | flag; exception | boolean | 检查状态是否为预期值 |
| checkOptions | opts; exception | boolean | 检查第一层子对象是否为预期值 |
| checkOption | opt; exception | boolean | 检查第一层子对象是否包含预期值 |



