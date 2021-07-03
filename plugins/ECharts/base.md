#### ECharts的基本使用  

1\. 引入 ECharts  
> 这里使用 CDN 引入，另外也可以在在打包环境中[按需引入](https://echarts.apache.org/zh/tutorial.html#在打包环境中使用%20ECharts) ECharts。   

```
<head>
  <script src="https://cdn.jsdelivr.net/npm/echarts@5.1.2/dist/echarts.min.js"></script>
</head>
```

2\. 准备 DOM 容器
```
<body>
  <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
  <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

3\. 初始化实例并添加配置
```
<script>

  // 基于准备好的dom，初始化echarts实例
  const myChart = echarts.init(document.getElementById('main'));

  // 指定图表的配置项和数据
  const option = {
    ...
  };

  // 将配置项和数据添加到图表实例
  myChart.setOption(option);

</script>
```

----

#### 深色模式  
> 在初始化echarts实例时，可以添加第二个参数 `'dark'` 用于开启深色模式。  

```
const myChart = echarts.init(document.getElementById('main'), 'dark');
```


















