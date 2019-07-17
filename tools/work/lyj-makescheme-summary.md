### makeScheme
> 功能：
- 封装：整体分为三个部分，分别作为 MakeScheme.js、EyQueryPanel.js、EyTablePanel.js 三个功能容器，其中两部分使用 EyQueryPanel 以及 EyTablePanel，只有头部需要单独处理。
- 地图操作：头部需要手动添加必经点，必经线,必经区。并可以框选删除。quertTable也可以进行点线面的勾选，并显示在地图上。不用框选删除。并且因方案类别不同，图层需要重置。不同面板之前图层不互相影响。
- 使用 ```$(function(){ ... }) ```进行“模块初始化”

#### $.ecity.Class 封装写法

- $.ecity 封装

    ```
    var MakeScheme = $.ecity.Class({

        //此处定义类的私有属性

        pointLayer;
        polylineLayer;
        polygonLayer;
        mapWnd;
        paramA;
        paramB;

        //完成初始化基本参数的赋值

        initialize: function(options) {
            $.ecity.extend(this, options);
            //DOM初始化
            this.createHtml();
            //DOM方法初始化
            this.bindEvent();
            //图层初始化
            this.initlayer();
            //完成基本参数的赋值
            this.mapWnd = options.mapWnd;
            this.patrolSvr = options.patrolSvr;
            //此处定义封装的变量的值，值可以是从外面传进来，避免全局变量滥用
            this.tableServer = options.tableServer;
            //手动画必经点
            this.pointNum = 0;
            //表格必经点
            this.tablePointNum = 0;
            //表格管长
            this.tablePipeLength = 0;
            //必经点总数
            this._count = 0;
            //选择类型

            //其他参数省略
            ....
        },
        //类的私有方法
        createHtml:function(){......},
        //方法中调用方法
        selectOnChange:function(){
            var that = this;
            that.startQuery();
        },
        startQuery:function(){},
        //将dom事件从dom上解绑
        bindEvent:function(){
            var that = this;
            that.$targetContainer.find('#equipType').on('change', function() {
            that.tabindex = $('#equipType option:selected').attr('dataIndex');
            that.selectOnChange(equipCfg);
            });
        },
        initlayer: function() {...},
        clearLayer: function() {...},
        destroyLayer: function() {...},
        //计算必经点总和
        getPointsSum: function() {...},
        //计算必经线总和
        addPolyline: function() {...},
        //用于手动增加必经点数目
        addPoint: function() {...},
        //用于手动增加必经线数目
        addPolyline: function() {...},
        //用于手动增加必经面数目
        addPolygon: function() {...},
        //画点
        drawPoint: function() {...},
        //画线
        drawLine: function() {...},
        //画面
        drawPolygon: function() {...},
        //获取线的长度
        getLength: function() {...},
        //删除指定的graphic
        onDeleGra: function() {...},
        //移除点
        removePoint: function() {...},
        //移除线
        redrawPolylines: function() {...},
        //移除面
        redrawPolygons: function() {...},
        ...
        //其他业务功能
        })    
    ```

- 封装的传参

    ```
        makeScheme = new MakeScheme({

            //参数都存放在option里。

            $targetContainer: $makeScheme,
            eContext: eContext,
            type: type,
            cfg: schemeCfg,
            queryPanelFields: queryPanelFields,
            metaIns: metaIns,
            tableSvrUrl: tableSvrUrl,
            mapWnd: mapWnd,
            patrolSvr: patrolSvr,
            tableServer: tableServer,
            //handleTypechange是onTypeChange的回调函数。用于处理外部的接口
            onTypeChange: function(res, itemType) {
                orderby = null;
                if (res) {
                handleTypeChange(res, itemType);
                }
            },
        });

    ```

