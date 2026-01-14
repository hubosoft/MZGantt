
# MZGantt Gantt Chart Plugin
MZGantt is a native JavaScript Gantt chart plugin. It supports Vue, TypeScript, JavaScript and other popular frontend frameworks, and can be quickly integrated into web applications or mobile apps.

## I. Plugin Features

* <b>Drag & Drop Editing:</b> Supports drag and drop editing
* <b>Inline Editing:</b> Supports inline editing functionality
* <b>Modal Support:</b> Provides task model to work with external modals for task editing

* <b>Customizable Columns:</b> Supports custom task attribute columns
* <b>Multi-segment Display:</b> Supports multi-segment display within task rows
* <b>Calendar Support:</b> Supports custom calendars
* <b>Image Export:</b> Supports full Gantt chart image export

* <b>Big Data Processing:</b> Supports lazy loading for tasks
* <b>Filter Support:</b> Supports setting conditions for quick filtering
* <b>Milestone Markers:</b> Supports configuring milestone marker lines
* <b>Four Task Relationships:</b> Supports drag-and-drop creation and editing of four task relationships (FS, FF, SF, SS)
* <b>Customizable Tooltips:</b> Supports custom tooltip content and styles

* <b>Customizable Styles:</b> Supports custom display styles
* <b>Easy to Use:</b> Direct JS reference or npm installation (supports Vue, TypeScript)
* <b>Rich Configuration:</b> Provides extensive configuration parameters and interfaces to meet project requirements

## II. Demo

Demo URL: <https://mzgantt.com/en/demo>

## III. How to Use?

MZGantt supports two usage methods: JS reference and npm installation.

1. Download
    Before use, download the standard JS version or install via npm.

2. Reference MZGantt Plugin
    Include the following JS packages in the HTML page header:
    * Standard JS reference:
    ```javascript
    <!-- Include jQuery library -->
    <script language="javascript" src="jquery.min.js"></script>

    <!-- Include Gantt chart plugin -->
    <link rel="stylesheet" type="text/css" href="./gantt/mzgantt.css" />
    <script language="javascript" src="./gantt/mzgantt.js"></script>
    <script language="javascript" src="./gantt/edit.js"></script>
    ```

    * npm reference:
    ```javascript
    // Import MZGantt packages, include the following packages as needed:
    //      1. Core package: MZGantt (required, includes basic Gantt chart display)
    //      2. Editor package: MZGanttEditor (optional, includes editing functionality)
    //      3. Export package: MZGanttExport (optional, includes image export functionality)
    //      4. Mobile package: MZGanttMobile (optional, includes mobile support)
    import {MZGantt, MZGanttEditor} from '/node_modules/mzgantt';

    // Get and set license key
    MZGantt.LicenseKey = "Your license key";
    ```

3. Define Gantt Chart Container (div)
    Define a div in the HTML page as the container for displaying the Gantt chart:
    ```html
    <div id="GanttChartDIV" style="width:100%;"></div>
    ```

