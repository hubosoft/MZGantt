
# MZGantt甘特图插件
MZGantt是一款原生js开发的甘特图插件。支持vue，ts，js等，支持流行的各种前端框架，可以快速引用到我们的web程序或者移动应用中。

## 一. 插件特征

* <b>拖拽编辑：</b>支持拖拽编辑
* <b>行内编辑：</b>支持行内编辑功能
* <b>弹框支持：</b>提供任务model，与外部弹框配合，完成任务编辑。

* <b>列自定义：</b>支持自定义任务属性列
* <b>多段展示：</b>支持任务行内多段展示
* <b>支持日历：</b>支持自定义日历
* <b>图片导出：</b>支持甘特图完整图片导出

* <b>大数据处理：</b>支持懒加载方式加载任务
* <b>过滤器支持：</b>支持设定条件，快速筛选
* <b>里程碑标记：</b>支持配置显示里程碑标记线条
* <b>四种任务关系：</b>支持拖拽建立和编辑四种任务关系(FS,FF,SF,SS)
* <b>消息框自定义：</b>支持消息框内容和样式自定义

* <b>样式可调：</b>支持显示样式自定义
* <b>使用简单：</b>js直接引用或者通过npm安装使用（支持vue，ts）
* <b>参数配置：</b>提供丰富配置参数和接口满足项目需求

## 二. 演示

演示地址：<https://mzgantt.com/demo>

## 三. 如何使用?

MZGantt支持js引用和npm安装两种使用方式。

1. 下载
    使用前，先下载普通js版插件或使用npm进行安装。

2. 引用MZGantt插件
    在html页面头部中引入如下js包：
    * 普通js引用:
    ```javascript
    <!-- 引入jquery库 -->
    <script language="javascript" src="jquery.min.js"></script>

    <!-- 引入甘特图插件 -->
    <link rel="stylesheet" type="text/css" href="./gantt/mzgantt.css" />
    <script language="javascript" src="./gantt/mzgantt.js"></script>
    <script language="javascript" src="./gantt/edit.js"></script>
    ```

    * npm引用:
    ```javascript
    // 导入MZGantt包，具体有如下几个包，按具体需求引入
    //      1. 基础包：MZGantt（必须引入。包含甘特图基本展示等功能）
    //      2. 编辑功能包：MZGanttEditor（按需引入。包含甘特图编辑功能）
    //      3. 导出功能包：MZGanttExport（按需引入。包含甘特图导出图片功能）
    //      4. 移动端支持包：MZGanttMobile（按需引入。包含移动端支持功能）
    import {MZGantt, MZGanttEditor} from '/node_modules/mzgantt';

    // 获取并设置授权key
    MZGantt.LicenseKey = "您的授权key";
    ```

3. 定义甘特图容器(div)
    在html页面内定义一个div，作为展示甘特图的容器：
    ```html
    <div id="GanttChartDIV" style="width:100%;"></div>
    ```

