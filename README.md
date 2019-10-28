# common-table-plus

基于Vue和Element-ui封装的通用Table列表组件，支持远程数据、搜索、导出、分页、表头筛选功能。
搜索功能：
    内置了基本的输入框、下拉框和时间选择框类型，支持传入自定义组件。
导出功能：
    支持自定义配置导出选项
表头筛选
    支持自定义筛选项，支持远程方法获取筛选数据


配置选项说明

|配置项|说明|详细|
|----|----|----|
|tableOptions|表格配置项|
|columns|列配置项|Object|
|formOptions|搜索条件Form表单配置项||
|exportOptions|导出配置项||


 


columns配置项详解 
为了防止tableOptions过于复杂，故将表格的列配置单独拿出,columns为数组
|字段名|说明|类型|可选值|默认值| 
|----|----|----|----|----|
|type|类型|string|selection/index/expand|-|
|prop|对应列内容的字段名|String|-|-|
|label|标签文本|String|-|-|
|width|宽度|Number|-|-|






formOptions配置项详解
|字段名|说明|类型|可选值|默认值|
|----|----|----|----|----|
|visible|是否显示搜索条件|boolean|true 或 false|true|
|formType|表单类型,分为简单和折叠两种类型|String|simple 简单 或 collapse:折叠|simple|
|inline|行内表单模式|boolean|true 或 false|false|
|showSearchBtn|是否显示搜索按钮|boolean|true 或 false|true|
|searchBtnText|搜索按钮文字|String|-|查询|
|showResetBtn|是否显示搜索按钮|boolean|true 或 false|true|
|ResetBtnText|重置按钮文字|String|-|重置|
|items|表单域|Array|-|[]|







 


## Project setup

npm install






### Lints and fixes files
```
npm run lint
```


