---
layout: post
title:  2014年中国少数民族身高地图
author: maiming
date:   2020-05-06
categories: data
tags: 可视化 中国 少数民族 2014
description: 如果你对少数民族的身高有刻板印象，现在可以验证这些印象对不对了。
leaflet: true
license: 'cc-by-3.0-cn'
---

<i>[English version]({% post_url 2020-05-23-chinese-minorities-height-map-2014 %})</i>

中国是一个多民族国家，除去占据人口绝对优势的汉族，还承认其他55个少数民族。中国的诸多身高调查也包含了这些少数民族。[2014年全国学生体质与健康调研](http://www.hep.com.cn/book/details?uuid=7c44562d-151f-1000-84e6-55b34aba28f0)涵盖了壮族、回族、维吾尔族等26个人口较多的少数民族，我们在此把这次调查数据绘制成身高地图，以方便查看。

## 地图
左下角切换性别。
<div id="mapid" style="height: 500px; width:90%; margin-left:auto;
  margin-right:auto;"></div>
<script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
crossorigin=""></script>
<script src="{{ site.baseurl }}/assets/js/china-minorities-geojson.min.js"></script>
<script src="{{ site.baseurl }}/assets/js/china-minorities-height.min.js"></script>
<script>
    var map = L.map('mapid', {fullscreenControl: true}).setView([34.533333, 108.916667], 3);
    var tileLayer = L.tileLayer('https://{s}.tile.osm.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://osm.org/copyright">OpenStreetMap</a> 贡献者 <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC BY-SA</a> | &copy; 埋名匿姓 <a href="https://creativecommons.org/licenses/by/3.0/cn/">CC BY 3.0 CN</a>'
    }).addTo(map);

    // control that shows state info on hover
	var maleInfo = L.control();

    maleInfo.onAdd = function (map) {
        this._div = L.DomUtil.create('div', 'info');
        this.update();
        return this._div;
    };

    maleInfo.update = function (props) {
        this._div.innerHTML = '<h4>18 岁男性平均身高</h4>' +  (props ?
            '<b>' + cmHeightData[props.id].diji_xingzheng_danwei + cmHeightData[props.id].minzu + '</b><br />' + cmHeightData[props.id].average_male_height + ' cm'            : '');
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
        this._div.innerHTML = '<h4>18 岁女性平均身高</h4>' +  (props ?
            '<b>' + cmHeightData[props.id].diji_xingzheng_danwei + cmHeightData[props.id].minzu + '</b><br />' + cmHeightData[props.id].average_female_height + ' cm'
            : '');
    };

    //info.addTo(map);

    function getFemaleColor(h) {
        return h >= 160 ? '#a50f15' :
               h >= 158 ? '#de2d26' :
               h >= 156 ? '#fb6a4a' :
               h >= 154 ? '#fc9272' :
               h >= 152 ? '#fcbba1' :
                          '#fee5d9';                
    }

    function getMaleColor(h) {
        return  h >= 173 ? '#084594' :
                h >= 171 ? '#2171b5' :
                h >= 169 ? '#4292c6' :
                h >= 167 ? '#6baed6' :
                h >= 165 ? '#9ecae1' :
                h >= 163 ? '#c6dbef' :
                           '#eff3ff';
    }

    function femaleStyle(feature) {
        return {
            weight: 1,
            opacity: 1,
            color: '#666',
            dashArray: '1',
            fillOpacity: 0.8,
            fillColor: getFemaleColor(cmHeightData[feature.properties.id].average_female_height)
        };
    }

    function maleStyle(feature) {
        return {
            weight: 1,
            opacity: 1,
            color: '#666',
            dashArray: '1',
            fillOpacity: 0.8,
            fillColor: getMaleColor(cmHeightData[feature.properties.id].average_male_height)
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

    var maleGeoJSON = L.geoJSON(chinaMinoritiesGeoData, {
        style: maleStyle,
        onEachFeature: onEachMaleFeature
    });
    var femaleGeoJSON = L.geoJSON(chinaMinoritiesGeoData, {
        style: femaleStyle,
        onEachFeature: onEachFemaleFeature
    });

    var maleLegend = L.control({position: 'bottomright'});

    maleLegend.onAdd = function (map) {

        var div = L.DomUtil.create('div', 'info legend'),
            grades = [161,163,165,167,169,171,173],
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
            grades = [150,152,154,156,158,160],
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

## 概述

除了蒙古族、朝鲜族位于东北地区，黎族位于海南之外，这次调查的少数民族均位于中国西部。东北、西北少数民族身材较高，而在西南地区内部存在明显的分化：青藏高原及其边缘的身材身高较高，距离青藏高原较远的身材较矮。

男生平均最高的民族是宁夏回族，平均174.5 cm，女生最高的则是撒拉族，达到160.9 cm，均位于西北地区。最矮的民族在西南地区，苗族男生18岁平均身高仅162.1 cm，佤族女生仅151.1 cm。男生极差13 cm，女生极差10 cm。

与全国汉族平均相比，少数民族身材较矮：

- 回族、哈萨克族身高明显超过汉族；
- 藏族、柯尔克孜族、白族、撒拉族、东乡族与汉族身高相近；
- 其他均明显矮于汉族。

如在各省份内部（除西藏外，西藏无汉族数据）进行比较，也是如此：

- 宁夏回族与汉族身高相近；
- 新疆哈萨克族与汉族身高相近；
- 海南黎族与汉族身高相近。
- 云南汉族与纳西族身高相近，矮于白族；
- 四川汉族矮于羌族；
- 其他的比较均为当地汉族明显高于当地少数民族。

我们暂时**无法确定**这些差异是来源于生活水平，还是基因。

## 取样方法的问题

学生体质与健康调研分开调查各个民族，取样的方法是先确定民族聚居的地区，然后在这些地区选取学校，抽出学生进行测试。对于聚居地集中的民族，这样的抽样方法并无不可。但是对于人口分布广、多与其他民族混居的民族，这样取出的样本并不能很好的代表全民族。例如：

- 蒙古族分布在内蒙古、新疆、青海各地，却只有唯一的取样区——通辽市。
- 回族除聚居在宁夏、甘肃、青海部分地区外，也散居在东部诸省和新疆，被宁夏一地的回族代表。
- 满族人口超过一千万，是中国第四大民族，但并没有被纳入学生体质与健康调研中，原因恐怕是普遍与汉族混居，缺乏典型的聚居地。

## 又是18岁了？

与之前绘制的[东亚身高地图]({% post_url 2020-05-04-dongya-shengao-ditu-2014 %})不同，这里我们采用了各少数民族的18岁学生数据。首先是因为并不需要和其他国家数据对比，不存在18岁数据缺乏的问题。其次，中国许多少数民族社会经济条件较差，发育期晚，17岁数据与成年身高相差太大。例如

- 彝族、撒拉族18岁男女身高差不足10厘米；
- 苗族、东乡族、黎族不足11厘米。

较低的男女身高差提示这些民族男生身高在成年之后仍然可以增长。

## 声明

本地图中使用的地理信息数据均来自网络，并不代表本站或本文作者的态度。