- 封装方法的联动
    ***实现 makeScheme 中方案改变，eyQueryPanel 进行联动***
    ```
        index.html

        //handleTypechange是onTypeChange的回调函数。用于处理外部的接口
        

        onTypeChange: function(res, itemType) {
            orderby = null;
            if (res) {
            handleTypeChange(res, itemType);
            }
        },
    ```
    ***注册方法onTypeChange,并handleTypeChange回调函数处理回调的数据，渲染eyQueryPanel。实现组件内部与外部的通讯。***

    - eyTablePanel（实例对象）进行勾选时，改变 makeScheme 当中的必经点
    ```
        index.html

        function onCheck(resSelect) {
          if(resSelect.rowData.geo.type == 'point'){
            makeScheme.addTablePoint(resSelect.rowData.geo,resSelect.rowData.gid,queryPanelComp.getParams().layerIds[0]);
          }else{
            var pipeRangeLength = resSelect.rowData.管长;
            makeScheme.addTablePolylineNumber(resSelect.rowData.geo,resSelect.rowData.gid,pipeRangeLength,queryPanelComp.getParams().layerIds[0]);
          }
          
        }

    ```

    - ***makeScheme（实例对象）调用私有方法```makeScheme.addTablePoint()```,实现外部与组件内部通讯。***

    ```
    //用于增加必经点数目，用于 EyTablePanel 在勾选时候调用
    
    addTablePoint: function(geo,gid,layId) {
        var that = this;

        var typePointUrl = that.typePointUrl('point');
        var graphic = new that.mapWnd.esri.Graphic(geo, that.sysmbolPoint, {
        type: queryPanelComp.getParams().layerInfos[0] ? queryPanelComp.getParams().layerInfos[0].dname : '核查',
        img: 'mark4',
        buffer:'80.0',
        equipid: gid,
        equiporigin:typePointUrl.url+'/'+layId,
        equipparam:typePointUrl.equipparam,
        gid:gid
        });
        that.tablePointLayer.add(graphic);
        that.tablePointNum = that.tablePointLayer.graphics.length;
        that.mapWnd.map.centerAt(geo);
        that.getPointsSum();
    },
    ```


- 封装将DOM与DOM操作分离
    ```
        //将dom事件从dom上解绑
        bindEvent:function(){
            var that = this;
            that.$targetContainer.find('#equipType').on('change', function() {
            that.tabindex = $('#equipType option:selected').attr('dataIndex');
            that.selectOnChange(equipCfg);
            });
        },

    ```

- 闭包也可以进行模块封装（避免全局污染）

    *** 参考资料：javascript高级程序设计（第4章<变量作用域和内存问题>，第6章<面向对象的程序设计>，第7章<函数表达式>）***
    
    ```

   
        //将匿名函数的立即执行函数结果赋值给数组，数组中每个函数都有自己num变量的一个副本
        //函数的参数是按值传递的，变量i的当前值赋值给参数Num
        function createFunctions(){
            var result = new Array;
            for(var i=0;i<10;i++){
                result[i] = function(num){
                    return function(){
                        return num;
                    }
                }(i)
            }
            return result;
        }    
        createFunctions();//1,2...10
        //每个函数作用域链中都保存着createFunctions的活动对象，所以它们引用的都是同一个i，
        //当createFunctions函数返回时，变量i的值是10，此时每个函数都引用保存变量i的同一个变量对象
        function createFunction(){
            var result = new Array;
            for(var i=0;i<10;i++){
                result[i] = function(){
                    return i;
                }
            }
        }
        createFunction();//10,10..10


    ```

#### 地图

    ```
    地图操作：
    1.头部需要手动添加必经点，必经线。quertTable也可以进行点线面的勾选，并显示在地图上。并且手动画的点线面和queryTable显示的点线面并不影响。
    2.多个面板的点线面不互相影响。
    3.并且因方案类别不同，图层需要重置。

    1. 为了要保证功能内的点线面独立性，需要使用三个图层（layerId）
    2. 为了保证面板多次装载，互相不受影响，layerId 之间需要唯一
    3. 内部统一使用 pointLayerId，polylineLayerId，polygonLayerId 进行图层相关操作
    ```
<!-- 1. 地图概念

    点画在grafic上,点图层添加grafic,map添加点图层。
    线画在grafic上,线图层添加grafic,map添加线图层。
    面画在grafic上,面图层添加grafic,map添加面图层。 -->

