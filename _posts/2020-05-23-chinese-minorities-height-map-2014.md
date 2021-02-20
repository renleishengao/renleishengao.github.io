---
layout: post
title: 2014 Height Map for Chinese Minorities
author: [maiming, yudajin]
date:   2020-05-23
categories: data
tags: 可视化 中国 少数民族 2014
description: 如果你对少数民族的身高有刻板印象，现在可以验证这些印象对不对了。
enable_comments: true
leaflet: true
license: 'cc-by-3.0-cn-en'
enable_comments: true
lang: 'en'
---

*[中文版]({% post_url 2020-05-06-shaoshu-minzu-shengao-ditu-2014 %})*

<i>Translated from the original Chinese version by Yudajin</i>

China is an ethnically diverse country with 55 recognized minorities in addition to the Han majority. The 2014 national student health report covers 26 minorities, including ethnic Zhuang, Hui and Uyghurs. Below is a map reflecting the average height of 18 year old minorities by their native regions.

## Map

Switch gender at the bottom left corner. 

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
        this._div.innerHTML = '<h4>Average height of 18-year-old boys</h4>' +  (props ?
            '<b>' + cmHeightData[props.id].minzu_en + ' in ' + cmHeightData[props.id].diji_xingzheng_danwei_en  + '</b><br />' + cmHeightData[props.id].average_male_height + ' cm'            : '');
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
        this._div.innerHTML = '<h4>Average height of 18-year-old girls</h4>' +  (props ?
            '<b>' + cmHeightData[props.id].minzu_en + ' in ' + cmHeightData[props.id].diji_xingzheng_danwei_en  + '</b><br />' + cmHeightData[props.id].average_female_height + ' cm'            : '');
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
        "Male": maleGeoJSON,
        "Female": femaleGeoJSON
    };
    L.control.layers(geoJSONs).setPosition("bottomleft").addTo(map);
    maleGeoJSON.addTo(map)

    // Default: Male
    maleLegend.addTo(map);

    // Switch
    map.on("baselayerchange", function(eventLayer) {
        //console.log(eventLayer.name);
        if (eventLayer.name == "Female") {
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

## Discussion

Ethnic mongols and Koreans are located in the North/Northeast. The Li ethnic group resides in Hainan, far South China. The rest of Chinese minorities measured all live in Western China. 

As with the Han, there is an obvious north/south shift in height. The tallest ethnic minority are the Hui from Ningxia, northwestern China, with boys measuring 174.5 cm. Northeastern Chinese groups such as Koreans (171.2cm) and Tongliao Mongolians (171.4 cm) also boast relatively tall heights. The southwest, on the other hand, is significantly shorter, with ethnic Hmong 18 year old boys at only 162.1 cm, while Wa girls at 18 only stand 151.1 cm.

In most regions, Han Chinese are taller than the minorities, which is especially evident in Southwest China, which also boasts a big gap in height between Tibetid and inland SEA groups such as the Wa and Zhuang difference. Ningxia Hui on the other hand match Ningxia Han in height, with Kazakhs in the northwest similar to the Han as well. Ethnic Li and Han from Hainan are both similar in height as well. Overall, the Han are taller than minorities in most regions, and we discern the reasons to genes and living standards, though there is no absolute certainly.

## Sampling

The national student health report on Chinese minorities samples students from minority enclaves in a region.  For ethnics concentrated in one region only, there is little problem with accurate representation. However,  some ethnic groups such as the nomadic Mongols and the Hui people are scattered all over northern China, including Inner Mongolia, Xinjiang, Gansu and Qinghai, making it difficult for minorities of one region to represent the same ethnicity in another region.

For example, the sample for Mongols come from Tongliao city in Eastern Inner Mongolia, far from Mongols in Xinjiang who may be different. Ethnic Manchus, despite their population in the tens of millions are not represented in this study, as many Manchus have assimilated into Han culture and lack a majority ethnic settlement.

## Why age 18?

Unlike the height map of 17 year olds in East Asia, 18 year old minority students are represented instead. Firstly, it's because this graph lacks comparison purposes, as countries like Japan, who only produce student reports up to age 17. The sample size for 18 year old minorities is also plentiful. 

Secondly, most Chinese minorities are classified as low-socioecononic in class, which correlates with a later onset of puberty, making 18 year olds more representive of adult height. For example, the height difference between 18 year old ethnic Yi males and females is less than 10 cm, with the Hmong, Dongxiang and Li ethnicities producing a gender height of less than 11 cm. Women finish puberty earlier than men, and the gender height gap in adulthood is generally around 12 to 14 cm, which suggests that many of these 18 year old boys have yet to finish growing, let alone 17 year olds.

## Disclaimer
The geographic information used in this map has been derived from the Internet and does not imply any endorsement of the site or the author.