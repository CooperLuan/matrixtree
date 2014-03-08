Title: DataMaps 插件
Date: 2014-03-08 20:28:00
Category: js
Tags: js,datamaps
Author: Cooper
Summary: DataMaps 插件

>在做一个需要用到 JS 世界地图插件的应用时，发现了 [DataMaps](http://datamaps.github.io/)，使用比 `jVectorMap`简单，地图基于 `D3.js`，目前的版本为 `0.2.2`

## 基本使用

html 代码

    :::html
    <script type="text/javascript" src="http://cdn.staticfile.org/d3/3.4.2/d3.min.js"></script>
    <script type="text/javascript" src="http://datamaps.github.io/scripts/datamaps.world.min.js"></script>
    <script type="text/javascript" src="http://d3js.org/topojson.v1.min.js"></script>
    <div id="map" style="height: 600px; width: 800px;"></div>

js 代码

    :::js
    $(function() {
        var map = new Datamap({
            element: document.getElementById("map")
        });
    })

<script type="text/javascript" src="http://cdn.staticfile.org/jquery/2.1.0/jquery.min.js"></script>
<script type="text/javascript" src="http://cdn.staticfile.org/d3/3.4.2/d3.min.js"></script>
<script type="text/javascript" src="http://datamaps.github.io/scripts/datamaps.world.min.js"></script>
<script type="text/javascript" src="http://d3js.org/topojson.v1.min.js"></script>
<div id="map"></div>
<script type="text/javascript">
$(function() {
    var map = new Datamap({
        element: document.getElementById("map"),
        fills: {
            defaultFill: "#D0D8E3",
            top: '#FF88A0',
        },
        data: {
            CHN: { fillKey: "top" },
            USA: { fillKey: "top" },
        }
    });
})
</script>

如果需要给某个区域着色，`DataMaps` 只提供了 `map.labels()` 接口, 显示区域的 id 即三位国家代号(CHN/USA)，默认显示全国国家的 `labels`

## 标签插件

需求

1. 只显示指定国家的标签
2. 标签内容可自定义

步骤

1. 查看源码中 lables 相关的代码

        ::::js
        this.addPlugin('bubbles', handleBubbles);
        this.addPlugin('legend', addLegend);
        this.addPlugin('arc', handleArcs);
        this.addPlugin('labels', handleLabels);

    DataMaps 的集中特性都在其中，demo 都是通过 `map.bubbles()/map.legend()` 方式调用的

    `addPlugin` 添加插件后即可将插件名作为函数调用

    自定义标签时修改 `handleLabels` 方法即可

2. 修改 `handleLabels` 方法

        :::js
        // 国家代码 -- 标签
        state_codes = {
            'CHN': '中国',
            'USA': '美国',
        }

        // 原代码
        this.svg.selectAll(".datamaps-subunit")
        .attr("data-foo", function(d) {
            // 指定国家才显示
            if(['CHN', 'USA'].indexOf(d.id) == -1) {
                return 'bar';
            }
            // 原代码
            // ****
            // 原代码

            // 显示为 state_codes 标签
            layer.append("text")
                .attr("x", x)
                .attr("y", y)
                .style("font-size", (options.fontSize || 10) + 'px')
                .style("font-family", options.fontFamily || "Verdana")
                .style("fill", options.labelColor || "#000")
                .text( state_codes[d.id] );
        });

3. 添加插件

    :::js
    map.addPlugin('customLabels', handleLabels);
    map.customLabels();

## 效果

<div id="map2"></div>
<script type="text/javascript">
$(function() {
    var map = new Datamap({
        element: document.getElementById("map2"),
        fills: {
            defaultFill: "#D0D8E3",
            top: '#FF88A0',
        },
        data: {
            CHN: { fillKey: "top" },
            USA: { fillKey: "top" },
        }
    });

    var state_codes = {
        'CHN': '中国',
        'USA': '美国',
    }

    /** 自定义标签
     * 重写 datamaps.js 中的 handleLabels
     */
    function handleLabels ( layer, options ) {
        var self = map;
        options = options || {};
        var labelStartCoodinates = this.projection([-67.707617, 42.722131]);
        this.svg.selectAll(".datamaps-subunit")
        .attr("data-foo", function(d) {
            console.log(d);
            if(['USA', 'CHN'].indexOf(d.id) == -1) {
                return 'bar';
            }
            var center = self.path.centroid(d);
            var xOffset = 7.5, yOffset = 5;

            if ( ["FL", "KY", "MI"].indexOf(d.id) > -1 ) xOffset = -2.5;
            if ( d.id === "NY" ) xOffset = -1;
            if ( d.id === "MI" ) yOffset = 18;
            if ( d.id === "LA" ) xOffset = 13;

            var x,y;

            x = center[0] - xOffset;
            y = center[1] + yOffset;

            var smallStateIndex = ["VT", "NH", "MA", "RI", "CT", "NJ", "DE", "MD", "DC"].indexOf(d.id);
            if ( smallStateIndex > -1) {
            var yStart = labelStartCoodinates[1];
            x = labelStartCoodinates[0];
            y = yStart + (smallStateIndex * (2+ (options.fontSize || 12)));
            layer.append("line")
                .attr("x1", x - 3)
                .attr("y1", y - 5)
                .attr("x2", center[0])
                .attr("y2", center[1])
                .style("stroke", options.labelColor || "#000")
                .style("stroke-width", options.lineWidth || 1)
            }

            layer.append("text")
                .attr("x", x)
                .attr("y", y)
                .style("font-size", (options.fontSize || 10) + 'px')
                .style("font-family", options.fontFamily || "Verdana")
                .style("fill", options.labelColor || "#000")
                .text( state_codes[d.id] );
            return "bar";
        });
    }
    map.addPlugin('customLabels', handleLabels);
    map.customLabels();
})
</script>