- 地图初始化和销毁

    ***初始化一个面板上的所有图层，包括手动画的点线面和框选的图层，以及queryTable上勾选的点线面和框选的图层。一旦面板destroy，就将所有图层移除。***

    ***初始化图层id，给相应的图层添加增删方法，将所有图层(layer)添加到地图上。***

    ```


    initlayer: function() {
        var that = this;
        //初始化图层（layerId）
        var pointLayerId = '';
        var polylineLayerId = '';
        var polygonLayerId = '';
        var tablePointLayerId = '';
        var tablePolylineLayerId = '';
        var polylinePointLayerId = '';
        var patrolTempId = '';
        var sole = new Date().getMilliseconds();
        var pointLayerId = 'point-layer-' + sole;
        var polylineLayerId = 'polyline-layer-' + sole;
        var polylinePointLayerId = 'polyline-point-layer-' + sole;
        var polygonLayerId = 'polygon-layer-' + sole;
        var tablePointLayerId = 'table-point-layer-' + sole;
        var tablePolylineLayerId = 'poly-line-layer-' + sole;
        var patrolTempId = 'patrol-temp-layer-' + sole;

        //初始化点图层，并添加增删方法
        that.pointLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: pointLayerId,
        });
        that.pointLayer.on('graphic-add', function(gra) {
        
        });
        that.pointLayer.on('graphic-remove', function(gra) {
        
        });
        //初始化线图层，并添加增删方法
        that.lineLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: polylineLayerId,
        });
        that.lineLayer.on('graphic-add', function(gra) {
        that.getLength();
        });
        that.lineLayer.on('graphic-remove', function(gra) {
        that.getLength();
        });
        //初始化表格点图层
        that.tablePointLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: tablePointLayerId,
        });
        //初始化表格线图层
        that.tablePolylineLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: tablePolylineLayerId,
        });
        that.tablePolypointlineLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: tablePolylineLayerId,
        });
        
        that.linePointLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: polylinePointLayerId,
        });

        //初始化面图层
        that.polygonLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        // 'id': 'patrolPolygon'
        id: polygonLayerId,
        });
        //初始化框选图层
        that.tempLayer = new this.mapWnd.esri.layers.GraphicsLayer({
        id: patrolTempId,
        });
        that.mapWnd.map.addLayer(that.tempLayer, 1);
        // that.mapWnd.map.addLayer(that.roundLayer, 2);
        that.mapWnd.map.addLayer(that.polygonLayer, 3);
        that.mapWnd.map.addLayer(that.lineLayer, 4);
        that.mapWnd.map.addLayer(that.linePointLayer, 5);
        that.mapWnd.map.addLayer(that.pointLayer, 6);
        that.mapWnd.map.addLayer(that.tablePointLayer, 7);
        that.mapWnd.map.addLayer(that.tablePolylineLayer, 8);
        that.mapWnd.map.addLayer(that.tablePolypointlineLayer, 2);
    },

    ```
    ***销毁地图上面板上的所有图层。***
```
destroyLayer: function() {
    if (this.pointLayer) {
      this.mapWnd.map.removeLayer(this.pointLayer);
    }
    if (this.lineLayer) {
      this.mapWnd.map.removeLayer(this.lineLayer);
    }
    if (this.linePointLayer) {
      this.mapWnd.map.removeLayer(this.linePointLayer);
    }
    if (this.polygonLayer) {
      this.mapWnd.map.removeLayer(this.polygonLayer);
    }
    if (this.tablePointLayer) {
      this.mapWnd.map.removeLayer(this.tablePointLayer);
    }
    if (this.tablePolylineLayer) {
      this.mapWnd.map.removeLayer(this.tablePolylineLayer);
    }
    if (this.tablePolypointlineLayer) {
      this.mapWnd.map.removeLayer(this.tablePolypointlineLayer);
    }
},

```

