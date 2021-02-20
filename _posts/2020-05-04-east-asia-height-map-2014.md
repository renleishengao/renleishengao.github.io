---
layout: post
title:  East Asia Height Map, 2014
author: [maiming, yudajin]
date:   2020-05-04
categories: data
tags: 可视化 东亚 中国 日本 韩国 2014
description: 不言而喻，一目了然。
enable_comments: true
leaflet: true
license: 'cc-by-3.0-cn-en'
enable_comments: true
lang: 'en'
---

*[中文版]({% post_url 2020-05-04-dongya-shengao-ditu-2014 %})*

<i>Translated from the original Chinese version by Yudajin</i>

Over the past century, average height in East Asia has undergone unprecedented growth, with many regions such as Korea and Japan reporting increases of 10 cm+ in stature from the end of WW2 to today. The graph below displays the average height of 17 year old students in East Asia as of 2014.

The data is taken from student physical examinations from various East Asian regions during 2014, before being published by official agencies in the form of legible reports. Hence indicating the credibility of the sources. The age is set to 17, allowing for comparability between nations and regions.

## Map
Switch gender at the bottom left corner. 
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
        attribution: '&copy; <a href="https://osm.org/copyright">OpenStreetMap</a> contributors <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC BY-SA</a> | &copy; 埋名匿姓 <a href="https://creativecommons.org/licenses/by/3.0/cn/">CC BY 3.0 CN</a>'
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
        this._div.innerHTML = '<h4>Average height of 17-year-old boys</h4>' +  (props ?
            '<b>' + heightData[props.name].name_en + '</b><br />' + heightData[props.name].average_male_height + ' cm'
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
        this._div.innerHTML = '<h4>Average height of 17-year-old girls</h4>' +  (props ?
            '<b>' + heightData[props.name].name_en + '</b><br />' + heightData[props.name].average_female_height + ' cm'
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
        "Boys":maleGeoJSON,
        "Girls": femaleGeoJSON
    };
    L.control.layers(geoJSONs).setPosition("bottomleft").addTo(map);
    maleGeoJSON.addTo(map)

    // Default: Male
    maleLegend.addTo(map);

    // Switch
    map.on("baselayerchange", function(eventLayer) {
        //console.log(eventLayer.name);
        if (eventLayer.name == "Girls") {
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

## Data Source
- Mainland China: [2014 National Survey on Students' Constitution and Health](http://www.hep.com.cn/book/details?uuid=7c44562d-151f-1000-84e6-55b34aba28f0) (全国学生体质与健康调研). *Han* data are used for all provinces except Tibet, while Lhasa Tibetan data are used for Tibet. 
- Japan: 2014 [School Health Examination Survey](https://www.mext.go.jp/b_menu/toukei/chousa05/hoken/1268826.htm) (学校保健統計調査).
- South Korea: 2015 [Statistical Yearbook of Education](https://kess.kedi.re.kr/main/publ/link?linkKey=291&userLang=ko) (교육통계연보).
- Data for North Korea, Mongolia, Hong Kong, Macau and Taiwan are temporarily absent.

Feel free to contact our page ([email us!](mailto:renleishengao@protonmail.com)) if you find any useful statistics regarding human height. We will assess the quality of the source, and potentially include it in our graphs. Thanks.

## East Asia: tallest and shortest region

China is home to both records. Beijing boys are the tallest in East Asia, averaging 175.86 cm, while girls from Shandong province are the tallest, standing at 162.93 cm on average. Guizhou holds the shortest height, with 17 year old boys at only 167.51 cm while girls break in at 155.47 cm. Hence, the statistical range is approx 8 cm.

The range increases to 8.5 to 10 cm if we separate rural and urban regions within China. Urban Shandong boys average 176.42 cm, while urban Beijing girls average 163.59 cm. Southwest China remains the shortest, with 166.49 cm for boys in rural Sichuan and 155.05 cm for girls in rural Guizhou.

## Why 17-year-olds?
Many samples of students measurement cap at age 17, making that age the most accurate for credible comparisons between countries universally.

## How much can people grow after 17?
While there is no definite answer, it is generally believed that in developed regions, girls will reach their final height by 17. Boys from the same region can be expected to grow 0.5 to 1 cm, statistically. However, people from poorer/malnourished regions tend to have a longer growth phrase/later onset of puberty, leading to more height gain after 17 compared to those from wealthy regions.

## Ethnic minorities in China are not included, why is this?
Data from the Chinese survey measures ethnic Han and minorities separately. Since the population ratio of minorities to Han is generally skewed in favour of the Han in most provinces, it can be difficult to combine both samples in the making of a graph supposed to represent the average populace. Instead, we will create a separate data graph just for Chinese minorities. 

## Disclaimer
The geographic information used in this map has been derived from the Internet and does not imply any endorsement of the site or the author.