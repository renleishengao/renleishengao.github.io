---
layout: post
title:  2014年东亚身高地图
author: maiming
date:   2020-05-04
categories: data
tags: 可视化 东亚 中国 日本 韩国 2014
description: 不言而喻，一目了然。
enable_comments: true
leaflet: true
license: 'cc-by-3.0-cn'
enable_comments: true
---

<i>[English version]({% post_url 2020-05-04-east-asia-height-map-2014 %})</i>

一百年来，东亚各国的平均身高经历了空前的增长。在日本和韩国，青年人身高从二战后到现在增加了近10 cm。下图以最直观和客观的方式呈现东亚年轻人的身高。

我们选择了东亚各国的学生体检数据。测量时间都在2014年，均由各国官方机构调查和出版，年龄也统一为17岁，保证了数据之间的可比性。

## 地图
左下角切换性别。
<div id="mapid" style="height: 500px; width:90%; margin-left:auto;
  margin-right:auto;"></div>
<script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
crossorigin=""></script>
<script src="{{ site.baseurl }}/assets/js/china-geojson.min.js"></script>
<script src="{{ site.baseurl }}/assets/js/south-korea-geojson.min.js"></script>
<script src="{{ site.baseurl }}/assets/js/japan-geojson.min.js"></script>
<script src="{{ site.baseurl }}/assets/js/east-asia-height.min.js"></script>
<script>
    var map = L.map('mapid', {fullscreenControl: true}).setView([34.533333, 108.916667], 3);
    var tileLayer = L.tileLayer('https://{s}.tile.osm.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://osm.org/copyright">OpenStreetMap</a> 贡献者 <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC BY-SA</a> | &copy; 埋名匿姓 <a href="https://creativecommons.org/licenses/by/3.0/cn/">CC BY 3.0 CN</a>'
    }).addTo(map);

    //L.geoJson(chinaData).addTo(map);

    // control that shows state info on hover
	var maleInfo = L.control();

    maleInfo.onAdd = function (map) {
        this._div = L.DomUtil.create('div', 'info');
        this.update();
        return this._div;
    };

    maleInfo.update = function (props) {
        this._div.innerHTML = '<h4>17 岁男性平均身高</h4>' +  (props ?
            '<b>' + heightData[props.name].name_zh + '</b><br />' + heightData[props.name].average_male_height + ' cm'
            : '');
    };

    // Default: Male
    maleInfo.addTo(map);

    var femaleInfo = L.control();

    femaleInfo.onAdd = function (map) {
        this._div = L.DomUtil.create('div', 'info');
        this.update();
        return this._div;
    };

    femaleInfo.update = function (props) {
        this._div.innerHTML = '<h4>17 岁女性平均身高</h4>' +  (props ?
            '<b>' + heightData[props.name].name_zh + '</b><br />' + heightData[props.name].average_female_height + ' cm'
            : '');
    };

    //info.addTo(map);

    function getFemaleColor(h) {
        return h >= 162 ? '#99000d' :
               h >= 161 ? '#cb181d' :
               h >= 160 ? '#ef3b2c' :
               h >= 159 ? '#fb6a4a' :
               h >= 158 ? '#fc9272' :
               h >= 157 ? '#fcbba1' :
               h >= 156 ? '#fee0d2' :
                          '#fff5f0';
    }

    function getMaleColor(h) {
        return  h >= 175 ? '#08306b' :
                h >= 174 ? '#08519c' :
                h >= 173 ? '#2171b5' :
                h >= 172 ? '#4292c6' :
                h >= 171 ? '#6baed6' :
                h >= 170 ? '#9ecae1' :
                h >= 169 ? '#c6dbef' :
                h >= 168 ? '#deebf7' :
                          '#f7fbff';
    }

    function femaleStyle(feature) {
        return {
            weight: 1,
            opacity: 1,
            color: '#666',
            dashArray: '1',
            fillOpacity: 0.8,
            fillColor: getFemaleColor(heightData[feature.properties.name].average_female_height)
        };
    }

    function maleStyle(feature) {
        return {
            weight: 1,
            opacity: 1,
            color: '#666',
            dashArray: '1',
            fillOpacity: 0.8,
            fillColor: getMaleColor(heightData[feature.properties.name].average_male_height)
        };
    }

    function maleHighlightFeature(e) {
        var layer = e.target;

        layer.setStyle({
                weight: 5,
                color: '#666',
                dashArray: '',
                fillOpacity: 0.7
        });

        if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
            layer.bringToFront();
        }
        maleInfo.update(layer.feature.properties);
    }

    function femaleHighlightFeature(e) {
        var layer = e.target;

        layer.setStyle({
                weight: 5,
                color: '#666',
                dashArray: '',
                fillOpacity: 0.7
        });

        if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
            layer.bringToFront();
        }
        femaleInfo.update(layer.feature.properties);
    }

    function resetFemaleHighlight(e) {
        femaleGeoJSON.resetStyle(e.target);
        femaleInfo.update();
    }

    function resetMaleHighlight(e) {
        maleGeoJSON.resetStyle(e.target);
        maleInfo.update();
    }

    function zoomToFeature(e) {
        map.fitBounds(e.target.getBounds());
    }

    function onEachFemaleFeature(feature, layer) {
        layer.on({
            mouseover: femaleHighlightFeature,
            mouseout: resetFemaleHighlight,
            click: zoomToFeature
        });
    }

    function onEachMaleFeature(feature, layer) {
        layer.on({
            mouseover: maleHighlightFeature,
            mouseout: resetMaleHighlight,
            click: zoomToFeature
        });
    }

    var eastAsiaGeoData = [chinaGeoData, southKoreaGeoData, japanGeoData];
    var maleGeoJSON = L.geoJSON(eastAsiaGeoData, {
        style: maleStyle,
        onEachFeature: onEachMaleFeature
    });
    var femaleGeoJSON = L.geoJSON(eastAsiaGeoData, {
        style: femaleStyle,
        onEachFeature: onEachFemaleFeature
    });

    var maleLegend = L.control({position: 'bottomright'});

    maleLegend.onAdd = function (map) {

        var div = L.DomUtil.create('div', 'info legend'),
            grades = [167,168,169,170,171,172,173,174,175],
            labels = [];

        // loop through our density intervals and generate a label with a colored square for each interval
        for (var i = 0; i < grades.length; i++) {
            div.innerHTML +=
                '<i style="background:' + getMaleColor(grades[i] + 0.1) + '"></i> ' +
                grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
        }

        return div;
    };

    var femaleLegend = L.control({position: 'bottomright'});

    femaleLegend.onAdd = function (map) {

        var div = L.DomUtil.create('div', 'info legend'),
            grades = [155,156,157,158,159,160,161,162],
            labels = [];

        // loop through our density intervals and generate a label with a colored square for each interval
        for (var i = 0; i < grades.length; i++) {
            div.innerHTML +=
                '<i style="background:' + getFemaleColor(grades[i] + 0.1) + '"></i> ' +
                grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
        }

        return div;
    };
    geoJSONs = {
        "男":maleGeoJSON,
        "女": femaleGeoJSON
    };
    L.control.layers(geoJSONs).setPosition("bottomleft").addTo(map);
    maleGeoJSON.addTo(map)

    // Default: Male
    maleLegend.addTo(map);

    // Switch
    map.on("baselayerchange", function(eventLayer) {
        //console.log(eventLayer.name);
        if (eventLayer.name == "女") {
            this.removeControl(maleLegend);
            this.removeControl(maleInfo);
            femaleLegend.addTo(this);
            femaleInfo.addTo(this);
        } else {
            this.removeControl(femaleLegend);
            this.removeControl(femaleInfo);
            maleLegend.addTo(this);
            maleInfo.addTo(this);
        }
    });

