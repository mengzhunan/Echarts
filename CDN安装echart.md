## 获取Apache ECharts

### 安装
**介绍**：Apache ECharts支持多种下载方式
- 从npm获取
  ```js
  npm install echarts --save
  ```
- 从 CDN 获取,推荐从 jsDelivr 引用 echarts。https://www.jsdelivr.com/package/npm/echarts
- 从 GitHub 获取
  [apache/echarts](https://github.com/apache/echarts) 项目的 [release](https://github.com/apache/echarts/releases) 页面可以找到各个版本的链接。点击下载页面下方 Assets 中的 Source code，解压后 dist 目录下的 echarts.js 即为包含完整 ECharts 功能的文件。
- 在线定制
如果只想引入部分模块以减少包体积，可以使用 [ECharts 在线定制功能](https://echarts.apache.org/zh/builder.html)。

## 引入 Apache ECharts

### 在 HTML 文件中引入
1. 将保存 echarts.js 的目录新建一个 index.html 文件，内容如下：
   ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <!-- 引入刚刚 ECharts 文件 -->
        <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.2/dist/echarts.min.js"></script>
    </head>
    </html>
   ```
   打开这个 index.html，你会看到一片空白。但是不要担心，打开控制台确认没有报错信息，就可以进行下一步。
2. 绘制一个简单的图表
   在绘图前我们需要为 ECharts 准备一个定义了高宽的 DOM 容器，添加：
   ```html
   <body>
     <!-- 为 ECharts 准备一个定义了宽高的 DOM -->
     <div id="main" style="width: 600px;height:400px;"></div>
   </body>
   ```
3. 然后通过 echarts.init 方法初始化一个 echarts 实例并通过 setOption 方法生成一个简单的柱状图，下面是完整代码。
   ```js
    // 初始化echarts对象将页面中的图标容器传入，提高init方法
        let myCharts = echarts.init(document.querySelector('#demo'));

        // 指定图标的配置选项
        let option = {
            // 标题
            title:{
                text:"销量图标"
            },
            // 提示框
            tooltip:{},
            // 
            legend:{
                data:['销量']
            },
            // x轴的内容
            xAxis: {
                data: ['足球', '篮球', '兵乓球', '羽毛球', '橄榄球', '棒球']
            },
            // y轴的内容
            yAxis: {}, //根据数量默认显示
            series:[
                {
                    name:'销量',
                    type:'bar',
                    data:[12,65,85,40,10,45]
                }
            ]
        };
        // 将配置指定到初始化echarts对象中
        myCharts.setOption(option)
   ```
### 按需引入 ECharts 图表和组件
**介绍**:上面的代码会引入 ECharts 中所有的图表和组件，但是假如你不想引入所有组件，也可以使用 ECharts 提供的按需引入的接口来打包项目必须的组件。
```js
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import * as echarts from 'echarts/core';
// 引入柱状图图表，图表后缀都为 Chart
import { BarChart } from 'echarts/charts';
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent
} from 'echarts/components';
// 标签自动布局，全局过渡动画等特性
import { LabelLayout, UniversalTransition } from 'echarts/features';
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from 'echarts/renderers';

// 注册必须的组件
echarts.use([
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  BarChart,
  LabelLayout,
  UniversalTransition,
  CanvasRenderer
]);

// 接下来的使用就跟之前一样，初始化图表，设置配置项
var myChart = echarts.init(document.getElementById('main'));
myChart.setOption({
  // ...
});
```
**注意**：的是为了保证打包的体积是最小的，ECharts 按需引入的时候不再提供任何渲染器，所以需要选择引入 CanvasRenderer 或者 SVGRenderer 作为渲染器。这样的好处是假如你只需要使用 svg 渲染模式，打包的结果中就不会再包含无需使用的 CanvasRenderer 模块。