4. 定义甘特图参数（可定义空对象{}，插件将使用使用默认值）
    此步骤可选。在需要个性化显示甘特图时，可以通过设置甘特图参数来实现。部分可设置参数如下：
    ```javascript
    var ganttConfig = {
        useFor: "Task",                         // 设置甘特图用途，资源利用（比如会议室管理）：'Resource', 任务管理（比如计划和进度管理）：'Task'。默认:Task
        ganttStatus: "e",                       // 设置甘特图状态（ e:编辑计划, r：只读）

        // 表格列配置（甘特图左侧表格部分显示列配置，非甘特图数据格式）
        columnDefs: [
            {name: 'seq',         field: "seq",        type:"level"},
            {name: 'checkbox',    field: "checkbox"},
            {name: 'iconColsVal', field: "iconColsVal", width: 30},

            // 以下字段，name可自定义，且须与数据中相应项目名称对应。field值为内置固定值，不可重新设置，且不支持设置type。
            {name: 'name',        field: "name",       title: "任务名称", width: 150},
            {name: 'resId',       field: "resId",      title: "责任人",   width: 50,    options: resourceList},
            {name: 'dur',         field: "dur",        title: "工期",     width: 60},
            {name: 'planStart',   field: "planStart",  title: "计划开始", width: 120,   editable: false},
            {name: 'planEnd',     field: "planEnd",    title: "计划完成", width: 120,   colCallBack:custCallbackFunc1},
            {name: 'planDur',     field: "planDur",    title: "计划人天", width: 60},
            {name: 'rsltStart',   field: "rsltStart",  title: "实际开始", width: 120},
            {name: 'rsltEnd',     field: "rsltEnd",    title: "实际完成", width: 120},
            {name: 'rsltDur',     field: "rsltDur",    title: "实际人天", width: 60},
            {name: 'pctComp',     field: "pctComp",    title: "完成度",   width: 60},
            // {name: 'caption',     field: "caption",    title: "标签文字"},

            // 自定义列（field: "custCol"）：支持设置type：DropDownList/TextBox
            // 属性altText:自定义列替换显示文字
            {name:"yourColName1", field: "custCol", title:"列名1", type: "TextBox", default: 10, width:50,align:"left"},
            {name:"yourColName2", field: "custCol", title:"列名2", type: "DropDownList", options:dropDownListData, width:100, align:"left"},
            {name:"yourColName3", field: "custCol", title:"列名3","width":150,"align":"left", colCallBack:custCallbackFunc2, altText: "查看图文"}
        ],

        // 甘特图区域显示控制
        showTrack: 1,                           // 0:计划甘特图；1:实绩甘特图 。默认：计划甘特图。
        showDuplicate: 0,                       // 是否显示 资源重叠分配标志
        showMileStone: 1,                       // 是否显示 里程碑：不显示
        showDependencies: 1,                    // 是否显示 连接线(0/1),默认：0不显示。
        showDayDate: "date",                    // 设置day格式时，显示星期或者日期（day:星期；date:日期）。默认:day显示星期。
        showWeekIndex: "",                      // 设置day格式时，右侧首行显示第几周或者显示周第一个日期。Y：显示周数，其他，显示日期。默认显示日期。

        // 数据定义类===============================================================================
	    weekWorkSetting:                        // 周工作日设置（D1:周1....D0:周天。1: 工作日，0：休息日)
		    {D1:1, D2:1, D3:1, D4:1, D5:1, D6:0, D0:0},
        holidayList: [                          // 设置节假日列表。在正常显示周末的情况下，添加列表中的日期作为节假日，灰色显示。
            '2023-02-09',
            '2023-02-21',
            '2023-03-20'],
        workdayList: ['2023-03-18'],            // 设置出勤日列表
        mileStoneLines: [                       // 里程碑配置
            {date:'2023-11-05',name:'startMS', title:'项目设计讨论', color:'#FF00BF', thickness: "2px"},
            {date:'2023-11-25',name:'pjMeeting', title:'代码review', color:'#088A29', thickness: "2px"},
            {date:'today',name:'today', title:'今日', color:'#FF0000', thickness: "2px"}
        ],

        // 插件配置类===============================================================================
        captionType: 'Caption',                 // 设置甘特图标题(None,Name, Caption,Resource,Duration,Complete,Reserver,Title,Theme)  None:不显示标题
        captionPosition: 'Right',               // 设置甘特图标题显示位置（Over:甘特图条上方；Right:甘特图条右侧，默认Over)

        // 显示日期格式和输入日期格式应该统一
        dateFormat: 'yyyy-mm-dd',               // 设置参数日期格式 ('mm/dd/yyyy', 'dd/mm/yyyy', 'yyyy-mm-dd','yyyy/mm/dd')
        weekStartDay: 0,                        // 设置星期第一日（1：星期一开始；0：星期天开始），默认：0

        workingHourRange: ['1','23'],           // 设置hour格式下的每天的工作时段。(以小时为单位)。默认: 1,23。
        hourFormat: 0,                          // 设置hour格式（0:小时后不加:00；1:小时后加:00）。默认:0显示星期.

        dragChgDur: "1",                        // 设置拖拽变更工日,如果showChgDur设置为不显示，则该选项的设置无效。
        autoCalDur: 1,                          // 设置自动计算工日（默认不自动计算）

        includeHoliday: "N",                    // 工日计算是否包含假期和周末：Y: 包含；N:不包含
        includeToTime: "Y",                     // 甘特图完成时间是否包含最后一个时间点（开始和完成时间为日期类型时设置为包含，小时的话设置为不包含），默认Y

        durPrecision: 2,                        // 设置工期和工日精度（默认小数后一位）
        fixColsCnt: 3,                          // 设置锁定列数, 默认:0(不锁定列)
	    fixParent: 1,                           // 固定父任务范围（1：不随子任务拖动而变动；0：随子任务拖动而变动）
	    idType: "Snow",                         // 任务id计算方法（默认：12位任意字符；UUID: uuid; Snow: 雪花id）

        // 样式控制类===============================================================================
        // 背景色设置类
        // planBackground: vPlanBackground,     // 设置甘特图计划条背景
        // resultBackground: vResultBackground, // 设置甘特图实绩条背景
        compBackground: '#0000FF',              // 设置完成度背景
        // groupBackground: '#00FF00',          // 设置父任务行背景
        holidayBGColor: '#E6E6E6',              // 设置休息日/假期背景
        currentTimeBackColor: ['h','#FF9797'],  // 设置当前时间背景(参数1：h: 通过设置表头样式显示当前时间; c: 通过设置内容单元格样式显示当前时间。参数2：颜色值)
        intervalColor: ['#FFFFFF','#F4F4F4'],   // 设置隔行背景颜色, 默认: #FFFFFF,#F4F4F4
        selectedRowColor: '#CAE1FF',            // 设置选中行背景颜色, 默认: #CAE1FF
        selectedCellColor: '#2E9AFE',			// 设置选中单元格边框颜色，默认：2E9AFE
        borderColor: '#9E9E9E',                 // 设置甘特图边框颜色, 默认: #9E9E9E

        // 甘特图条
        barBorderRadius: 6,                     // 设置甘特图条圆角半径, 默认: 3px
        barBorderWeight: 1,                     // 设置甘特图条边框粗细, 默认: 1px
        barBorderColor: '#9E0000',              // 设置甘特图条边框颜色, 默认: #9E9E9E
        barOpacity: 1,                          // 设置甘特图条透明度，默认：0.7
        barGridBorderWeight: 1,                 // 设置甘特图条背景边框粗细, 默认: 1px
        resultBarStyle: 'mark',                 // 实绩条样式（normal:不标记计划外日期，mark: 标记计划外日期; cust: 自定义分段）
        markResultBarColor:                     // 实绩条样式为mark类型时，实绩条背景颜色
		{beforePlanBGColor: '#00FF00',
		 exceedPlanDBColor: '#cc0000'},
        barHeightAdj: 0,						// 进度条高度调整量（默认0px，不做任何调整；值越大，进度条越高。可以为负值）
	    barDistanceAdj: 0,						// 计划进度条和实际进度条之间距离调整量（默认0px，不做任何调整；值越大，距离越近。可以为负值。）

        // 拖动杆
        dragHandlerBackColor: '#D8D8D8',		// 拖动杆背景色

        // lineExpandLen: 0,                    // 依赖线伸出量参数
        dependLineColor: '#228B22',             // 连接线颜色
        dependLineMouseOverColor: '#FE9A2E',    // 连接线鼠标浮动颜色
        criticalPathBGColor: '#FE9A2E',         // 关键路径标记颜色

        // 宽高设置
        leftSideWidth: 300,                     // 设置左侧框宽度, 默认:300px
        contentHeight: 360,                     // 设置甘特图高度（不含表头）, 默认:300px.如果在页面调用adjustGanttHeight函数使甘特图高度自适应，则次参数可不设置。
        rowHeight: 35,                          // 设置行高度, 默认:35px

        // 左侧列宽度设置，在完善columnDefs后，这部分就可以废弃了。
        iconColWidth: 30,                       // 设置图标列宽度，默认:60px
        nameColWidth: 150,                      // 设置任务/资源列宽度，默认:150px
        dateColWidth: 120,                      // 设置日期列宽度，默认:80px

        // 右侧格子宽度设置（不同时间刻度时）
        hourColWidth: 20,                       // 小时刻度时
        dayColWidth: 20,                        // 天刻度时
        monthColWidth: 70,                      // 月刻度时
        quarterColWidth: 100                    // 季度刻度时
    };

    // 甘特图参数配置
    myGantt.config(ganttConfig);
    ```

