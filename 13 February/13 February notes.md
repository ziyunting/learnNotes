2月13日
7:30起床  
8:20-9:00 英语学习 
9:00-9:40 复习昨天学习的d3(从今天起使用d3v5版本进行学习)   
ps:发现d3很多文档都是英文的，觉得有必要增加一点英语学习时间，由于内网机无法正常开展工作，于是增加一些学习的种类。
9:40-11:40 完善完整的d3柱状图

1:30-3:00 梳理项目代码
3:00-4:00 参加瞩目会议
4:00-6:00 为柱状图添加动画和交互

###d3中的update enter exit
简单通俗的来讲 update就是起到更新的作用 用于已有的匹配  
enter就是当元素的个数小于数据的个数时，需要新增元素实现一一匹配，enter进入，有新的元素进入画布中
exit就是当元素的个数大于数据的个数时，需要删除元素实现一一匹配，exit退出，多余元素退出画布

###d3 v5版本中的比例尺修正
在昨天的学习中使用的是d3 v3的版本。跟导师沟通后，导师建议使用d3 v5的版本来进行学习，因为v3版本的d3版本有些老旧,
部分API已经不能够正常使用。例如比例尺API  
早上将已有的v3更换为v5版本后，发现比例尺功能不能够正常使用。查看文档后是它的api发生了变化 
####线性比例尺 
原来是使用d3.scale.linear，现在是使用d3.scaleLinear
``` 
let linear = d3.scaleLinear()
                .domain([0, d3.max(dataSet)])
                .range([0, 250]);
```
####序数比例尺
原来是使用d3.scale.ordinal,现在是使用d3.scaleOrdinal
``` 
 var index = [0, 1, 2, 3, 4];
 var fruit = ["apple", "banana", "orange", "watermelon", "peer"]
 
 var ordinal = d3.scaleOrdinal()
         .domain(index)
         .range(color);
```
###d3 v5版本中坐标轴变化
原来是使用d3.axis.scale(liner).orient('bottom')
修正后使用d3.axisBottom(linear)
``` 
  let axis =d3.axisBottom(linear).ticks(3);
```
在这里有一个需要注意的地方是tickets的使用不是根据他的段数来决定的，而是有除数来决定的
真正的思考逻辑是，用1,2,5及其10倍数作为除数去计算。比如[0, 600]，用1,2,5及其10倍数去除，而不要用3,6,9之类的数去除。
600/50=12，所以ticks填写12是可行的，600/3=200，所以ticks填写200是不行的

scaleBand  
当创建一个bar chart时，scaleBand可以帮助我们来决定bar的几何形状，并且已经考虑好了各个bar之间的padding值。
输入的domain通过一个数值数组来指定（每个值都对应一个band）并且range通过bands的最小和最大范围来定义(也就是bar chart的整个宽度)
scaleBand会将range划分为n个bands(n是domain数组的数值个数)并且在考虑padding的情况下计算出每个band的位置和宽度.
####d3动画
利用.transition().delay().duration 来实现  （和jquery极其类似）
###d3交互
可利用on来实现最基本的交互。
click：鼠标单击某元素时触发，相当于mousedown和mouseup的组合
mouseover：鼠标放在某元素上触发
mouseout：鼠标移出某元素时触发
mousemove：鼠标移动时触发
mousedown：鼠标按钮被按下时触发
mouseup：鼠标按钮被松开时触发
dblclick：鼠标双击时触发


接下来一方面考虑在优化柱状图的同时，接触一些其他的图形。同时能够利用d3来实现一些其他的图形。