4. Define Gantt Chart Parameters (can define empty object {}, plugin will use default values)
    This step is optional. When personalized display is needed, set Gantt chart parameters. Some configurable parameters are as follows:
    ```javascript
    var ganttConfig = {
        useFor: "Task",                         // Set Gantt chart purpose: 'Resource' for resource utilization (e.g., meeting room management), 'Task' for task management (e.g., planning and progress). Default: Task
        ganttStatus: "e",                       // Set Gantt chart status (e: edit plan, r: read-only)

        // Column configuration (left table column display configuration, not Gantt data format)
        columnDefs: [
            {name: 'seq',         field: "seq",        type:"level"},
            {name: 'checkbox',    field: "checkbox"},
            {name: 'iconColsVal', field: "iconColsVal", width: 30},

            // For the following fields, name can be customized and must match corresponding item names in data. field values are built-in fixed values, cannot be reset, and do not support type setting.
            {name: 'name',        field: "name",       title: "Task Name",          width: 150},
            {name: 'resId',       field: "resId",      title: "Responsible Person", width: 50,    options: resourceList},
            {name: 'dur',         field: "dur",        title: "Duration",           width: 60},
            {name: 'planStart',   field: "planStart",  title: "Plan Start",         width: 120,   editable: false},
            {name: 'planEnd',     field: "planEnd",    title: "Plan End",           width: 120,   colCallBack:custCallbackFunc1},
            {name: 'planDur',     field: "planDur",    title: "Plan Days",          width: 60},
            {name: 'rsltStart',   field: "rsltStart",  title: "Actual Start",       width: 120},
            {name: 'rsltEnd',     field: "rsltEnd",    title: "Actual End",         width: 120},
            {name: 'rsltDur',     field: "rsltDur",    title: "Actual Days",        width: 60},
            {name: 'pctComp',     field: "pctComp",    title: "Completion",         width: 60},
            // {name: 'caption',  field: "caption",    title: "Label Text"},

            // Custom columns (field: "custCol"): supports type setting: DropDownList/TextBox
            // Attribute altText: custom column replacement display text
            {name:"yourColName1", field: "custCol", title:"Column 1", type: "TextBox", default: 10, width:50,align:"left"},
            {name:"yourColName2", field: "custCol", title:"Column 2", type: "DropDownList", options:dropDownListData, width:100, align:"left"},
            {name:"yourColName3", field: "custCol", title:"Column 3","width":150,"align":"left", colCallBack:custCallbackFunc2, altText: "View Image"}
        ],

        // Gantt chart area display control
        showTrack: 1,                           // 0: Plan Gantt; 1: Actual Gantt. Default: Plan Gantt
        showDuplicate: 0,                       // Whether to show resource overlap indicator
        showMileStone: 1,                       // Whether to show milestone: 0: not show
        showDependencies: 1,                    // Whether to show connection lines (0/1), default: 0 (not show)
        showDayDate: "date",                    // When day format is set, display weekday or date (day: weekday; date: date). Default: day shows weekday
        showWeekIndex: "",                      // When day format is set, right header shows week number or first date of week. Y: show week number; others: show date. Default: show date

        // Data definition class===============================================================================
	    weekWorkSetting:                        // Weekly workday settings (D1: Monday...D0: Sunday. 1: workday, 0: rest day)
		    {D1:1, D2:1, D3:1, D4:1, D5:1, D6:0, D0:0},
        holidayList: [                          // Set holiday list. When weekends are normally displayed, add dates in the list as holidays, displayed in gray
            '2023-02-09',
            '2023-02-21',
            '2023-03-20'],
        workdayList: ['2023-03-18'],            // Set workday list
        mileStoneLines: [                       // Milestone configuration
            {date:'2023-11-05',name:'startMS', title:'Project Design Discussion', color:'#FF00BF', thickness: "2px"},
            {date:'2023-11-25',name:'pjMeeting', title:'Code Review', color:'#088A29', thickness: "2px"},
            {date:'today',name:'today', title:'Today', color:'#FF0000', thickness: "2px"}
        ],

        // Plugin configuration class===============================================================================
        captionType: 'Caption',                 // Set Gantt chart title (None, Name, Caption, Resource, Duration, Complete, Reserver, Title, Theme). None: do not show title
        captionPosition: 'Right',               // Set Gantt chart title position (Over: above Gantt bar; Right: right side of Gantt bar, default: Over)

        // Display date format and input date format should be unified
        dateFormat: 'yyyy-mm-dd',               // Set parameter date format ('mm/dd/yyyy', 'dd/mm/yyyy', 'yyyy-mm-dd','yyyy/mm/dd')
        weekStartDay: 0,                        // Set first day of week (1: Monday; 0: Sunday), default: 0

        workingHourRange: ['1','23'],           // Set daily working hours in hour format (in hours). Default: 1,23
        hourFormat: 0,                          // Set hour format (0: hour without :00; 1: hour with :00). Default: 0 shows weekday

        dragChgDur: "1",                        // Set drag to change workdays. If showChgDur is set to not show, this option is invalid
        autoCalDur: 1,                          // Set automatic calculation of workdays (default: not auto-calculate)

        includeHoliday: "N",                    // Whether workday calculation includes holidays and weekends: Y: include; N: exclude
        includeToTime: "Y",                     // Whether Gantt completion time includes the last time point (set to include when start and end times are date type, set to exclude for hours), default: Y

        durPrecision: 2,                        // Set duration and workday precision (default: one decimal place)
        fixColsCnt: 3,                          // Set number of locked columns, default: 0 (no locked columns)
	    fixParent: 1,                           // Fix parent task range (1: does not change with child task drag; 0: changes with child task drag)
	    idType: "Snow",                         // Task ID calculation method (default: 12-digit arbitrary characters; UUID: uuid; Snow: snowflake ID)

        // Style control class===============================================================================
        // Background color settings
        // planBackground: vPlanBackground,     // Set Gantt plan bar background
        // resultBackground: vResultBackground, // Set Gantt actual bar background
        compBackground: '#0000FF',              // Set completion background
        // groupBackground: '#00FF00',          // Set parent task row background
        holidayBGColor: '#E6E6E6',              // Set rest day/holiday background
        currentTimeBackColor: ['h','#FF9797'],  // Set current time background (param1: h: show current time via header style; c: show current time via cell style. param2: color value)
        intervalColor: ['#FFFFFF','#F4F4F4'],   // Set alternating row background color, default: #FFFFFF,#F4F4F4
        selectedRowColor: '#CAE1FF',            // Set selected row background color, default: #CAE1FF
        selectedCellColor: '#2E9AFE',           // Set selected cell border color, default: 2E9AFE
        borderColor: '#9E9E9E',                 // Set Gantt border color, default: #9E9E9E

        // Gantt bars
        barBorderRadius: 6,                     // Set Gantt bar border radius, default: 3px
        barBorderWeight: 1,                     // Set Gantt bar border weight, default: 1px
        barBorderColor: '#9E0000',              // Set Gantt bar border color, default: #9E9E9E
        barOpacity: 1,                          // Set Gantt bar opacity, default: 0.7
        barGridBorderWeight: 1,                 // Set Gantt bar background border weight, default: 1px
        resultBarStyle: 'mark',                 // Actual bar style (normal: do not mark dates outside plan, mark: mark dates outside plan; cust: custom segments)
        markResultBarColor:                     // Actual bar background color when resultBarStyle is mark type
		{beforePlanBGColor: '#00FF00',
		 exceedPlanDBColor: '#cc0000'},
        barHeightAdj: 0,						// Progress bar height adjustment (default 0px, no adjustment; larger value = higher bar. Can be negative)
	    barDistanceAdj: 0,						// Distance adjustment between plan and actual progress bars (default 0px, no adjustment; larger value = closer. Can be negative)

        // Drag handle
        dragHandlerBackColor: '#D8D8D8',        // Drag handle background color

        // lineExpandLen: 0,                    // Dependency line extension parameter
        dependLineColor: '#228B22',             // Connection line color
        dependLineMouseOverColor: '#FE9A2E',    // Connection line mouse hover color
        criticalPathBGColor: '#FE9A2E',         // Critical path marker color

        // Width and height settings
        leftSideWidth: 300,                     // Set left panel width, default: 300px
        contentHeight: 360,                     // Set Gantt chart height (excluding header), default: 300px. If adjustGanttHeight function is called on page for auto-height, this parameter can be omitted
        rowHeight: 35,                          // Set row height, default: 35px

        // Left column width settings, can be deprecated after columnDefs is complete
        iconColWidth: 30,                       // Set icon column width, default: 60px
        nameColWidth: 150,                      // Set task/resource column width, default: 150px
        dateColWidth: 120,                      // Set date column width, default: 80px

        // Right grid width settings (for different time scales)
        hourColWidth: 20,                       // Hour scale
        dayColWidth: 20,                        // Day scale
        monthColWidth: 70,                      // Month scale
        quarterColWidth: 100                    // Quarter scale
    };

    // Gantt chart parameter configuration
    myGantt.config(ganttConfig);
    ```