5. 后台获取数据，或定义数据

    ```javascript
    /*
      甘特图数据对象项目说明：
      一：内置固定数据项，不可自定义其他名称。
        1. id                  //（必须）String        节点编号（任意字符串或数字，不可重复）
        2. plan                //（可选）Array         计划数据
        3. isExpand            //（可选）Number(0或1)  是否展开显示 1:展开/0:收缩
        4. isGroup             //（可选）Number(0或1)  是否设置为父（组）1：组/0：叶子节点
        5. preNodes            //（可选）String|Array  前置节点，多个前置节点时，使用逗号分开
        6. parentId            //（可选）String        父节点编号(当前行是最顶层节点时，该值须设置为空"")
        7. isMS                //（可选）Number(0或1)  是否里程碑
        8. caption             //（可选）String        标题
        9. planBarColor        //（可选）String        甘特图计划背景（颜色值)

      二：以下数据项，可使用自定义名称，须与列定义中name匹配。
        10. name               //（必须）String        名称（可以作为任务名称，资源名称等）
        11. iconColsVal        //（可选）Array         图标列(可以是用逗号隔开的多个图标）
        12. rsltStart          //（可选）String Date   实际开始
        13. rsltEnd            //（可选）String Date   实际完成
        14. rsltDur            //（可选）Number        实际人天
        15. resId              //（可选）String        资源/负责人
        16. pctComp            //（可选）Number        完成度（0 ~ 100 百分比)
        17. seq                //（可选）Decimal       排序号
        18. yourColName        //（可选）String        自定义列的值，key需要与自定义列名对应
    */

    // 甘特图数据
    let ganttData= [
        {
            "id": 1,
            "seq": 1000,
            "iconColsVal": [],
            "name": "单位A施工期间",
            "plan": [{}],
            "rsltStart": "",
            "rsltEnd": "",
            "rsltDur": 1,
            "planBarColor": "",
            "isMS": 0,
            "resId": "1",
            "pctComp": 0,
            "isGroup": 1,
            "parentId": "",
            "isExpand": 1,
            "preNodes": "",
            "caption": "重点关注",
            "yourColName1": "10",
            "yourColName2": "02",
            "yourColName3": "<img src='./images/stop.png'>",
        }, {
            "id": 11,
            "seq": 2000,
            "iconColsVal": [{
                    "src": "./images/stop.png",
                    "title": "停止"
                }
            ],
            "name": "任务名称1",
            "plan": [{
                    "start": "",
                    "end": "",
                    "dur": "2"
                }
            ],
            "rsltStart": "2023-07-02",
            "rsltEnd": "2023-07-06",
            "rsltDur": 1,
            "planBarColor": "#2EFE9A",
            "isMS": 0,
            "resId": "2",
            "pctComp": 50,
            "isGroup": 0,
            "parentId": 1,
            "isExpand": 1,
            "preNodes": "",
            "caption": "",
            "yourColName1": "1",
            "yourColName2": "02",
            "yourColName3": "",
            "custRsltBars": [
                {
                    name: "p1",
                    title: "分段1",
                    start: "2023-07-02",
                    end: "2023-07-04",
                    style: "background:#2E9AFE;color:#FFFFFF;text-align:left;"
                }, {
                    name: "p2",
                    title: "分段2",
                    start: "2023-07-06",
                    end: "2023-07-17",
                    html: "<span style='height:7px;border-radius:5px 5px;position:absolute;left:5px;top:-1px;background:red;font-size:9px;color:#FFFFFF'>美丽</span>",
                    style: "background:#FF0000;color:#FFFFFF;text-align:left;"
                },
            ],
        }, {
            "id": 12,
            "seq": 3000,
            "iconColsVal": [],
            "name": "任务名称2",
            "plan": [{
                    "start": "2023-07-06",
                    "end": "2023-07-10"
                }
            ],
            "rsltStart": "2023-07-07",
            "rsltEnd": "2023-07-10",
            "rsltDur": 1,
            "planBarColor": "#2EFE9A",
            "isMS": 0,
            "resId": "2",
            "pctComp": 10,
            "isGroup": 0,
            "parentId": 1,
            "isExpand": 1,
            "preNodes": "",
            "caption": "备注1",
            "yourColName1": "1",
            "yourColName2": "03",
            "yourColName3": "",
        }, {
            "id": 13,
            "seq": 4000,
            "iconColsVal": [{
                    "src": "./images/star.png",
                    "title": "星级"
                }
            ],
            "name": "任务名称3",
            "plan": [{
                    "start": "2023-07-11",
                    "end": "2023-07-15"
                }
            ],
            "rsltStart": "2023-07-11",
            "rsltEnd": "2023-07-14",
            "rsltDur": 1,
            "planBarColor": "#2EFE9A",
            "isMS": 0,
            "resId": "2",
            "pctComp": 20,
            "isGroup": 0,
            "parentId": 1,
            "isExpand": 1,
            "preNodes": 12,
            "caption": "",
            "yourColName1": "50",
            "yourColName2": "01",
            "yourColName3": "",
        }, {
            "id": 14,
            "seq": 5000,
            "iconColsVal": [],
            "name": "任务名称4",
            "plan": [{
                    "start": "",
                    "end": "2023-07-17"
                }
            ],
            "rsltStart": "2023-07-16",
            "rsltEnd": "2023-07-17",
            "rsltDur": 1,
            "planBarColor": vPlanBackground,
            "isMS": 0,
            "resId": "2",
            "pctComp": 0,
            "isGroup": 0,
            "parentId": 1,
            "isExpand": 1,
            "preNodes": "",
            "caption": "",
            "yourColName1": "20",
            "yourColName2": "01",
            "yourColName3": "",
        },
        {
            "id": 2,
            "seq": 6000,
            "iconColsVal": [],
            "name": "单位B施工期间",
            "plan": [{}
            ],
            "rsltStart": "",
            "rsltEnd": "",
            "rsltDur": 1,
            "planBarColor": "",
            "isMS": 0,
            "resId": "1",
            "pctComp": 0,
            "isGroup": 1,
            "parentId": "",
            "isExpand": 1,
            "preNodes": "",
            "caption": "",
            "yourColName1": "4",
            "yourColName2": "01",
            "yourColName3": "<img src='./images/stop.png'>",
        }, {
            "id": 21,
            "seq": 7000,
            "iconColsVal": [{
                    "src": "./images/stop.png",
                    "title": "停止"
                }
            ],
            "name": "任务名称1",
            "plan": [{
                    "start": "2023-07-19",
                    "end": "2023-07-29"
                }
            ],
            "rsltStart": "2023-07-26",
            "rsltEnd": "2023-07-29",
            "rsltDur": 1,
            "planBarColor": vPlanBackground,
            "isMS": 0,
            "resId": "2",
            "pctComp": 40,
            "isGroup": 0,
            "parentId": 2,
            "isExpand": 1,
            "preNodes": "",
            "caption": "",
            "yourColName1": "4",
            "yourColName2": "02",
            "yourColName3": "",
        },
        ......
    ];
    ```


