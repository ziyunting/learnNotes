2月12日
7:30起床
8:20-9:30 了解github page并配置了简单的可供查看的 展示页（还待优化）
9:30-10:30 复习所学的canvas和svg基础知识
10:30-11：40 大致浏览d3，对d3有了一个简单的了解
1:30-3:30 d3学习
3:30-5:00  配置远程连接环境
5:00-6:00 代码梳理
8:00-9:30 笔记回顾
今天的进度比较缓慢，因为家中有个小朋友的干扰。
###d3入门
从写法上来看d3是一种类jquery似的写法,最鲜明的感受就是链式调用

####选择器
```
d3.select()：选择所有指定元素的第一个
d3.selectAll()：选择指定全部元素
```
####数据绑定
```
datum //单个数据绑定
let str = "this is a paragraph";
    let body = d3.select('body');
    let p1 = body.select('.area1').selectAll('p');
    p1.datum(str);
    p1.text(function (d, i) {
        return "第" + i + '个元素内容为' + d;
    });
    
data  //多个数据绑定
let arr = ['one', 'two', 'three'];
    let p2 = body.select('.area2').selectAll('p')
    p2.data(arr);
    p2.text(function (d, i) {
        return "第" + i + '个元素内容为' + d
    })
```
####插入删除
```
append() 在选择集末尾插入元素
insert() 在选择集前面插入元素
remove() 删除指定元素
```
可以通过链式调用来实现修改多种样式的功能

###实现柱状图
首先从最简单的方向开始思考。
实现柱状图就是在一块背景上加上很有个有序排列的不等矩形。基于此，可以很方便的使用svg的rect来实现矩形
```
//添加画布
var svg = d3.select('body').append('svg').attr('width', 300).attr('height', 300);

//模拟数据
let dataSet = [123, 56, 100, 80, 160];
//实现矩形
let rectHeight = 25;
console.log(svg.selectAll('rect').data(dataSet).enter());
svg.selectAll('rect')
    .data(dataSet).enter()
    .append('rect')
    .attr('x', 20)
    .attr('y', function (d, i) {
        return i * rectHeight;
    })
    .attr('width', function (d, i) {
        return d;
    })
    .attr('height', function (d, i) {
        return rectHeight - 2
    })
    .attr('fill', 'steelblue')
```
如果要改变纵向柱状图，那么只需要改变x，y和width，height就可以实现纵向的柱状图。
``` 
//添加画布
var svg = d3.select('body').append('svg').attr('width', 300).attr('height', 300);

//模拟数据
let dataSet = [123, 56, 100, 80, 160];
//实现矩形
let rectHeight = 25;
console.log(svg.selectAll('rect').data(dataSet).enter());
svg.selectAll("rect")
    .data(dataset)
    .enter()
    .append("rect")
    .attr("y",function(d,i){
         return height - d;
    })
    .attr("x",function(d,i){
         return i * rectHeight;
    })
    .attr("height",function(d){
         return d;
    })
    .attr("width",rectHeight-2)
    .attr("fill","steelblue");
```
###比例尺
在实际的开发中，不可能一一对应的关系，更多的是采用比例尺的方式来进行缩放或放大。
svg可以使用viewbox的方式来进行控制。此处采用d3的方式来进行控制
####线性比例尺
现行比例尺在我的理解中就是有序数据的整段排列
``` 
var min = d3.min(dataset);
var max = d3.max(dataset);

var linear = d3.scale.linear()
        .domain([min, max])
        .range([0, 300]);        
```
        
####序数比例尺
在我的理解中就是离散点的集合，不能按照一定规律进行总结的，只能够一一对应来进行的
```
 var index = [0, 1, 2, 3, 4];
 var fruit = ["apple", "banana", "orange", "watermelon", "peer"]
 
 var ordinal = d3.scale.ordinal()
         .domain(index)
         .range(color);
 
 ```