</script>

## 数据来源
- 中国内地：[2014年全国学生体质与健康调研](http://www.hep.com.cn/book/details?uuid=7c44562d-151f-1000-84e6-55b34aba28f0)。其中除西藏外，各省均为**汉族**数据，西藏使用拉萨藏族数据。调研中各省城乡分开统计，我按照城、乡简单平均计算各省身高；
- 日本：2014年[学校保健统计调查](https://www.mext.go.jp/b_menu/toukei/chousa05/hoken/1268826.htm)；
- 韩国：2015年[教育统计年鉴](https://kess.kedi.re.kr/main/publ/link?linkKey=291&userLang=ko)（교육통계연보）；
- 朝鲜、蒙古、香港、澳门、台湾等国家和地区数据暂缺。

如果你有其他的身高数据来源，你可以在本页下面评论，或者直接与我联系。在确认数据的准确性之后，我会加入你的数据。

## 东亚身高之最
男生最高的地区是中国北京市，平均175.86 cm，女生最高是中国山东省，平均162.93 cm。最矮的也在中国，贵州省男女都是东亚地区最矮，平均身高为167.51/155.47 cm，极差为8厘米左右。

如果把中国调查中的城乡分开，差距还会更极端。最高的山东城市男生平均176.42 cm，最高的北京城市女生平均163.59 cm。最矮的依然在中国西南地区，贵州农村女生平均155.05 cm，四川农村男生平均166.49 cm。差距扩大到8.5～10 cm。

## 为什么是17岁？
中国的调研年龄是7～22岁，韩国是6～17岁（有的年份到18岁），日本是5～17岁。为了保证不同国家数据的可比性，我选取了共有的，而且最接近成年的年龄，也就是17岁。

## 17岁到成年还能长多少？
暂无定论，但是普遍认为沿海发达地区的女生不会再有增长，而男生的增长也在1 cm之内。在欠发达、高海拔地区，人类的生长发育期会更长，因此增幅依地区而不同，不可一概而论。

## 为什么不包含中国其他的少数民族？
这是全国学生体质与健康调研的问题。此调研中，少数民族和各省汉族分开调查，导致暂时没有较好的数据呈现方式。

在汉族比例较高（比如说大于90%）的省份，只选取汉族数据，能够客观地反应当地身高，但是在西部、边疆地区并不如此。除了西藏之外，内蒙古、宁夏、青海、新疆、云南等地均没有在当地占据绝对人口优势的民族。因此也不能直接采用少数民族数据。

我单独制作了一个中国少数民族的身高地图。鉴于西部边疆地区多民族混杂，而蒙古族、回族等民族地域分布广，内部身高差异大，我会把数据精确到取样的地级行政单位，而不仅仅是省级。

*2020-05-06更新：[少数民族身高地图]({% post_url 2020-05-06-shaoshu-minzu-shengao-ditu-2014 %})已经制作完成。*

## 声明
本地图中使用的地理信息数据均来自网络，并不代表本站或本文作者的态度。