6. 创建甘特图对象

    ```javascript
    // 实例化甘特图对象（在vue中需要执行此语句进行甘特图对象实例化）
    const myGantt = MZGantt.init();

    // 启动功能模块
    //   (1)使用插件npm安装包时，如果需要编辑、导出和移动端支持功能，引用相应功能包后，需要在此启动
    //   (2)使用插件普通js包时，以下启动（start）命令可以略去。
    //启动甘特图编辑
    MZGanttEditor.start(myGantt);
    // 启动导出功能
    MZGanttExport.start(myGantt);

    /*
    创建并展示甘特图
        参数1：(必须)容器div对象id
        参数2：(可选)时间刻度类型
        参数3：(可选)配置选项
        参数4：(可选)数据
    */
    myGantt.createGantt("GanttChartDIV", "day", ganttConfig, data);

    // 注意：此处如果为设置ganttConfig和data，可以独立使用配置方法和绑定数据等方法
    // 甘特图参数配置
    myGantt.config(ganttConfig);

    // 加载数据
    myGantt.bindGanttData(data);

    // 甘特图展示
    myGantt.drawGantt()
    ```

7. 监听事件
    MZGantt支持监听渲染或者点击等事件，可以使用这些事件实现一些特殊的控制效果。

    ```javascript
    // 单元格渲染
    myGantt.on("cellRender", function (row, cellObj) {
        // *******************************************************
        // 可对以下甘特图左侧表格以下类型（field)单元格进行渲染控制：
        // 一. 内置类型列：
        //    name                 名称
        //    iconColsVal          图标列
        //    resId                资源/负责人
        //    planStart            计划开始
        //    planEnd              计划完成
        //    planDur              计划人天
        //    rsltStart            实际开始
        //    rsltEnd              实际完成
        //    rsltDur              实际工日
        //    pctComp              完成度

        // 二.自定义列
        //    yourColName1             自定义列名（name)
        // *******************************************************

        // 获取单元格名称（渲染对象单元格）
        var field = cellObj.field;

        // 定义渲染样式对象
        var cellStyle = {};

        // 样式设置：任务名称
        if (field == "name") {
            if (row.plan[0].dur > 5) {
                cellStyle["cellHTML"] = '<span style="color:red;" >' + row.name + '</span>';
                // cell.style.color = "red";
            }
            if (row.plan[0].dur >= 2 && row.plan[0].dur <= 5) {
                cellStyle["cellHTML"] = '<span style="color:green;" >' + row.name + '</span>';
            }
            if (row.plan[0].dur < 2) {
                cellStyle["cellHTML"] = '<span style="color:black;" >' + row.name + '</span>';
            }
        }

        // 样式设置：负责人
        if (field == "resId") {
            if (row.name == "任务名称1") {
                cellStyle["ce11Style"] = 'color:green;font-weight:bold';
            }
        }

        // // 样式设置：自定义列(使用自定义列名称)
        // if (field == "yourColName2") {
        //   cellStyle["cellHTML"] = '<input id="tstBtn" type ="button" value="隐藏里程碑" onclick="test1">';
        // }

        return cellStyle;
    });

    // 进度条渲染
    myGantt.on("barRender", function (row) {
        // 定义渲染样式对象
        var barStyle = {};

        if (row.name === "任务名称2") {
            barStyle["planBarStyle"] = 'background:red;';
            // barStyle["planBarLeftHtml"] = '<img width="20px" height="20px" src="./images/work.svg" />';
            // barStyle["planBarOverHtml"] = '';
            barStyle["planBarRightHtml"] = '<div style="display:block;">努力最酷啊</div>';
        }

        // 返回对象
        return barStyle;
    });

    // 点击进度条
    myGantt.on("clickBar", function (row, eventXY, dom) {
        console.log("click: 任务：" + row.name + ", 点击日期:" + eventXY.date);
    });

    // 右键点击进度条
    myGantt.on("rightClick", function (row, eventXY, dom) {
        console.log("right click: 任务：" + row.name + ", 点击对象id:" + dom.id);
        console.log("点击日期:" + eventXY.date);
    });
    ```

