# 10 分钟渲染气象风场及数据 API

基于 leaflet 绘制实现动态风场的绘制，[demo](http://map.xqqx123.com/)

![demo](https://gitee.com/ice200117/leaflet-wind/raw/master/images/demo.gif)

## 开发步骤

1. 加载 js 库和 css 样式

```javascript
    <script src="libwind.min.js"></script>
    <link rel="stylesheet" href="leaflet.css">
```

2. body 标签中添加 map 容器和自定义的样式

```html
<div id="map"></div>
<div id="currentPoint" class="current-point">
  <div id="wind-value"></div>
  <div id="scale-value"></div>
</div>
<style>
  html,
  body {
    height: 100%;
  }
  #map {
    height: 100%;
  }
  .current-point {
    width: auto;
    background: white;
    position: absolute;
    top: 100px;
    left: 500px;
    z-index: 1200;
    padding: 2px 5px;
    border-radius: 5px;
  }
</style>
```

3. 添加 leaflet 地图

```javascript
//初始化地图
var map = L.map("map").setView([39.085294, 112.201538], 6);
var night = L.tileLayer(
  "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetPurplishBlue/MapServer/tile/{z}/{y}/{x}"
).addTo(map);
```

4. 添加风场矢量图层

```javascript
var dataTime = new Date();
//风场图层初始化
var windField = L.velocityLayer({});
//风场数据加载
L.windUtil().getImgData(
  dataTime,
  L.windUtil().options.products["wind"],
  function (res) {
    windField.setData(res);
    windField.addTo(map);
  }
);
```

5. 添加温度标量图层

```javascript
//温度图层初始化
var tempLayer = L.scaleLayer({
  opacity: 0.7, //透明度
});
//温度图层数据加载
var tempproduct = L.windUtil().options.products["temp"];
var tempscale = chroma.scale(tempproduct.color).domain(tempproduct.range);
L.windUtil().getImgData(dataTime, tempproduct, function (res) {
  var s = L.ScalarField.fromPNGGrid(res[0]);
  tempLayer.setColor(tempscale);
  tempLayer.setData(s, tempproduct);
  scaleLayer.addTo(map);
});
```

## 提供数据种类

1. 从 2019 年至今的 GFS 模型和 GRAPES 模型气象数据。参数包括风场、温度、湿度、气压、降水、辐射、能见度、沙尘。
2. 从 2019 年至今的空气质量站点插值格点数据。参数包括 AQI、PM<sub>2.5</sub>、PM<sub>10</sub>、CO、O<sub>3</sub>、NO<sub>2</sub>、SO<sub>2</sub>
3. 未来五天的 GFS 模型和 CUACE 模型气象预报数据。预报气象参数包括风场、温度、湿度、气压、降水、辐射、能见度、沙尘。
4. 未来五天 CUACE 模型空气质量预报数据。预报空气质量参数 AQI、PM<sub>2.5</sub>、PM<sub>10</sub>、CO、O<sub>3</sub>、NO<sub>2</sub>、SO<sub>2</sub>
5. 全国空气质量国控点、省控点、城市、区县数据接口
6. 卫星遥感火点、气溶胶、颗粒物、NO<sub>2</sub>等数据

## 更多

更多示例见

1. [喜鹊气象](http://wind.xqqx123.com/) http://wind.xqqx123.com/
2. [城市空气质量管控平台](http://xx.xqqx123.com/) http://xx.xqqx123.com/ 账号 密码 请扫描下方二维码添加微信申请访问。
3. [github 仓库]() https://github.com/ice200117/leaflet-wind
4. [gitee 仓库](http://demo.xqqx123.com/) https://gitee.com/ice200117/leaflet-wind

支持智慧气象、大气污染智能管控平台等二次开发，请添加微信

<img src="https://i.loli.net/2021/07/22/RCp5nyg14Qutcv6.jpg" alt="微信图片_20210722200044" style="zoom: 67%;" />