- 地图画点线面

    - 画点
    ***戳点***
    ```
        drawPoint: function() {
            var that = this;
            that.mapWnd.drawToolbar.activate(
            that.mapWnd.esri.toolbars.Draw.POINT,
            function(geo) {
                // mapWnd.drawToolbar.deactivate();
                that.addPoint(geo);
            }
            );
        },

    ```
    ***创建点的图形，并定义属性***
    ```
       // 用于手动增加必经点数目
        addPoint: function(geo) {
            var that = this;
            var graphic = new that.mapWnd.esri.Graphic(geo, that.sysmbolPoint, {
            type: 0,
            contents: [],
            img: 'mark4',
            buffer: 80

            });
            //计算点的总数和点图层添加点
            that.pointLayer.add(graphic);
            that.pointNum = that.pointLayer.graphics.length;
            that.getPointsSum();
        },
    ```
    - 画线
    ***戳线***
    ```
        drawLine: function() {
            var that = this;
            that.mapWnd.drawToolbar.activate(
            that.mapWnd.esri.toolbars.Draw.POLYLINE,
            function(geo) {
                that.mapWnd.drawToolbar.deactivate();
                that.addPolyline(geo);
                // that.drawLine(geo);
            }
            );
        },
    ```
    ***创建线的图形，并定义属性***
    ```
        addPolyline: function(geo) {
            var that = this;
            var _mlen = 0;
            that.mapWnd.require(
            ['esri/geometry/geometryEngine', 'esri/units'],
            function(geometryEngine, units) {
                _mlen = geometryEngine.planarLength(geo, units.KILOMETERS);
                // var _mlen = parseFloat(
                //   geometryEngine.planarLength(gra.geometry, units.KILOMETERS)
                // ).toFixed(2);
                var graphic = new mapWnd.esri.Graphic(geo, that.sysmbolPolyline, {
                contents: [],
                linelen: _mlen,
                buffer: 80
                });
                
                that.lineLayer.add(graphic);
                that.getLength();
            
                var passPoint = geo.paths[0];
                for (var i = 0; i < passPoint.length; i++) {
                var point = new that.mapWnd.esri.geometry.Point({
                    x: passPoint[i][0],
                    y: passPoint[i][1],
                    spatialReference: mapWnd.map.spatialReference,
                });
                var pointSym = new that.eContext.$.ecity.mapWnd.esri.symbol.PictureMarkerSymbol(
                    'images/point.png',
                    10,
                    10
                );
                var graphic = new that.mapWnd.esri.Graphic(point, pointSym, {
                    type: 0,
                    contents: [],
                    img: 'mark4',
                });
                that.linePointLayer.add(graphic);
                }
            }
            );
        
            // setDisabled();
        },
    ```
    ***戳面***
    ```
        drawPolygon: function() {
            var that = this;
            that.mapWnd.drawToolbar.activate(
            that.mapWnd.esri.toolbars.Draw.POLYGON,
            function(geo) {
                that.mapWnd.drawToolbar.deactivate();
                that.addPolygon(geo);
            }
            );
        },
    ```
    ***创建面的图形，并定义属性***
    ```
        addPolygon: function(geo) {
            var that = this;
            // var sysmbol = layer.id == 'patrolRound' ? sRound : sPolygon;
            var graphic = new that.mapWnd.esri.Graphic(geo, this.sPolygon, {
            contents: [],
            });
            that.polygonLayer.add(graphic);
            that.setDisabled();
        },
    ```
    ***框选删除点线面***
    ***判断是否删除，删除则重绘***
    ***重绘判断是否在图层内，不在则放到temp图层，则重新绘制temp图层***
    ```
        removePoint: function() {
            var that = this;
            that.mapWnd.drawToolbar.activate(
            that.mapWnd.esri.toolbars.Draw.POLYGON,
            function(polygon) {
                that.mapWnd.drawToolbar.deactivate();
                var graphic = new that.mapWnd.esri.Graphic(polygon, that.sPolygon);
                that.tempLayer.add(graphic);
                that.tempLayer.refresh();
                eContext.$.ecity.dialog.confirm(
                '确定删除？',
                function(dlg) {
                    // var delData=[];
                    that.redrawPoints(polygon);
                    if(polygon.type == 'polygon'){
                    that.redrawPolygons(polygon);
                    }
                    //线回调函数内部执行重绘操作，重绘操作完成后，执行Templayer的清除，并不影响点面的重绘以及回调。
                    that.redrawPolylines(polygon, function() {
                    dlg.close();
                    that.tempLayer.clear();
                    });
                    that.redrawPolygons(polygon, function() {
                    dlg.close();
                    that.tempLayer.clear();
                    });
                },
                function(dlg) {
                    that.tempLayer.clear();
                    that.tempLayer.refresh();
                    dlg.close();
                }
                );
            }
            );
        },
    ```
    ***重新绘制线***
    redrawPolylines()

    ```
        redrawPolylines: function(polygon, callback) {
            var that = this;
            var geometrys = [];
            that.mapWnd.require(['esri/geometry/geometryEngine'], function(
            geometryEngine
            ) {
            $(that.lineLayer.graphics).each(function(key, gra) {
                if (gra.geometry) {
                if (!geometryEngine.intersects(gra.geometry, polygon)) {
                    // that.pointNum--;
                    // that.getPointsSum();
                    geometrys.push(gra);
                }
                }
            });
            that.lineLayer.clear();
            that.linePointLayer.clear();

            for (var i = 0; i < geometrys.length; i++) {
                that.lineLayer.add(geometrys[i]);
            }
            that.lineLayer.refresh();
            that.getLength();

            geometrys.forEach(function(gra) {
                var passPoint = gra.geometry.paths[0];
                for (var i = 0; i < passPoint.length; i++) {
                var point = new that.mapWnd.esri.geometry.Point({
                    x: passPoint[i][0],
                    y: passPoint[i][1],
                    spatialReference: mapWnd.map.spatialReference,
                });
                var pointSym = new that.eContext.$.ecity.mapWnd.esri.symbol.PictureMarkerSymbol(
                    'images/point.png',
                    10,
                    10
                );
                var graphic = new that.mapWnd.esri.Graphic(point, pointSym, {
                    type: 0,
                    contents: [],
                    img: 'mark4',
                });
                that.linePointLayer.add(graphic);
                }
            });

            callback && callback();
            });
        },
    ```