8. 多种加载方式
    为了提升加载效率，插件支持多种加载方式。
    ###### 方式1：普通加载（一次性加载并渲染所有任务）
    ```javascript
    // 创建甘特图对象并配置
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);
    // 绑定数据
    myGantt.bindGanttData(data);

    // ※ 以上三个步骤可以简化为:
    myGantt.createGantt("GanttChartDIV", "day", ganttConfig, data);
    ```

    ###### 方式2：数据监听渲染加载
    ```javascript
    // 配置参数
    var ganttConfig = {
        loadType: "listenLoad"
    };

    // 创建甘特图对象并配置
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);

    // 定义甘特图监听器
    var dataListener = myGantt.listener;

    // 设置值甘特图值，自动驱动插件，更新显示。
    dataListener.rawGanttData = [
        {
            "id": 1,
            "seq": 1000,
            "iconColsVal": [],
            "name": "单位A施工期间",
            "rsltStart": "",
            "rsltEnd": "",
            "rsltDur": 1,
            "planBarColor": "#C0EBE8",
            "linkTo": "http://",
            "isMS": 0,
            "resId": "1",
            "pctComp": 0,
            "isGroup": 1,
            "parentId": "",
            "isExpand": 1,
            "preNodes": "",
            "caption": "重点关注",
            "plan": [{ "start": "2023-07-05", "end": "2023-07-06", "dur": 10 }]
        }
    ];

    // 不需要再执行bindGanttData和drawGantt方式
    ```

    ###### 方式3：分页渲染（一次性加载所有数据，分页渲染）
    ```javascript
    // 配置参数
    var ganttConfig = {
        loadType: "loadAllToPage",
        pageSize: 20         // 分页加载页面大小
    };

    // 创建甘特图对象并配置
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);

    // 绑定数据(数据按页逐页展示出来)
    myGantt.bindGanttData(data);
    ```

    ###### 方式4：分页加载渲染（每次加载并渲染一定行数，逐页加载并渲染）
    ```javascript
    // 分页加载处理
    //   参数pageNo为插件要请求页面的数据，从1开始
    function loadPageData(pageNode) {
        // 返回参数指定页的甘特图数据
        ...
    }

    // 配置参数
    var ganttConfig = {
        loadType: "loadByPage",
        pageSize: 20,               // 分页加载页面大小
        loadDataFunc: loadPageData  // 分页加载处理
    };

    // 创建甘特图对象并配置
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);

    // 甘特图渲染
    myGantt.drawGantt();
    ```

## 四. 甘特图数据行数据模型
下表格式为MZGantt甘特图数据行数据模型，在绑定数据到甘特图前，需要按照此格式作成数据。可以批量绑定，参考bindGanttDate方式；也可单行绑定，多用于与外部弹框结合。

<div style="margin-left:30px">

|<div style="width:170px">数据项</div>| <div style="width:260px">类型</div>| <div style="width:290px">说明</div> |
|----| ----| ----|
id| （必须）String|编号
name|（必须）String|名称
isGroup|（可选）Number(0或1)|是否组任务（父任务）,决定一个任务是否可以包含子任务。
iconColsVal| （可选）Array|图像对象(src, title)数组
resId|（可选）String|资源编号
resName|（可选）String|资源名称
plan|（可选）Array|计划对象，包含start,end,dur属性
rsltStart|（可选）String Date|实际开始时间
rsltEnd|（可选）String Date|实际完成时间
pctComp|（可选）Number|完成度
rsltDur|（可选）Number |实际投入
planBarColor|（可选）String| 甘特图条颜色（计划）
preNodes| （可选）String|Array|前置节点
parentId|（可选）String |（可选）String|父任务编号
caption|（可选）String |标题
isMS|（可选）Number(0或1)|是否里程碑
自定义列1|（可选）String|自定义列
自定义列2|（可选）String|自定义列
custRsltBars|（可选）Array|分段进度条(name, title, start, end, html, style)数组

