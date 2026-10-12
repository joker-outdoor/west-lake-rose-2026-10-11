# 西湖玫瑰线

沿着西湖，跑出一朵玫瑰。真实 KML 的 4370 个轨迹点完整保留，使用 Esri 卫星影像和 DEM 地形，支持统一玫红、按海拔、按推算速度着色，以及联动海拔剖面和 2D 俯视图。

地图按坐标测算为 **21.73 km**。原记录里程为 **21.756429 km**，累计爬升 **141.31 m**、下降 **138.73 m**。原记录总耗时 8348.110 秒，减去暂停 480.741 秒后，运动时间为 **2h 11m 07s**；平均配速按原记录里程计算，为 **6′02″/km**。两种里程口径分别标注。

逐点速度由相邻 WGS84 坐标与时间差推算，单位 m/s，页面显示 km/h 与配速。超过 30 秒的间隔不显示速度；缺失时间保持缺失；低于 0.5 m/s 标记停留且不显示配速。局部停留标记不替代原记录暂停统计。导出方的未知扩展数组没有当作实测速度。

## 生成与预览

原始 KML 留在仓库外。先生成真实影像与地形，再启动预览；生成时不要使用 `--demo`。

```sh
python3 -m pip install -r requirements.txt
python3 scripts/build_data.py /path/to/source.kml --config route.config.json --output data
python3 scripts/check_data.py
node --check app.js
node --check route-style.js
python3 -m unittest discover -s scripts -p 'test_*.py'
node --test scripts/test_route_style.mjs
python3 -m http.server 8080 --bind 127.0.0.1
```

打开 `http://127.0.0.1:8080/`；`?view=2d` 为卫星俯视图。检查桌面和手机布局、三种着色与图例、完整玫瑰线、3D 粗线和遮挡线颜色、剖面指针及方向键定位，再导出清晰 PNG。实际影像中的坐标对齐需浏览器验收，KML 坐标没有进行 GCJ-02 偏移。

公开 GPX 仅含轨迹名、坐标和海拔；公开 JSON 保留相对速度、缺失状态与汇总统计，不含逐点绝对时间、账号、头像或未知原扩展。日期保存在数据字段中，页面默认不展示。

## 来源与维护

渲染与生成器复用 [Joker Outdoor 模板](https://github.com/joker-outdoor/route-visualization-template)，模板基线为 `8335765690ff768020d2c6969c242166d70e7f0c`。通用改进先维护模板，再同步同一源码到路线项目；skill 只维护编排。

- 底图：[Esri World Imagery](https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer)。Imagery © Esri, Maxar, Earthstar Geographics, and the GIS User Community；影像版权归对应提供者。
- 地形：[Mapzen / Tilezen Terrain Tiles](https://github.com/tilezen/joerd)，SRTM / GMTED2010 courtesy of USGS；[数据署名与许可](https://github.com/tilezen/joerd/blob/master/docs/attribution.md)。
- Three.js 0.160.0 官方原文件，MIT，见 `vendor/LICENSE`。
