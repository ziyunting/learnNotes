2月14日
7:30起床
8:20-9:00 英语学习
9:00-9:30 回顾昨天学习的内容
9:30-12:20     绘制扇形图
1:30-6:00  解决bug学习力向导图（目前只能参考别人的实现基本的力向导图，对于很多的api还不太理解）

今天
###扇形图绘制要点记录
1.
``` 
d3.schemeCategory10 
一个d3内置的颜色数组
通过前面的离散比例尺来生成颜色比例
```
2.
``` 
    var pieDate = d3.pie(dataset);  //d3的饼状图生成器,生成饼状图的弧形数据
 
    d3.arc()         //d3中的弧线绘制器
      .innerRadius(innerRadius)  //弧形的内半径
      .outerRadius(outerRadius); //弧形的外半径
```
3.
```
绘制扇形就是利用前面学到的svg的Path属性。利用 d3.arc生成的数据进行弧形生成

```
4.
``` 
添加文字
arc_generator.centroid(d)  此处是每个扇形的中点。
```
###向导图绘制要点记录
1.画布准备
``` 
    var marge = {top: 30, bottom: 30, left: 30, right: 30}
    var svg = d3.select("svg")
    var width = svg.attr("width")
    var height = svg.attr("height")
    var g = svg.append("g")
        .attr("transform", "translate(" + marge.top + "," + marge.left + ")");
```
2.数据准备
主要是准备点集和边集
```
//准备数据
    var nodes = [
        {name: "跑步"},
        {name: "游泳"},
        {name: "足球"},
        {name: "猪"},
        {name: "牛"},
        {name: "张三"},
        {name: "李四"},
        {name: "王五"},
    ];

    var edges = [
        {source: 0, target: 6, relation: "爱好", value: 1.5},
        {source: 6, target: 3, relation: "属相", value: 1},
        {source: 6, target: 5, relation: "兄弟", value: 1},
        {source: 5, target: 1, relation: "爱好", value: 1.5},
        {source: 5, target: 7, relation: "夫妻", value: 0.5},
        {source: 5, target: 4, relation: "属相", value: 1},
        {source: 7, target: 4, relation: "属相", value: 1},
        {source: 7, target: 2, relation: "爱好", value: 1.5},
    ];
```
3.新建力向导图
``` 
    var forceSimulation = d3.forceSimulation()
        .force("link", d3.forceLink())
        .force("charge", d3.forceManyBody())
        .force("center", d3.forceCenter());
    ;
```
4.生成数据
``` 
forceSimulation.nodes(nodes)
    .on("tick", ticked);
//生成边数据
forceSimulation.force("link")
    .links(edges)
    .distance(function (d) {//每一边的长度
        return d.value * 100;
    });
//设置图形的中心位置
forceSimulation.force("center")
    .x(width / 2)
    .y(height / 2);
```
5.绘制边
包含边的基础线条和文字
``` 
    var links = g.append("g")
        .selectAll("line")
        .data(edges)
        .enter()
        .append("line")
        .attr("stroke", function (d, i) {
            return colorScale(i);
        })
        .attr("stroke-width", 1);
    var linksText = g.append("g")
        .selectAll("text")
        .data(edges)
        .enter()
        .append("text")
        .text(function (d) {
            return d.relation;
        })
```
6.绘制节点
``` 
    var gs = g.selectAll(".circleText")
        .data(nodes)
        .enter()
        .append("g")
        .attr("transform", function (d, i) {
            var cirX = d.x;
            var cirY = d.y;
            return "translate(" + cirX + "," + cirY + ")";
        })
        .call(d3.drag()
            .on("start", started)
            .on("drag", dragged)
            .on("end", ended)
        );

    //绘制节点
    gs.append("circle")
        .attr("r", 10)
        .attr("fill", function (d, i) {
            return colorScale(i);
        })
    //文字
    gs.append("text")
        .attr("x", -10)
        .attr("y", -20)
        .attr("dy", 10)
        .text(function (d) {
            return d.name;
        })
```
7.数据函数处理
``` 
    function ticked() {
        links
            .attr("x1", function (d) {
                return d.source.x;
            })
            .attr("y1", function (d) {
                return d.source.y;
            })
            .attr("x2", function (d) {
                return d.target.x;
            })
            .attr("y2", function (d) {
                return d.target.y;
            });

        linksText
            .attr("x", function (d) {
                return (d.source.x + d.target.x) / 2;
            })
            .attr("y", function (d) {
                return (d.source.y + d.target.y) / 2;
            });

        gs
            .attr("transform", function (d) {
                return "translate(" + d.x + "," + d.y + ")";
            });
    }

    function started(d) {
        if (!d3.event.active) {
            forceSimulation.alphaTarget(0.8).restart();
        }
        d.fx = d.x;
        d.fy = d.y;
    }

    function dragged(d) {
        d.fx = d3.event.x;
        d.fy = d3.event.y;
    }

    function ended(d) {
        if (!d3.event.active) {
            forceSimulation.alphaTarget(0);
        }
        d.fx = null;
        d.fy = null;
    }
```