```javascript
// ****************** 弹框示例参考如下 ******************
// 第一步：使用弹框，弹框确认时构造如下数据
var task = {};

task.name = "测试任务1";                                               // (必须)任务名称
task.isGroup = 0;                                                     // (可选)是否父任务（组）
task.iconColsVal = [{"src": "./images/test.jpg","title": "",}];       // (可选)图标列值
task.resId = "01";                                                    // (可选)担当者id
task.resName = "刘德华";                                              // (可选)担当者姓名（可选)
task.plan = [{"start": "2023-12-20","end": "2023-12-25","dur": 5}];   // (可选)计划数据
task.planBarColor = "#FF0000";                                        // (可选)进度条颜色
task.rsltStart = "2023-12-20";                                        // (可选)实际开始
task.rsltEnd = "2023-12-26";                                          // (可选)实际完成
task.pctComp = 90;                                                    // (可选)完成百分比
task.rsltDur = 6;                                                     // (可选)完成量
task.preNodes = [{"id":13,"type":"FS","gapDays":1}];                  // (可选)前置任务（可多个）
task.parentId = "";                                                   // (saveType为add/append/insert时可选；为addChild时必须)父任务
task.caption = "测试任务1";                                            // (可选)任务标题
task.isMS = 0;                                                        // (可选)是否里程碑任务

// 第二步: 更新任务行数据
var ret = MZGantt.updRows(saveType, createTaskModel());               // saveType: add/append/insert/edit/addChild
if (ret.code == 0) {
    // 关闭任务输入框

} else {
    // 提示错误消息
    console.log(ret.msg);
    return;
}
```
</div>

## 五. 高级功能

MZGantt支持更多高级功能，如国际化和多语言包设置，拖动建立并编辑任务关系，加载动画配置等。

1. 国际化和语言包设置
    ```javascript
    // 设置语言包
    //   注意：设置语言类型，需要在甘特图显示方法（drawGantt）之前执行。
    myGantt.setGanttLang("en");        // cn: 中文; en:英语; jp:日语;

    // 字符重命名（以下字符可重命名）
    //     count_tab: "第",
    //     reserve_pic: "预约者",
    //     theme: "主题",
    //     loading:"加载中...",
    //     click_2_view:"查看图文",
    //     column_task: "任务名称",
    //     column_complete_ratio: "完成度",
    //     column_rslt: "实际起止日期",
    //     column_start_date: "计划开始时间",
    //     column_end_date: "计划完成时间",
    //     column_rslt_start_date: "实际开始时间",
    //     column_rslt_end_date: "实际完成时间",
    //     week: "周",
    //     quarter: "季度",
    //     menu_goto:"转到任务",
    //     menu_goto_today:"定位到今天",
    //     select_child:"选择所有子任务",
    //     menu_FF: "完成-完成(FF)",
    //     menu_FS: "完成-开始(FS)",
    //     menu_SF: "开始-完成(SF)",
    //     menu_SS: "开始-开始(SS)"
    var labelDefs = {
        "menu_SF":"开-完"}
    ;

    // 国际化设置
    myGantt.setMyLabels(labelDefs, "en");
    ```
2. 进度条消息框自定义
    鼠标放在进度条上，会自动显示提示框，显示任务信息。也可以根据需求，自定义提示框。
    ```javascript
    // 在甘特图配置中设置参数infoBoxItems和infoBoxStyle
    var ganttConfig = {
        ...
        infoBoxItems: [       // 信息框项目定义
            {
                title: '',value: "name",
                titleStyle: "font-weight: bold;",
                valStyle: "font-weight: bold;"},
            {
                title: '公式写法',	expression: "((yourColName1 * yourColName3) + pctComp) + '天'",
                titleStyle: "font-weight: bold;",
                valStyle: "color: red;"},
            {
                title: '格式化字串',	value: "这是格式化写法: {planStart}",
                titleStyle: "font-weight: bold;",
                valStyle: "color: red;"},
            {
                title: '计划开始',	value: "planStart",
                titleStyle: ""},
            {
                title: '计划结束', value: "planEnd",
                titleStyle: "", },
            {
                title: '实际开始',	value: "rsltStart",
                titleStyle: ""},
            {
                title: '实际结束', value: "rsltEnd",
                titleStyle: "", },
            {
                title: '标题', value: "title",
                titleStyle: "", }
        ],
        infoBoxStyle:        // 信息框样式定义
            "BORDER: #cdcdcd 1px solid;background:#FFFFFF;box-shadow: 3px 3px 2px #CDCDCD;border-radius:5px;padding:5px;",

        ...
    }
    ```
    信息项目的设置，可以选择以下任意一种方式：
    1. 使用值（vulue）
    2. 使用公式（expression）
        * 日期/字符类型的值：须加上引号，比如:
            > expression: "这是实际开始日: 'rsltStart'"
        * 数值型的值：直接写在公式中即可，比如：
            > expression: "((yourColName1 * yourColName3) + pctComp) + '天'"
    3. 使用字符替换（{...}）

3. 任务关系
* <b>建立任务关系：</b>
编辑状态下，在A任务进度条上按下鼠标向下拖动，拖动到B任务松*开鼠标，会弹出任务关系弹框，选择任务关系，点击确定即可建立任务A和B之间的关系。
* <b>编辑任务关系：</b>
编辑状态下，点击关系线，弹出任务关系弹框，修改或删除任务关系。

* <b>间隔时间修改方法：</b>
    方法1：在关系弹框中修改保存；
    方法2：拖动后置任务，自动修改。