- queryTable 点的勾选以及删除

    ***地图上创建点时候，给grafic gid属性,再把grafig添加到图层上。移除时判断勾选的点gid是否等于之前创建点时候图层赋予的gid，相等则移除点图层***

    ```
        addTablePoint: function(geo,gid,layId) {
            var that = this;

            var typePointUrl = that.typePointUrl('point');
            var graphic = new that.mapWnd.esri.Graphic(geo, that.sysmbolPoint, {
            type: queryPanelComp.getParams().layerInfos[0] ? queryPanelComp.getParams().layerInfos[0].dname : '核查',
            img: 'mark4',
            buffer:'80.0',
            equipid: gid,
            equiporigin:typePointUrl.url+'/'+layId,
            equipparam:typePointUrl.equipparam,
            gid:gid
            });
            that.tablePointLayer.add(graphic);
            that.tablePointNum = that.tablePointLayer.graphics.length;
            that.mapWnd.map.centerAt(geo);
            that.getPointsSum();
        },
    ```

    ```
        removeTablePoint: function(geo,gid) {
            var that = this;
            if(that.tablePointLayer.graphics){
            $(that.tablePointLayer.graphics).each(function(key,gra){
                if(gra.attributes.gid == gid){
                that.tablePointLayer.remove(gra);
                return false;
                }
            })
            }
            that.tablePointNum = that.tablePointLayer.graphics.length;
            that.getPointsSum();
        },
    ```
