# Ext.js组件学习

## 1.信息提示框组件

```powershell
1. Ext.MessageBox.alert()方法
弹出提示框
2. Ext.MessageBox.confirm()方法
确认还是取消
3. Ext.MessageBox.prompt()方法
输入内容
4. Ext.MessageBox.show()方法
模态框,可以自定义多种格式
```

## 2.面板组件

```
1.最简单的Panel示例。
2.带滚动条的Panel
3.可收缩的Panel
4.包含顶部和底部工具栏的Panel 
5.为按钮添加事件处理
6.可拖动的Panel
7.拖动后，不显示以前位置的虚线框
8.拖动后，停留在拖动位置 
9.拖动时，不显示以前的阴影
10. Panel顶部工具栏 

contentEl  renderTo  applyTo  都是显示内容
```

## 3.工具栏和菜单栏

```
//头部工具栏
			tools:[
				{id:"save",qtip:"保存"},
				{id:"help",qtip:"帮助"},
				{id:"close",qtip:"关闭"}
			],
			//工具栏
			tbar:new Ext.Toolbar({
				width:300,
				height:20
			})

	var mymenu = new Ext.menu.Menu({
			items:[
				{text:"保存"},
				{text:"另存为"}
			]
		});
		
		mypanel.getTopToolbar().add(new Ext.Toolbar.TextItem("工具栏"));
		mypanel.getTopToolbar().add(new Ext.Toolbar.Fill());
		mypanel.getTopToolbar().add(new Ext.Toolbar.Button({text:"保存"}));
		mypanel.getTopToolbar().add(new Ext.Toolbar.Separator());
		mypanel.getTopToolbar().add(new Ext.Toolbar.Button({text:"另存为"}));
		mypanel.getTopToolbar().add(new Ext.Toolbar.Button({text:"文件",menu:mymenu}));
	}

```

## 4.窗口组件Window

```
Ext.onReady(
	function() {
		new Ext.Window({
			width:300,
			height:300,
			title:"window"
		}).show();
	}
);

resizable:false,(是否可以改变大小，默认可以)
maximizable:true,（是否增加最大化，默认没有）
draggable:false,（是否可以拖动，默认可以）
minimizable:true,（是否增加最小化，默认无）

closeAction属性用来设置关闭按钮操作，有两个可选值，一个为close，一个为hide。
如果为close，那么关闭后不能再打开，而hide可以重新打开该窗口。

```

## 5.ExtJS布局

```
Ext.onReady(
	function() {
		new Ext.Panel({
			renderTo:hello,
			width:400,
			height:300,
			title:"我是Panel",
			autoScroll:true,
			collapsible:true,
			items:[
				new Ext.Panel({
					width:200,
					height:100,
					title:"面板一"
				}),
				new Ext.Panel({
					width:200,
					height:100,
					title:"面板二"
				})
			]
		})
	}
);

ContainerLayout容器布局
Ext.layout.ContainerLayout提供了所有布局类的基本功能，它没有可视化的外观，只是提供容器作为布局的基本逻辑，这个类通常被扩展而不通过new关键字直接创建。
如果面板（panel）没有指定任何布局类，则它将会作为默认布局来创建

Fit布局，用于嵌套布局时使之自适应容器大小，通常用于菜单，表单等的嵌套布局。 Fit布局顾名思义，Fit即“自适应”的意思，通常使用在我们进行嵌套布局的时候使用

Accordion布局由类Ext.layout.Accordion定义，名称为accordion，表示可折叠的布局，也就是说使用该布局的容器组件中的子元素是可折叠的形式

Card布局由Ext.layout.CardLayout类定义，名称为card，该布局将会在容器组件中某一时刻使得只显示一个子元素。可以满足安装向导、Tab选项板等应用中面板显示的需求

AnchorLayout是最简单的布局管理器，它只是将元素按照配置的属性在元素容器中进行定位

AbsoluteLayout布局扩展自Ext.layout.AnchorLayout布局，对应面板布局（layout）配置项的名称为absolute。它可以根据子面板中配置的x/y坐标进行定位，并且坐标值支持使用固定值和百分比两种形式

Form布局由类Ext.layout.FormLayout定义，名称为form，是一种专门用于管理表单中输入字段的布局，这种布局主要用于在程序中创建表单字段或表单元素等使用

Column列布局由Ext.layout.ColumnLayout类定义，名称为column。列布局把整个容器组件看成一列，然后往里面放入子元素的时候，可以通过在子元素中指定使用columnWidth或width来指定子元素所占的列宽度。columnWidth表示使用百分比的形式指定列宽度，而width则是使用绝对象素的方式指定列宽度，在实际应用中可以混合使用两种方式

Ext.layout.TableLayout对应面板布局layout配置项的名称为table。这种比较允许你非常容易的渲染内容到HTML表格中，可以指定列数（columns），跨行（rowspan），跨列（colspan），可以创建出复杂的表格布局。必须使用layoutConfig属性来指定属于此布局的配置，table布局仅有唯一的布局配置项columns，而包含在其中的子面板会具有rowspan和colspan两个配置项

Border布局由类Ext.layout.BorderLayout定义，布局名称为border。该布局把容器分成东南西北中五个区域，分别由east，south, west，north, cente来表示，在往容器中添加子元素的时候，我们只需要指定这些子元素所在的位置，Border布局会自动把子元素放到布局指定的位置
```

## 6.ViewPort和TabPanel

```

```

## 7.FormPanel

```
表单Form
```

## 8.TreePanel

```
树菜单
```

## 9.ExtJS常用工具类和函数

```

```

## 10.GridPanel

```

```

## 11.ExtJS与服务器交互

```
Ext.Ajax.request({
	url:"loginServer.jsp",
	params:{
		username:"lifengxing",
		password:"123"
	},
	callback:function(options,success,response){
		if(success){
			alert(response.responseText)
		}
	}
})

一、向服务器发送请求。
1.通过Ext.Ajax.request向服务器发送请求。
2.通过url属性来指定发送请求的地址。
3.通过params属性来设置请求参数。
4.callback属性来指定回调函数。

```