4. 加载动画设置
    MZGantt支持显示加载动画。分为插件初始化加载动画和插件手动开关动画两种，请参考下例。
    ```javascript
    // 1. 插件初始化动画**************************************************
    // 1.1 普通js版本时
    <!-- 导入甘特图插件 -->
	<link rel="stylesheet" type="text/css" href="../../gantt/mzgantt.css" />
	<script language="javascript">
        // 参数：show: 是否显示插件初始化动画，默认不显示；containerId：显示到甘特图容器dom对象id，默认整个窗口
		window.showInitLoading = {show:true, containerId: "GanttChartDIV"};
	</script>
	<script language="javascript" src="../../gantt/mzgantt.js"></script>
	...

    // 1.2 npm版本时
	// 初始化甘特图插件（参数解释同上）
    var showInitLoadingMsg = {show:true, containerId: "GanttChartDIV"};
    const myGantt = MZGantt.init(showInitLoadingMsg);
	...

    // 2. 手动开关动画****************************************************
    //    手动开关打开加载动画时，普通js版本和npm版本使用相同方法。
    //    参数1：显示开关（true：显示；false：关闭）
    //    参数2：显示位置dom对象ID（默认整个窗口）
    myGantt.showLoading(true,"GanttChartDIV");
    ```

## 六. 插件方法及事件

MZGantt提供丰富接口，操作或控制甘特图数据和特征。
1. 甘特图显示
    |<div style="width:150px">方法</div>|<div style="width:166px">说明</div>|<div style="width:200px">参数</div>|
    |----| ----|----|
    |init|初始化甘特图实例|无（vue中使用插件时，需要执行此方法实例化插件）|
    |createGantt| 创建甘特图对象| 参数1：（必须）DIV容器ID<br>参数2：（可选）时间刻度day/week/month/quarter（默认day）<br>参数3：配置选项<br>参数4：(可选)数据|
    |config|配置甘特图|JSON对象值，参考：甘特图参数配置|
    |bindGanttData| 绑定甘特图数据|甘特图JSON数据：需要符合MZGantt数据model|
    |drawGantt|显示甘特图|无|

2. 显示控制
    |<div style="width:150px">方法</div>|<div style="width:248px">说明</div>|<div style="width:200px">参数</div>|
    |----| ----|----|
    |changeFormat|变更显示刻度|显示刻度：day/week/month/quarter|
    |switchTrack| 切换显示实绩|开关值：true/false|
    |showMileStone|切换显示里程碑时间标记线|开关值：true/false|
    |setMSLine|动态添加/修改里程碑时间标记线|里程碑定义对象（参考mileStoneLines中对象定义）|
    |rmMSLine|动态删除里程碑时间标记线|里程碑标记线名(name)|
    |showCriticalPath|切换显示关键路径|开关值："1"/"0"|
    |setRowReadonly|设置行只读条件|条件判断函数。<br>例，设置新添加行只读时，代码如下：<br>`myGantt.setRowReadonly(function(row) {`<br>&nbsp;&nbsp;&nbsp;&nbsp;`return row.operation != "new" && row.operation != "newmodified";`<br>`});`|
    |adjustGanttHeight|动态调整甘特图区域高度，可实现甘特图高度自适应|甘特图区域高度（像素值）|
    |setColsVisiable|动态设置列是否可见|参数请参考示例，设置colname1和colname2不可见：<br>`myGantt.setColsVisiable({colname1: false, colname2: false})`|
    |fitGanttWidth|动态调整甘特图右侧时间宽度，使宽度刚好填满右侧进度条区域|无|
    |fitMobile|使甘特图适配移动端显示（自动横屏）|无|
    |hideGanttBar|隐藏/显示右边甘特图条区域。|开关值：true/false|

3. 获取数据
    |<div style="width:150px">方法</div>|<div style="width:360px">说明</div>|<div style="width:240px">参数</div>|
    |----| ----|----|
    |getAllRows | 获取所有行数据|无|
    |getUpdatedRows |获取值有变化的行|无|
    |getSelectedRows |获取当前选择行|无|
    |getAllParentRows | 获取所有组任务|无|
    |getChildRows|获取指定行的所有子任务数据|任务ID|
    |getSelectedRowID |获取选择行任务ID|无|
    |getSelectedBarID|获取选择进度条ID（资源管理时用）|无|
    |getSelectedRowSeq|获取当前选择行行索引号|无|
    |getSelectedRowParent|获取选择行父任务ID|无|
    |getMileStoneLines|获取里程碑时间线数据|无|

4. 属性设置
    |<div style="width:100px">方法/div>|<div style="width:354px">说明</div>|<div style="width:215px">参数</div>|
    |----| ----|----|
    |setBulkMoveType|同步移动方式设置|D（关联任务）<br>P（按责任人）<br>N（所有后续任务）|
    |setGanttStatus|设置甘特图编辑状态|参数：状态字符（r: 只读；e:编辑）<br>。字符串类型|
    |setColEditable|动态设置列 可编辑/只读|参数：键值对象(列名name:true/false)。<br>例：{'planStart':false,'planEnd':false}<br>true:可编辑；false：只读|
    |setStartEndDate|设置项目起止日|参数1：开始日。字符串类型<br>参数2：结束日。字符串类型<br>参数3：是否锁定日期范围(true/false)。字符串类型|
    |setToDate|设置甘特图显示日期|参数1：显示目标日期。字符串类型。<br>参数2：是否自动适配宽度(true/false)<br>例：myGantt.setToDate('2025-04-16', true);<br>|