5. Get data from backend, or define data

    ```javascript
    /*
      Gantt chart data object item description:
      I: Built-in fixed data items, cannot customize other names
        1. id                  // (Required) String          Node ID (any string or number, must be unique)
        2. plan                // (Optional) Array           Plan data
        3. isExpand            // (Optional) Number(0 or 1)  Whether to expand display 1: expand/0: collapse
        4. isGroup             // (Optional) Number(0 or 1)  Whether to set as parent (group) 1: group/0: leaf node
        5. preNodes            // (Optional) String|Array    Predecessor nodes, use comma to separate multiple predecessor nodes
        6. parentId            // (Optional) String          Parent node ID (when current row is top-level node, this value must be set to empty "")
        7. isMS                // (Optional) Number(0 or 1)  Whether milestone
        8. caption             // (Optional) String          Title
        9. planBarColor        // (Optional) String          Gantt plan background (color value)

      II: The following data items can use custom names and must match the name in column definitions
        10. name               // (Required) String          Name (can be task name, resource name, etc.)
        11. iconColsVal        // (Optional) Array           Icon column (can be multiple icons separated by comma)
        12. rsltStart          // (Optional) String Date     Actual start
        13. rsltEnd            // (Optional) String Date     Actual end
        14. rsltDur            // (Optional) Number          Actual workdays
        15. resId              // (Optional) String          Resource/Responsible person
        16. pctComp            // (Optional) Number          Completion (0 ~ 100 percentage)
        17. seq                // (Optional) Decimal         Sort number
        18. yourColName        // (Optional) String          Custom column value, key needs to match custom column name
    */

    // Gantt chart data
    let ganttData= [
        {
            "id": 1,
            "seq": 1000,
            "iconColsVal": [],
            "name": "Unit A Construction Period",
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
            "caption": "Key Focus",
            "yourColName1": "10",
            "yourColName2": "02",
            "yourColName3": "<img src='./images/stop.png'>",
        }, {
            "id": 11,
            "seq": 2000,
            "iconColsVal": [{
                    "src": "./images/stop.png",
                    "title": "Stop"
                }
            ],
            "name": "Task Name 1",
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
                    title: "Segment 1",
                    start: "2023-07-02",
                    end: "2023-07-04",
                    style: "background:#2E9AFE;color:#FFFFFF;text-align:left;"
                }, {
                    name: "p2",
                    title: "Segment 2",
                    start: "2023-07-06",
                    end: "2023-07-17",
                    html: "<span style='height:7px;border-radius:5px 5px;position:absolute;left:5px;top:-1px;background:red;font-size:9px;color:#FFFFFF'>Beautiful</span>",
                    style: "background:#FF0000;color:#FFFFFF;text-align:left;"
                },
            ],
        }, {
            "id": 12,
            "seq": 3000,
            "iconColsVal": [],
            "name": "Task Name 2",
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
            "caption": "Note 1",
            "yourColName1": "1",
            "yourColName2": "03",
            "yourColName3": "",
        }, {
            "id": 13,
            "seq": 4000,
            "iconColsVal": [{
                    "src": "./images/star.png",
                    "title": "Star"
                }
            ],
            "name": "Task Name 3",
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
            "name": "Task Name 4",
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
            "name": "Unit B Construction Period",
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
                    "title": "Stop"
                }
            ],
            "name": "Task Name 1",
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


6. Create Gantt Chart Object

    ```javascript
    // Instantiate Gantt chart object (in Vue, this statement is needed to instantiate the Gantt chart object)
    const myGantt = MZGantt.init();

    // Start functional modules
    //   (1) When using npm package, if editing, export and mobile support are needed, start them here after importing corresponding packages
    //   (2) When using standard JS package, the following start commands can be omitted
    // Start Gantt chart editing
    MZGanttEditor.start(myGantt);
    // Start export functionality
    MZGanttExport.start(myGantt);

    /*
    Create and display Gantt chart
        Parameter 1: (Required) Container div object id
        Parameter 2: (Optional) Time scale type
        Parameter 3: (Optional) Configuration options
        Parameter 4: (Optional) Data
    */
    myGantt.createGantt("GanttChartDIV", "day", ganttConfig, data);

    // Note: If ganttConfig and data are not set here, you can use configuration method and data binding method separately
    // Gantt chart parameter configuration
    myGantt.config(ganttConfig);

    // Load data
    myGantt.bindGanttData(data);

    // Display Gantt chart
    myGantt.drawGantt()
    ```

7. Event Listeners
    MZGantt supports listening to rendering or click events, which can be used to implement special control effects.

    ```javascript
    // Cell rendering
    myGantt.on("cellRender", function (row, cellObj) {
        // *******************************************************
        // Can control rendering of the following cell types (field) in the left table of Gantt chart:
        // I. Built-in type columns:
        //    name                 Name
        //    iconColsVal          Icon column
        //    resId                Resource/Responsible person
        //    planStart            Plan start
        //    planEnd              Plan end
        //    planDur              Plan days
        //    rsltStart            Actual start
        //    rsltEnd              Actual end
        //    rsltDur              Actual workdays
        //    pctComp              Completion

        // II. Custom columns
        //    yourColName1         Custom column name (name)
        // *******************************************************

        // Get cell name (rendering object cell)
        var field = cellObj.field;

        // Define rendering style object
        var cellStyle = {};

        // Style setting: Task name
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

        // Style setting: Responsible person
        if (field == "resId") {
            if (row.name == "Task Name 1") {
                cellStyle["ce11Style"] = 'color:green;font-weight:bold';
            }
        }

        // // Style setting: Custom column (using custom column name)
        // if (field == "yourColName2") {
        //   cellStyle["cellHTML"] = '<input id="tstBtn" type ="button" value="Hide Milestone" onclick="test1">';
        // }

        return cellStyle;
    });

    // Progress bar rendering
    myGantt.on("barRender", function (row) {
        // Define rendering style object
        var barStyle = {};

        if (row.name === "Task Name 2") {
            barStyle["planBarStyle"] = 'background:red;';
            // barStyle["planBarLeftHtml"] = '<img width="20px" height="20px" src="./images/work.svg" />';
            // barStyle["planBarOverHtml"] = '';
            barStyle["planBarRightHtml"] = '<div style="display:block;">Hard Work is Cool</div>';
        }

        // Return object
        return barStyle;
    });

    // Click progress bar
    myGantt.on("clickBar", function (row, eventXY, dom) {
        console.log("click: Task: " + row.name + ", Click date: " + eventXY.date);
    });

    // Right-click progress bar
    myGantt.on("rightClick", function (row, eventXY, dom) {
        console.log("right click: Task: " + row.name + ", Click object id: " + dom.id);
        console.log("Click date: " + eventXY.date);
    });
    ```

8. Multiple Loading Methods
    To improve loading efficiency, the plugin supports multiple loading methods.
    ###### Method 1: Normal Loading (load and render all tasks at once)
    ```javascript
    // Create Gantt chart object and configure
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);
    // Bind data
    myGantt.bindGanttData(data);

    // ※ The above three steps can be simplified to:
    myGantt.createGantt("GanttChartDIV", "day", ganttConfig, data);
    ```

    ###### Method 2: Data Listener Rendering Loading
    ```javascript
    // Configuration parameters
    var ganttConfig = {
        loadType: "listenLoad"
    };

    // Create Gantt chart object and configure
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);

    // Define Gantt chart listener
    var dataListener = myGantt.listener;

    // Set Gantt chart value, automatically drive plugin to update display
    dataListener.rawGanttData = [
        {
            "id": 1,
            "seq": 1000,
            "iconColsVal": [],
            "name": "Unit A Construction Period",
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
            "caption": "Key Focus",
            "plan": [{ "start": "2023-07-05", "end": "2023-07-06", "dur": 10 }]
        }
    ];

    // No need to execute bindGanttData and drawGantt methods
    ```

    ###### Method 3: Paginated Rendering (load all data at once, render in pages)
    ```javascript
    // Configuration parameters
    var ganttConfig = {
        loadType: "loadAllToPage",
        pageSize: 20         // Pagination page size
    };

    // Create Gantt chart object and configure
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);

    // Bind data (data displayed page by page)
    myGantt.bindGanttData(data);
    ```

    ###### Method 4: Paginated Loading Rendering (load and render a certain number of rows each time, load and render page by page)
    ```javascript
    // Pagination loading handler
    //   Parameter pageNo is the page data requested by plugin, starting from 1
    function loadPageData(pageNode) {
        // Return Gantt chart data for specified page
        ...
    }

    // Configuration parameters
    var ganttConfig = {
        loadType: "loadByPage",
        pageSize: 20,               // Pagination page size
        loadDataFunc: loadPageData  // Pagination loading handler
    };

    // Create Gantt chart object and configure
    myGantt.createGantt("GanttChartDIV", "day");
    myGantt.config(ganttConfig);

    // Gantt chart rendering
    myGantt.drawGantt();
    ```

## IV. Gantt Chart Data Row Data Model
The table below shows the MZGantt Gantt chart data row data model. Before binding data to the Gantt chart, data must be created in this format. Can bind in batch, refer to bindGanttDate method; can also bind single row, mostly used with external modals.

<div style="margin-left:30px">

|<div style="width:170px">Data Item</div>| <div style="width:260px">Type</div>| <div style="width:290px">Description</div> |
|----| ----| ----|
id| (Required) String|ID
name|(Required) String|Name
isGroup|(Optional) Number(0 or 1)|Whether group task (parent task), determines if a task can contain child tasks
iconColsVal| (Optional) Array|Image object (src, title) array
resId|(Optional) String|Resource ID
resName|(Optional) String|Resource name
plan|(Optional) Array|Plan object, contains start, end, dur attributes
rsltStart|(Optional) String Date|Actual start time
rsltEnd|(Optional) String Date|Actual end time
pctComp|(Optional) Number|Completion
rsltDur|(Optional) Number |Actual input
planBarColor|(Optional) String| Gantt bar color (plan)
preNodes| (Optional) String|Array|Predecessor nodes
parentId|(Optional) String |(Optional) String|Parent task ID
caption|(Optional) String |Title
isMS|(Optional) Number(0 or 1)|Whether milestone
Custom Column 1|(Optional) String|Custom column
Custom Column 2|(Optional) String|Custom column
custRsltBars|(Optional) Array|Segmented progress bar (name, title, start, end, html, style) array

```javascript
// ****************** Modal example reference as follows ******************
// Step 1: Use modal, construct the following data when modal is confirmed
var task = {};

