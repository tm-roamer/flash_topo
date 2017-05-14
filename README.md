# flash_topo
flash版的拓扑图(topo)展示工具

## 来龙去脉
本拓扑图插件使用cytoscapeweb制作,并扩展了cytoscapeweb的源码包.

    开源项目地址: http://cytoscapeweb.cytoscape.org/
    细节: flex版本3.6A
    使用技术: flare.swc,pureMVC.swc
    API:官网有详细的示例和API使用介绍
    
## flash下载本地无法查看的问题

    由于flash有沙箱机制,本地观看flash插件,需要简单设置flash可执行路径
    当网页打开,右击flash区域,点击[全局设置...]
    然后参考下面的图片,将本目录存放的盘符填写上,flash就可以正常执行了
    
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/readme1.png "示例图片")
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/readme2.png "示例图片")

## 目录说明

    api:   存放官网汉化的API和具体使用教程(使用zdoc制作,整理起来还是蛮累人的)
    demo:  存放了具体demo演示示例.
    doc:   readme的说明图片
    src:   里面的src.rar是标准的Flash Builder工程,解压即可使用
  
## 示例图片演示
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/ui.jpg "示例图片")
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/project.jpg "示例图片")
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/project2.jpg "示例图片")
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/project4.jpg "示例图片")
![github](https://raw.githubusercontent.com/tm-roamer/flash_topo/master/doc/api.jpg "示例图片")

## 定制部分
   
    (1)节点的闪烁效果
    
    { name: "flicker", type: "boolean",defValue:false},	//控制是否发光
    
    (2)补充的ConsoleBox控制台
    
    /**
	 * 配置控制台参数---注意:本方法名不可以修改
	 *　本函数独立区分于ctyoscapeweb的配置方式,降低耦合,本方法由控制台(自定义组件)直接调用.
	 */
	function  _initConsoleBoxParam(){
		//参数对象----注意:所有字段不可以修改,不支持自定义字段
		var setParameters = {
			//控制跟随的背景图片,有缺陷,节点需要x,y偏移才能使用.
			backgroundFollowImg:{
				//xOffset:-20,		//程序定位大概的坐标后,在通过此变量精确调整x坐标
				//yOffset:15,		//程序定位大概的坐标后,在通过此变量精确调整y坐标
				isShow:true,	//只有配置了才显示,默认为false,
				timer:1000,	 	//用于背景图片的延时加载,单位毫秒,默认1000毫秒(1秒),
				//followNode:"A01",	//Management这里用节点的Label或者Id来进行控制,(这个节点选用左上角节点作为背景图的对应点)
				url:"img/bg.jpg"
			},
			panConsoleBoxVisible:true,			//控制台是否可见 默认为false,
			panBoxIsHelpVisible:true,			//面板的帮助图标按钮是否显示,
			panAddEdgeBoxVisible:true,			//添加连线浮动面板,和属性面板[显示文字标签]是否可见,默认为false,
			panConsoleBoxShowHide:"show",		//控制台初始化的时候是展开还是收起,支持字段"hide","show",默认"show"------已实现			
			panConsoleBoxAlign:"right",			//控制台的对齐方式,目前只支持,左对齐left,右对齐right
			panConsoleBoxBackgroundColor:'0xC1C1C1',//控制台背景色以下二个颜色最好同步,默认颜色3e3e3e
			panConsoleBoxBorderColor:'0xC1C1C1',//控制台背景边框颜色
			panConsoleBoxFontColor:'0x252525',	//控制台文字颜色
			dragOutVBoxRowNum:'3',				//拖拽面板的默认显示行数,默认显示4行,最大值为6行
			initPanArray:[						//这个数组是控制面板的显示和收起.
				{name:"dragOutVBox",			//这个字段面板的名称,目前只支持三个面板,拖拽面板:dragOutVBox,布局:layoutOutVBox,属性:attrOutVBox,不可以修改.
				 label:"拖拽面板",				//面板标题显示文字
				 visible:true,					//面板是否显示,默认true,
				 show_hide:"show"},				//面板的是否展开收起,只支持2个字段,展开"show",收起"hide",默认"show"
				{name:"layoutOutVBox",label:"布局面板",visible:true,show_hide:"show"},
				{name:"attrOutVBox",label:"属性面板",visible:true,show_hide:"show"}
			],
			dragImgArray:[	//配置拖拽面板的图标数组,目前只支持4个字段,名称attrValue,内置图标preset,自定义图标url,value级别高于preset
				{attrValue:"node1", toolTip:"伤不起1",	preset:"node1", value:"img/node1.png"},
				{attrValue:"node2", toolTip:"伤不起2",	preset:"node2", value:"img/node2.png"},
				{attrValue:"node3", toolTip:"伤不起3",	preset:"node3", value:"img/node3.png"},
				{attrValue:"node4", toolTip:"伤不起4",	preset:"node4", value:"img/node4.png"},
				{attrValue:"node5", toolTip:"伤不起5",	preset:"node5", value:"img/node5.png"},
				{attrValue:"node6", toolTip:"伤不起1",	preset:"node6", value:"img/node6.png"},
				{attrValue:"node7", toolTip:"伤不起2",	preset:"node7", value:"img/node7.png"},
				{attrValue:"node8", toolTip:"伤不起3",	preset:"node8", value:"img/node8.png"},
				{attrValue:"node9", toolTip:"伤不起4",	preset:"node9", value:"img/node9.png"}
			]
		}
		return setParameters;
	}
			
	(3)添加节点和添加连线的回调
    
    /**
	 * 添加拖拽节点的回调和调用方法
	 * 参数: type的意思是:"node"为新添加的节点,
	 *		      "edge"为新添加的连线,
	 * 参数: data的意思是:为节点的情况data保存的信息是(以下字段不可以更改)
	 *				x,(x坐标)
	 *				y,(y坐标)
	 *				imgtype(图片类型)
	 * 		      为连线的情况data保存的信息是(以下字段不可以更改)
	 *				id:连线id
	 *				source: 开始节点id
	 *				target: 结束节点id
	 *				directed: 是否有方向,默认为true
	 *				width:	  连线的宽度1-10,默认5px
	 *				label: 	  连线的标签
	 *				lineStyle: 连线的样式有4中,实心,圆点,短矩形,长矩形
	 *				sourceArrowShape:开始箭头样式
	 *				targetArrowShape:结束箭头样式
	 *                  		color: 	  线条颜色
	 *                  		sourceArrowColor: 开始箭头颜色
	 *				targetArrowColor: 结束箭头颜色
	 */
	function _configAddElementsData(type,data){
		if( type && type === "node" ){
		  var node = {  shape:"RECTANGLE",	
				label: 	data.topoType,
				flicker:false,			
				imgtype:data.imgtype||data.topoType
		  }
		  var node1 = vis.addNode(data.x, data.y, node, true);
		}else if( type && type === "edge"){
			var edge = { id: data.id,
				source: data.source,
				target: data.target,
				directed:false,
				network:15,
				width:2,
				label:"a08 (15) a08", 
				lineStyle:"SOLID"
			};
			var edge = vis.addEdge(edge, true);
		}
		return null;	//无需更改
	}