- queryTable 线的勾选以及删除以及长度计算
    ***利用图层的删减计算长度，图层增加一条线，遍历图层计算线的长度。图层减少一条线，遍历图层，计算线的长度。每次重新遍历，不用减法计算。全选操作也是如此。***
    ```
        addTablePolylineNumber: function(geo,gid,pipeLenth,layId) {
            var that = this;
            var typePointUrl = that.typePointUrl('line');
            var centerpoint = mapWnd.getCenterOfPolyline(geo.paths);
            
            var graphic = new mapWnd.esri.Graphic(geo, this.sysmbolPolyline, {
            equipid: gid,
            linelen: pipeLenth,
            buffer:'80.0',
            equiporigin:typePointUrl.url+'/'+layId,
            equipparam:typePointUrl.equipparam,
            type: queryPanelComp.getParams().layerInfos[0].dname,
            gid:gid
            });
            
            // 添加线，计算线的长度
            that.tablePolylineLayer.add(graphic);
            var sum = 0;
            for(var j=0;j<that.tablePolylineLayer.graphics.length;j++){
            sum += parseFloat(that.tablePolylineLayer.graphics[j].attributes.linelen);
            }
            that.tablePipeLength = sum;
            that.getLength();

            var passPoint = geo.paths[0];
            for (var i = 0; i < passPoint.length; i++) {
            var point = new that.mapWnd.esri.geometry.Point({
                x: passPoint[i][0],
                y: passPoint[i][1],
                spatialReference: mapWnd.map.spatialReference,
            });
            var pointSym = new that.eContext.$.ecity.mapWnd.esri.symbol.PictureMarkerSymbol(
                'images/point.png',
                10,
                10
            );
            var graphic = new that.mapWnd.esri.Graphic(point, pointSym, {
                type: 0,
                contents: [],
                img: 'mark4',
            });
            that.tablePolypointlineLayer.add(graphic);
            }
            // that.mapWnd.map.centerAt(centerpoint);
            that.mapWnd.map.centerAndZoom(centerpoint,8);
            // that.getLength();
        },
    ```

    ```
        removeTablePolylineNumber: function(geo,gid,pipeLenth){
            var that = this;
            if(that.tablePolylineLayer.graphics){
            $(that.tablePolylineLayer.graphics).each(function(key,gra){
                if(gra.attributes.gid == gid){
                that.tablePolylineLayer.remove(gra);
                return false;
                }
            })
            }
            var sum = 0;
            for(var j=0;j<that.tablePolylineLayer.graphics.length;j++){
            sum += parseFloat(that.tablePolylineLayer.graphics[j].attributes.linelen);
            }
            that.tablePipeLength = sum;
            that.getLength();
        },
    ```

#### 开发中的修正

1. 变量声明（声明，命名规范，命名格式）

    - [语雀](https://www.yuque.com/cabin/func/mgonz2#aggIg)
    - [javascipt高级程序设计-第四章：变量，作用域与内存问题。](https://www.yuque.com/cabin/func/mgonz2#aggIg)
    - [语雀培训规范](https://www.yuque.com/cabin/train/lv5kxx#68mndb)
2. 重复逻辑
    - 添加单个点线与全选添加点线，是分两个方法写的。其实可以放在一个方法里面。
3. 全局变量
    - 使用var声明的变量会自动被添加到最接近的环境。如果初始化变量没有声明，则变量会被自动添加到全局环境。

4. 方法问题：写代码是最后一步，先思考好，再写代码，调试太浪费时间。


#### 学习资料参考
> 资料合集
- [ 每个 JavaScript 工程师都应懂的33个概念 ](https://github.com/stephentian/33-js-concepts?tdsourcetag=s_pctim_aiomsg)
> (构造函数法,原型方式,构造函数+原型方式等五种)
- [JavaScript类的写法](https://segmentfault.com/a/1190000000725051)
> javascript高级编程
- [浅谈 javascript 回调函数](https://wiki.jikexueyuan.com/project/brief-talk-js/callback-function.html)
- [JavaScript 中的回调](https://www.w3cplus.com/javascript/callbacks-in-javascript.html)
- [es6 javascript的class的静态方法、属性和实例属性](https://blog.csdn.net/qq_30100043/article/details/53542966)
- [static](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/static)
- 低耦合高内聚，makescheme中注册的onTypeChange方法中，queryPanel初始化。
queryPanel 中 onClick事件，触发tablePanel初始化。

- $.ecity.class中，封装的共有方法，是对外的接口。共有方法变了，外部的接口也都一起改变。

- 面板加载时，从上至下渲染,延迟问题。

<!-- - [class 封装 闭包]() -->
<!-- - [uuid]() -->
<!-- - [作用域,变量]() -->
- [地图官网](https://developers.arcgis.com/javascript/3/jshelp/intro_firstmap_amd.html)

