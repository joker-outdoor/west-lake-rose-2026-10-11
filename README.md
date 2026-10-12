# 西湖玫瑰线

沿着西湖，跑出一朵玫瑰。采用与本次活动匹配的运动健康 GPS 轨迹，共 7415 个点，配合真实 Esri 卫星影像与 DEM，支持统一玫红、海拔、推算速度三种着色和联动剖面。起点、终点在 3D 和 2D 中均有独立标记与引线。

运动统计以运动健康（Apple Watch）为准：**22.16 km**、**2h 04m 47s**、平均配速 **5′38″/km**、累计爬升 **116.66 m**。时长使用 HealthKit 活动时长，已排除暂停；平均配速为该时长除以运动距离。未提供累计下降，不用 GPS 高度噪声补算。主运动里程与用于剖面定位的坐标测算距离分开。

逐点海拔来自运动健康 GPS；逐点速度按同一轨迹的坐标和时间差推算，非设备直接记录的速度。超过 30 秒的区间显示缺失，低于 0.5 m/s 标记停留；这不替代健康活动时长。色阶取中间 90% 的运动速度范围以减少尖峰影响，点位读数保留原值。

## 生成与预览

运动健康的带时间 GPX 私有输入与原 KML 对照文件留在仓库外。先生成真实影像与地形，再启动预览；生成时不要使用 `--demo`。

```sh
python3 -m pip install -r requirements.txt
python3 scripts/build_data.py /path/to/health-route.private.gpx --config route.config.json --output data
python3 scripts/check_data.py
node --check app.js
node --check route-style.js
python3 -m unittest discover -s scripts -p 'test_*.py'
node --test scripts/test_route_style.mjs
python3 -m http.server 8080 --bind 127.0.0.1
```

打开 `http://127.0.0.1:8080/`；`?view=2d` 为卫星俯视图。检查桌面和手机布局、三种着色与图例、完整玫瑰线、3D 粗线和遮挡线颜色、剖面指针及方向键定位，再导出清晰 PNG。实际影像中的坐标对齐需浏览器验收，健康轨迹 WGS84 坐标没有进行 GCJ-02 偏移。

公开 GPX 仅含轨迹名、坐标和海拔；公开 JSON 保留相对速度、缺失状态与汇总统计，不含逐点绝对时间、账号、头像或未知原扩展。日期保存在数据字段中，页面默认不展示。

## 来源与维护

渲染与生成器复用 [Joker Outdoor 模板](https://github.com/joker-outdoor/route-visualization-template)，模板基线为 `8335765690ff768020d2c6969c242166d70e7f0c`。通用改进先维护模板，再同步同一源码到路线项目；skill 只维护编排。

- 底图：[Esri World Imagery](https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer)。Imagery © Esri, Maxar, Earthstar Geographics, and the GIS User Community；影像版权归对应提供者。
- 地形：[Mapzen / Tilezen Terrain Tiles](https://github.com/tilezen/joerd)，SRTM / GMTED2010 courtesy of USGS；[数据署名与许可](https://github.com/tilezen/joerd/blob/master/docs/attribution.md)。
- Three.js 0.160.0 官方原文件，MIT，见 `vendor/LICENSE`。