task.name = "Test Task 1";                                            // (Required) Task name
task.isGroup = 0;                                                     // (Optional) Whether parent task (group)
task.iconColsVal = [{"src": "./images/test.jpg","title": "",}];       // (Optional) Icon column value
task.resId = "01";                                                    // (Optional) Responsible person ID
task.resName = "Andy Lau";                                            // (Optional) Responsible person name (optional)
task.plan = [{"start": "2023-12-20","end": "2023-12-25","dur": 5}];   // (Optional) Plan data
task.planBarColor = "#FF0000";                                        // (Optional) Progress bar color
task.rsltStart = "2023-12-20";                                        // (Optional) Actual start
task.rsltEnd = "2023-12-26";                                          // (Optional) Actual end
task.pctComp = 90;                                                    // (Optional) Completion percentage
task.rsltDur = 6;                                                     // (Optional) Completion amount
task.preNodes = [{"id":13,"type":"FS","gapDays":1}];                  // (Optional) Predecessor tasks (can be multiple)
task.parentId = "";                                                   // (Optional when saveType is add/append/insert; required when addChild) Parent task
task.caption = "Test Task 1";                                         // (Optional) Task title
task.isMS = 0;                                                        // (Optional) Whether milestone task

// Step 2: Update task row data
var ret = MZGantt.updRows(saveType, createTaskModel());               // saveType: add/append/insert/edit/addChild
if (ret.code == 0) {
    // Close task input box

} else {
    // Show error message
    console.log(ret.msg);
    return;
}
```
</div>

## V. Advanced Features

MZGantt supports more advanced features, such as internationalization and multi-language package settings, drag to create and edit task relationships, loading animation configuration, etc.

1. Internationalization and Language Package Settings
    ```javascript
    // Set language package
    //   Note: Language type setting must be executed before Gantt chart display method (drawGantt)
    myGantt.setGanttLang("en");        // cn: Chinese; en: English; jp: Japanese;

    // Character renaming (the following characters can be renamed)
    //     count_tab: "No.",
    //     reserve_pic: "Reserver",
    //     theme: "Theme",
    //     loading:"Loading...",
    //     click_2_view:"View Image",
    //     column_task: "Task Name",
    //     column_complete_ratio: "Completion",
    //     column_rslt: "Actual Start-End Date",
    //     column_start_date: "Plan Start Time",
    //     column_end_date: "Plan End Time",
    //     column_rslt_start_date: "Actual Start Time",
    //     column_rslt_end_date: "Actual End Time",
    //     week: "Week",
    //     quarter: "Quarter",
    //     menu_goto:"Go to Task",
    //     menu_goto_today:"Go to Today",
    //     select_child:"Select All Child Tasks",
    //     menu_FF: "Finish-Finish(FF)",
    //     menu_FS: "Finish-Start(FS)",
    //     menu_SF: "Start-Finish(SF)",
    //     menu_SS: "Start-Start(SS)"
    var labelDefs = {
        "menu_SF":"Start-Finish"}
    ;

    // Internationalization settings
    myGantt.setMyLabels(labelDefs, "en");
    ```
2. Progress Bar Tooltip Customization
    When mouse hovers over progress bar, a tooltip automatically displays task information. You can also customize the tooltip according to requirements.
    ```javascript
    // Set parameters infoBoxItems and infoBoxStyle in Gantt chart configuration
    var ganttConfig = {
        ...
        infoBoxItems: [       // Tooltip item definition
            {
                title: '',value: "name",
                titleStyle: "font-weight: bold;",
                valStyle: "font-weight: bold;"},
            {
                title: 'Formula',	expression: "((yourColName1 * yourColName3) + pctComp) + ' days'",
                titleStyle: "font-weight: bold;",
                valStyle: "color: red;"},
            {
                title: 'Formatted String',	value: "This is formatted: {planStart}",
                titleStyle: "font-weight: bold;",
                valStyle: "color: red;"},
            {
                title: 'Plan Start',	value: "planStart",
                titleStyle: ""},
            {
                title: 'Plan End', value: "planEnd",
                titleStyle: "", },
            {
                title: 'Actual Start',	value: "rsltStart",
                titleStyle: ""},
            {
                title: 'Actual End', value: "rsltEnd",
                titleStyle: "", },
            {
                title: 'Title', value: "title",
                titleStyle: "", }
        ],
        infoBoxStyle:        // Tooltip style definition
            "BORDER: #cdcdcd 1px solid;background:#FFFFFF;box-shadow: 3px 3px 2px #CDCDCD;border-radius:5px;padding:5px;",

        ...
    }
    ```
    Tooltip item settings can use any of the following methods:
    1. Use value
    2. Use expression
        * Date/string type values: must be quoted, e.g.:
            > expression: "This is actual start date: 'rsltStart'"
        * Numeric type values: write directly in expression, e.g.:
            > expression: "((yourColName1 * yourColName3) + pctComp) + ' days'"
    3. Use string replacement ({...})

3. Task Relationships
* <b>Create Task Relationship:</b>
In edit mode, press mouse on task A progress bar and drag down to task B, release mouse, task relationship modal will pop up, select relationship type, click OK to establish relationship between task A and B.
* <b>Edit Task Relationship:</b>
In edit mode, click relationship line, task relationship modal will pop up, modify or delete task relationship.

* <b>Gap Time Modification Methods:</b>
    Method 1: Modify and save in relationship modal;
    Method 2: Drag successor task, automatically modify.

4. Loading Animation Settings
    MZGantt supports displaying loading animation. There are two types: plugin initialization loading animation and manual on/off animation, please refer to the following example.
    ```javascript
    // 1. Plugin initialization animation**************************************************
    // 1.1 Standard JS version
    <!-- Import Gantt chart plugin -->
	<link rel="stylesheet" type="text/css" href="../../gantt/mzgantt.css" />
	<script language="javascript">
        // Parameters: show: whether to show plugin initialization animation, default: not show; containerId: display to Gantt chart container dom object id, default: entire window
		window.showInitLoading = {show:true, containerId: "GanttChartDIV"};
	</script>
	<script language="javascript" src="../../gantt/mzgantt.js"></script>
	...

    // 1.2 npm version
	// Initialize Gantt chart plugin (parameter explanation same as above)
    var showInitLoadingMsg = {show:true, containerId: "GanttChartDIV"};
    const myGantt = MZGantt.init(showInitLoadingMsg);
	...

    // 2. Manual on/off animation****************************************************
    //    When manually turning on loading animation, standard JS version and npm version use the same method.
    //    Parameter 1: Display switch (true: show; false: close)
    //    Parameter 2: Display position dom object ID (default: entire window)
    myGantt.showLoading(true,"GanttChartDIV");
    ```

## VI. Plugin Methods and Events

MZGantt provides rich interfaces to operate or control Gantt chart data and features.
1. Gantt Chart Display
    |<div style="width:150px">Method</div>|<div style="width:166px">Description</div>|<div style="width:200px">Parameters</div>|
    |----| ----|----|
    |init|Initialize Gantt chart instance|None (when using plugin in Vue, need to execute this method to instantiate plugin)|
    |createGantt| Create Gantt chart object| Parameter 1: (Required) DIV container ID<br>Parameter 2: (Optional) Time scale day/week/month/quarter (default: day)<br>Parameter 3: Configuration options<br>Parameter 4: (Optional) Data|
    |config|Configure Gantt chart|JSON object value, refer to: Gantt chart parameter configuration|
    |bindGanttData| Bind Gantt chart data|Gantt chart JSON data: must conform to MZGantt data model|
    |drawGantt|Display Gantt chart|None|

2. Display Control
    |<div style="width:150px">Method</div>|<div style="width:248px">Description</div>|<div style="width:200px">Parameters</div>|
    |----| ----|----|
    |changeFormat|Change display scale|Display scale: day/week/month/quarter|
    |switchTrack| Switch to show actual|Switch value: true/false|
    |showMileStone|Toggle milestone time marker lines|Switch value: true/false|
    |setMSLine|Dynamically add/modify milestone time marker lines|Milestone definition object (refer to object definition in mileStoneLines)|
    |rmMSLine|Dynamically delete milestone time marker lines|Milestone marker line name (name)|
    |showCriticalPath|Toggle critical path display|Switch value: "1"/"0"|
    |setRowReadonly|Set row read-only condition|Condition judgment function.<br>Example, when setting newly added rows as read-only, code is as follows:<br>`myGantt.setRowReadonly(function(row) {`<br>&nbsp;&nbsp;&nbsp;&nbsp;`return row.operation != "new" && row.operation != "newmodified";`<br>`});`|
    |adjustGanttHeight|Dynamically adjust Gantt chart area height, can achieve Gantt chart height auto-adaptation|Gantt chart area height (pixel value)|
    |setColsVisiable|Dynamically set column visibility|Refer to example for parameters, set colname1 and colname2 invisible:<br>`myGantt.setColsVisiable({colname1: false, colname2: false})`|
    |fitGanttWidth|Dynamically adjust Gantt chart right side time width, make width just fill right progress bar area|None|
    |fitMobile|Make Gantt chart adapt to mobile display (auto landscape)|None|
    |hideGanttBar|Hide/show right Gantt bar area|Switch value: true/false|

3. Get Data
    |<div style="width:150px">Method</div>|<div style="width:360px">Description</div>|<div style="width:240px">Parameters</div>|
    |----| ----|----|
    |getAllRows | Get all row data|None|
    |getUpdatedRows |Get rows with changed values|None|
    |getSelectedRows |Get currently selected rows|None|
    |getAllParentRows | Get all group tasks|None|
    |getChildRows|Get all child task data for specified row|Task ID|
    |getSelectedRowID |Get selected row task ID|None|
    |getSelectedBarID|Get selected progress bar ID (used in resource management)|None|
    |getSelectedRowSeq|Get current selected row index|None|
    |getSelectedRowParent|Get selected row parent task ID|None|
    |getMileStoneLines|Get milestone timeline data|None|

4. Property Settings
    |<div style="width:100px">Method</div>|<div style="width:354px">Description</div>|<div style="width:215px">Parameters</div>|
    |----| ----|----|
    |setBulkMoveType|Synchronous move mode setting|D (Related tasks)<br>P (By responsible person)<br>N (All subsequent tasks)|
    |setGanttStatus|Set Gantt chart edit status|Parameter: status character (r: read-only; e: edit)<br>String type|
    |setColEditable|Dynamically set column editable/read-only|Parameter: key-value object (column name: true/false).<br>Example: {'planStart':false,'planEnd':false}<br>true: editable; false: read-only|
    |setStartEndDate|Set project start and end dates|Parameter 1: Start date. String type<br>Parameter 2: End date. String type<br>Parameter 3: Whether to lock date range (true/false). String type|
    |setToDate|Set Gantt chart display date|Parameter 1: Display target date. String type.<br>Parameter 2: Whether to auto-fit width (true/false)<br>Example: myGantt.setToDate('2025-04-16', true);<br>|

5. Data Editing
    |<div style="width:130px">Method</div>|<div style="width:124px">Description</div>|<div style="width:240px">Parameters</div>
    |----| ----|----|
    |addRow|Add row|Parameter: Add position <br>&nbsp;&nbsp;&nbsp;&nbsp;add: after current row;<br>&nbsp;&nbsp;&nbsp;&nbsp;insert: before current row;<br>&nbsp;&nbsp;&nbsp;&nbsp;append: at end;<br>&nbsp;&nbsp;&nbsp;&nbsp;addChild: add child task|
    |deleteRows|Delete selected rows|None|
    |cloneRows |Clone selected rows|None|
    |updRows|Update row data|Parameter 1: Add position <br>&nbsp;&nbsp;&nbsp;&nbsp;add: after current row;<br>&nbsp;&nbsp;&nbsp;&nbsp;insert: before current row;<br>&nbsp;&nbsp;&nbsp;&nbsp;append: at end;<br>&nbsp;&nbsp;&nbsp;&nbsp;addChild: add child task;<br>&nbsp;&nbsp;&nbsp;&nbsp;edit: modify<br> Parameter 2: Task model data<br> <br>Note:<br>When using inline editing, this method is not needed.<br>This interface is suitable when using external task edit modal, to submit data obtained from external edit modal (Parameter 2).|
    |resetGantt |Reset all task row edit status|None|
6. Filter
    |<div style="width:170px">Method</div>| <div style="width:592px">Description</div>
    |----| ----|
    addFilter|Set conditions to filter data. Example:<br>` MZGantt.filterRows((task) =>{return task.duration > 3;},"onlyShow")`
    removefilter|Remove filter, restore data display. Example:<br>`MZGantt.removefilter()`

7. Export Image
    |<div style="width:170px">Method</div>| <div style="width:190px">Description</div>| <div style="width:375px">Parameters</div>
    |----| ----|----|
    exportGanttImg |Save as image| None

8. Event Listeners
    |<div style="width:230px">Event Name</div>| <div style="width:560px">Description</div>|
    |----| ----|
    |cellRender|Cell rendering event.<br>Supports listening to this event, setting custom callback handler. Render cells.<br>Example: ` MZGantt.on("cellRender", function(row, cellObj) {...})`|
    |cellBlur|Cell blur event. Triggered when inline editing cell loses focus. Example:<br>` MZGantt.on("cellBlur", function(row, cellName, oldVal, newVal) {...})`|
    |rowRender|Table row rendering event.<br>Supports listening to this event, setting custom callback handler. Render row styles.<br>Example: ` MZGantt.on("rowRender", function(row, rowStyleObj) {...})`|
    |barRender|Progress bar rendering event.<br>Can dynamically listen to user data, render progress bar styles in real-time.<br>Example: ` MZGantt.on("barRender", function(row, barObj) {...})`|
    |clickBar |Progress bar click event.<br>Example: ` MZGantt.on("clickBar", function(row, eventXY, dom) {...})`|
    |dblClickBar |Progress bar double-click event.<br>Example: ` MZGantt.on("dblClickBar", function(row, barData, dom) {...})`|
    |clickRow |Row click event.<br>Example: ` MZGantt.on("clickRow", function(row, eventXY) {...})`|
    |rightClick/rightClickBar|Progress bar right-click event.<br>Example: ` MZGantt.on("rightClickBar", function(row, eventXY, dom) {...})`|
    |loaded|Loading complete event.<br>Automatically executed after Gantt chart loading completes, can set user's own processing logic as needed.<br>Example: `MZGantt.on("loaded", function(e, data) {...})`|
    |barDragEnd|Progress bar drag end event. Triggered when dragging progress bar is released. Example:<br>` MZGantt.on("barDragEnd", function(row, updRows) {...})`|
    |rowDragEnd|Row drag end event. Triggered when dragging row is released. Example:<br>` MZGantt.on("rowDragEnd", function(row, updRows) {...})`|


## VII. Common Error Codes
<div style="margin-left:30px">

|<div style="width:230px">Error Code</div>| <div style="width:560px">Description</div>|
|----|----|
|0x00010|Before executing createGantt command, Gantt chart container div must be loaded, otherwise this error is shown.|
|0x00020|Error configuring Gantt chart.|
|0x00030|Error rendering Gantt chart (drawGantt command).|
|0x00040|Error rendering milestone marker lines.|
|0x00050|Exception occurred when binding data. Please refer to <IV. Gantt Chart Data Row Data Model> to create Gantt chart row data.|
|0x00060|Exception occurred when batch loading task data.|
|0x00080|Error occurred when clicking cell.|
|0x00090|Error occurred when clicking progress bar.|
|0x00100|Plugin initialization exception.|
</div>

## VIII. Download
1. Standard JS Version
    Download Or use following CDN:
    ```
    <link rel="stylesheet" type="text/css" href="https://gcore.jsdelivr.net/npm/mzgantt@1.0.7/cdn/mzgantt.css" />
    <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.7/cdn/mzgantt.js"></script>
    <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.7/cdn/edit.js"></script>
    <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.7/cdn/export.js"></script>
    <script language="javascript" src="https://gcore.jsdelivr.net/npm/mzgantt@1.0.7/cdn/mobile.js"></script>
    ```

2. npm Version (supports Vue, etc.): Install directly using npm command.
    > npm install mzgantt

## IX. Versions

MZGantt provides 3 versions to meet different user needs.

1. Free Trial Version
    No authorization required, free to use after download (with some feature limitations), does not affect your development.

2. Personal Version
    Need to obtain personal version license key after download, no feature limitations, can freely use all plugin features.

3. Commercial Version
    Need to obtain commercial version license key after download, no feature limitations and no commercial use restrictions, can freely use all plugin features.

## X. Contact

Mobile: +31(0)623010866
Email:
- Service 1: info@ndes-global.com
- Service 2: info@tecjt.com
- Service 3: hubosoft@foxmail.com