5. 数据编辑类
    |<div style="width:130px">方法</div>|<div style="width:124px">说明</div>|<div style="width:240px">参数</div>
    |----| ----|----|
    |addRow|新增行|参数：新增位置 <br>&nbsp;&nbsp;&nbsp;&nbsp;add:当前行后；<br>&nbsp;&nbsp;&nbsp;&nbsp;insert：当前行前；<br>&nbsp;&nbsp;&nbsp;&nbsp;append：末尾；<br>&nbsp;&nbsp;&nbsp;&nbsp;addChild：添加子任务|
    |deleteRows|删除选择行|无|
    |cloneRows |克隆选择行|无|
    |updRows|更新行数据|参数1：新增位置 <br>&nbsp;&nbsp;&nbsp;&nbsp;add:当前行后；<br>&nbsp;&nbsp;&nbsp;&nbsp;insert：当前行前；<br>&nbsp;&nbsp;&nbsp;&nbsp;append：末尾；<br>&nbsp;&nbsp;&nbsp;&nbsp;addChild：添加子任务；<br>&nbsp;&nbsp;&nbsp;&nbsp;edit：修改<br> 参数2：任务模型数据<br> <br>注意：<br>行内编辑时，无需使用该方法。<br>该接口适合使用外部任务编辑弹框时，用于提交通过外部编辑弹框得到的数据（参数2）。|
    |resetGantt |重置所有任务行编辑状态|无|
6. 过滤器
    |<div style="width:170px">方法</div>| <div style="width:592px">说明</div>
    |----| ----|
    addFilter|设置条件对数据进行过滤。示例：<br>` MZGantt.filterRows((task) =>{return task.duration > 3;},"onlyShow")`
    removefilter|移除过滤器，恢复数据显示。示例：<br>`MZGantt.removefilter()`

7. 导出图片
    |<div style="width:170px">方法</div>| <div style="width:190px">说明</div>| <div style="width:375px">参数</div>
    |----| ----|----|
    exportGanttImg |存为图片| 无

8. 事件监听
    |<div style="width:230px">事件名称</div>| <div style="width:560px">说明</div>|
    |----| ----|
    |cellRender|单元格渲染事件。<br>支持监听该事件，设置自定义回调处理。对单元格进行渲染。<br>示例：` MZGantt.on("cellRender", function(row, cellObj) {...})`|
    |cellBlur|单元格失去焦点事件。行内编辑单元格失去焦点时触发。示例：<br>` MZGantt.on("cellBlur", function(row, cellName, oldVal, newVal) {...})`|
    |rowRender|表格行渲染事件。<br>支持监听该事件，设置自定义回调处理。对行进行样式渲染。<br>示例：` MZGantt.on("rowRender", function(row, rowStyleObj) {...})`|
    |barRender|进度条渲染事件。<br>可动态监听用户数据，实时渲染进度条样式。<br>示例：` MZGantt.on("barRender", function(row, barObj) {...})`|
    |clickBar |进度条点击事件。<br>示例：` MZGantt.on("clickBar", function(row, eventXY, dom) {...})`|
    |dblClickBar |进度条双击击事件。<br>示例：` MZGantt.on("dblClickBar", function(row, barData, dom) {...})`|
    |clickRow |点击行事件。<br>示例：` MZGantt.on("clickRow", function(row, eventXY) {...})`|
    |rightClick/rightClickBar|进度条右键点击事件。<br>示例：` MZGantt.on("rightClickBar", function(row, eventXY, dom) {...})`|
    |loaded|加载完成事件。<br>甘特图加载完成后自动执行，可根据需要设置用户自己的处理逻辑。<br>示例：`MZGantt.on("loaded", function(e, data) {...})`|
    |barDragEnd|进度条拖拽结束事件。拖拽进度条释放时触发。示例：<br>` MZGantt.on("barDragEnd", function(row, updRows) {...})`|
    |rowDragEnd|行拖拽结束事件。拖拽行并释放时触发。示例：<br>` MZGantt.on("rowDragEnd", function(row, updRows) {...})`|


## 七. 常见错误码
<div style="margin-left:30px">

|<div style="width:230px">错误码</div>| <div style="width:560px">说明</div>|
|----|----|
|0x00010|createGantt命令执行之前，甘特图容器div必须要加载完成，否则提示此错误。|
|0x00020|配置甘特图出错。|
|0x00030|渲染甘特图（drawGantt命令）出错。|
|0x00040|渲染里程碑标记线时出错。|
|0x00050|绑定数据发生异常。请参考<四. 甘特图数据行数据模型>作成甘特图行数据。|
|0x00060|批量加载任务数据异常。|
|0x00080|点击单元格时发生错误。|
|0x00090|点击进度条时发生错误。|
|0x00100|插件初始化异常。|
</div>

## 八. 下载
1. 普通js版
    进入下载： <a href="https://gitee.com/tecjt_home/mzgantt_js">MZGantt甘特图插件（普通js版）</a>
    或者使用CDN：
        <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.6/cdn/mzgantt.css"></script>
        <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.6/cdn/mzgantt.js"></script>
        <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.6/cdn/edit.js"></script>
        <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.6/cdn/export.js"></script>
        <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.6/cdn/mobile.js"></script>

2. npm版（支持vue等）：直接使用npm命令安装即可。
    > npm install mzgantt

## 九. 版本

MZGantt提供3个版本，以满足不同用户的需求。。

1. 免费试用版
    无须任何授权，下载后可免费使用（功能部分受限），不影响您的开发。

2. 个人版
    下载后需获取个人版授权key，无功能限制，可自由地使用插件中的各种功能。

3. 商业版
    下载后需获取商业版授权key，无功能限制和商业使用限制，可自由使用插件中的各种功能。

## 十. 联系方式

1. 手机：1369 5047 200 (王先生)
2. 微信：
<img src="image.png" width="200" />


