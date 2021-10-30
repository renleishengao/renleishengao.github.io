---
layout: post
title: 瑞士各州身高差异的分析
author: [maiming]
date:   2021-10-30
categories: analysis
tags: 瑞士 精英效应 全样本 
description: 我们获取到了最近5年瑞士分州的兵役体质测试数据，并进行简要分析
license: 'cc-by-3.0-cn'
enable_mathjax: true
---

## 数据来源

我们从瑞士联邦体育局（Bundesamt für Sport, BASPO）的网站上获取到了瑞士2016—2020年[兵役体质测试](https://www.baspo.admin.ch/de/sportfoerderung/breitensport/fitnesstest-armee-fta-rekrutierung.html)（Fitnesstest der Armee, FTA）的数据。

### 样本量与代表性

2016年拥有完整体质测试数据的男性应征者为32329人，占男性总征兵数81%，2017年30638人，占83%，2018年26005人，占83%，2019年25725人，占86.3%，2020年20240人，占88.9%，列入下表。网站上文档还提供了瑞士各州的样本量和占总征兵数的比例。2016—2020年各州体质测试比例均在70%以上，保证了较好的代表性。

<table>
  <caption>
    瑞士2016—2020年兵役体质测试男性样本量与占总男性征兵数的比例
  </caption>
  <thead>
    <tr>
      <th>年份</th>
      <th>样本量</th>
      <th>比例</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2016</td>
      <td>32329</td>
      <td>81%</td>
    </tr>
    <tr>
      <td>2017</td>
      <td>30638</td>
      <td>83%</td>
    </tr>
    <tr>
      <td>2018</td>
      <td>26005</td>
      <td>83%</td>
    </tr>
    <tr>
      <td>2019</td>
      <td>25725</td>
      <td>86.3%</td>
    </tr>
    <tr>
      <td>2020</td>
      <td>20240</td>
      <td>88.9%</td>
    </tr>
  </tbody>
</table>

## 身体形态

瑞士2016—2020年的平均身高和体重如下。

<table>
  <caption>
    瑞士2016—2020年兵役体质测试男性平均身高
  </caption>
  <thead>
    <tr>
      <th>年份</th>
      <th>样本量</th>
      <th>平均身高（cm）</th>
      <th>平均体重（kg）</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2016</td>
      <td>32329</td>
      <td>178.4</td>
      <td>74.3</td>
    </tr>
    <tr>
      <td>2017</td>
      <td>30638</td>
      <td>178.4</td>
      <td>74.2</td>
    </tr>
    <tr>
      <td>2018</td>
      <td>26005</td>
      <td>178.5</td>
      <td>74.2</td>
    </tr>
    <tr>
      <td>2019</td>
      <td>25725</td>
      <td>178.5</td>
      <td>74.7</td>
    </tr>
    <tr>
      <td>2020</td>
      <td>20240</td>
      <td>178.4</td>
      <td>74.87</td>
    </tr>
  </tbody>
</table>

我们以州别对2016—2020年的身高、体重数据使用各年份样本量进行加权平均，得到如下的结果。

<table>
  <caption>
    瑞士2016—2020年兵役体质测试各州男性平均身高
  </caption>
  <thead>
    <tr>
      <th>州</th>
      <th>州代码</th>
      <th>样本量</th>
      <th>平均身高（cm）</th>
      <th>平均体重（kg）</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>阿尔高</td>
      <td>AG</td>
      <td>10640</td>
      <td>178.6</td>
      <td>75.1</td>
    </tr>
    <tr>
      <td>内阿彭策尔</td>
      <td>AI</td>
      <td>399</td>
      <td>177.2</td>
      <td>71.2</td>
    </tr>
    <tr>
      <td>外阿彭策尔</td>
      <td>AR</td>
      <td>1114</td>
      <td>178.5</td>
      <td>72.9</td>
    </tr>
    <tr>
      <td>伯尔尼</td>
      <td>BE</td>
      <td>16380</td>
      <td>178.6</td>
      <td>75.0</td>
    </tr>
    <tr>
      <td>巴塞尔乡村</td>
      <td>BL</td>
      <td>4962</td>
      <td>178.8</td>
      <td>75.5</td>
    </tr>
    <tr>
      <td>巴塞尔城市</td>
      <td>BS</td>
      <td>2039</td>
      <td>179.1</td>
      <td>77.0</td>
    </tr>
    <tr>
      <td>弗里堡</td>
      <td>FR</td>
      <td>5558</td>
      <td>178.3</td>
      <td>74.3</td>
    </tr>
    <tr>
      <td>日内瓦</td>
      <td>GE</td>
      <td>7370</td>
      <td>177.8</td>
      <td>73.8</td>
    </tr>
    <tr>
      <td>格拉鲁斯</td>
      <td>GL</td>
      <td>643</td>
      <td>178.0</td>
      <td>74.0</td>
    </tr>
    <tr>
      <td>格劳宾登</td>
      <td>GR</td>
      <td>3318</td>
      <td>178.9</td>
      <td>73.4</td>
    </tr>
    <tr>
      <td>汝拉</td>
      <td>JU</td>
      <td>1416</td>
      <td>177.6</td>
      <td>73.1</td>
    </tr>
    <tr>
      <td>卢塞恩</td>
      <td>LU</td>
      <td>8009</td>
      <td>178.6</td>
      <td>74.5</td>
    </tr>
    <tr>
      <td>纳沙泰尔</td>
      <td>NE</td>
      <td>2700</td>
      <td>177.8</td>
      <td>73.1</td>
    </tr>
    <tr>
      <td>下瓦尔登</td>
      <td>NW</td>
      <td>799</td>
      <td>178.8</td>
      <td>73.8</td>
    </tr>
    <tr>
      <td>上瓦尔登</td>
      <td>OW</td>
      <td>718</td>
      <td>178.5</td>
      <td>73.7</td>
    </tr>
    <tr>
      <td>圣加仑</td>
      <td>SG</td>
      <td>8888</td>
      <td>178.5</td>
      <td>74.2</td>
    </tr>
    <tr>
      <td>沙夫豪森</td>
      <td>SH</td>
      <td>1228</td>
      <td>178.9</td>
      <td>75.9</td>
    </tr>
    <tr>
      <td>索洛图恩</td>
      <td>SO</td>
      <td>4588</td>
      <td>178.3</td>
      <td>75.0</td>
    </tr>
    <tr>
      <td>施维茨</td>
      <td>SZ</td>
      <td>2636</td>
      <td>179.3</td>
      <td>75.3</td>
    </tr>
    <tr>
      <td>图尔高</td>
      <td>TG</td>
      <td>5120</td>
      <td>178.4</td>
      <td>74.6</td>
    </tr>
    <tr>
      <td>提契诺</td>
      <td>TI</td>
      <td>5712</td>
      <td>177.5</td>
      <td>73.4</td>
    </tr>
    <tr>
      <td>乌里</td>
      <td>UR</td>
      <td>828</td>
      <td>178.0</td>
      <td>72.6</td>
    </tr>
    <tr>
      <td>沃</td>
      <td>VD</td>
      <td>10709</td>
      <td>178.3</td>
      <td>73.7</td>
    </tr>
    <tr>
      <td>瓦莱</td>
      <td>VS</td>
      <td>5100</td>
      <td>178.1</td>
      <td>73.0</td>
    </tr>
    <tr>
      <td>楚格</td>
      <td>ZG</td>
      <td>1916</td>
      <td>179.2</td>
      <td>74.2</td>
    </tr>
    <tr>
      <td>苏黎世</td>
      <td>ZH</td>
      <td>22147</td>
      <td>178.6</td>
      <td>74.8</td>
    </tr>
  </tbody>
</table>

## 身高与语言的关系

我们从瑞士联邦统计局（Bundesamt für Statistik, BFS）的网站上获取了2019年瑞士各州的主要[语言分布数据](https://www.bfs.admin.ch/bfs/de/home/statistiken/kataloge-datenbanken/tabellen.assetdetail.15384573.html)，选取并计算了永久居留人口中15—24岁年龄层说各种语言的人口比例，按照下面的统计模型进行了回归分析。

\\[h_c = k_l p_{c, l} + b_l + \varepsilon_c\\]

我们取\\(l = \text{德语、法语、意大利语}\\)，分三次进行统计回归。变量\\(c\\)为瑞士的26个州，\\(h_c\\)为各州2016—2020年的男性平均身高，而\\(\varepsilon_c\\)为统计模型中各州的残差，\\(p_{c, l}\\)为\\(c\\)州15—24岁人群中\\(l\\)作为主要语言的比例，\\(k_l, b_l\\)为该线性模型中身高与语言比例的相关系数和截距。

我们使用各州的样本量进行加权回归，结果如下。我们发现瑞士各州平均身高与该州的德语人口比例呈显著的正相关关系，而与法语、意大利语人口比例呈显著的负相关关系。

<table>
  <caption>
    瑞士各州平均身高与语言比例的回归分析（一）
  </caption>
  <thead>
    <tr>
      <th rowspan="2">分析的语言\(l\)</th>
      <th colspan="4">截距\(b_l\)</th>
      <th colspan="5">相关系数\(k_l\)</th>
    </tr>
    <tr>
      <th>系数</th>
      <th>标准误差</th>
      <th>\(\text{P}_{2.5}\)</th>
      <th>\(\text{P}_{97.5}\)</th>
      <th>系数</th>
      <th>标准误差</th>
      <th>\(\text{P}_{2.5}\)</th>
      <th>\(\text{P}_{97.5}\)</th>
      <th>\(t\)检验的\(p\)值</th>
    </tr>
  </thead>
  <tbody>
  	<tr>
  	  <td>德语</td>
  	  <td>177.9457</td>
  	  <td>0.107</td>
  	  <td>177.725</td>
  	  <td>178.166</td>
  	  <td>0.7575</td>
  	  <td>0.138</td>
  	  <td>0.473</td>
  	  <td>1.042</td>
  	  <td>0.000*</td>
  	</tr>
   	<tr>
  	  <td>法语</td>
  	  <td>178.5922</td>
  	  <td>0.079</td>
  	  <td>178.430</td>
  	  <td>178.754</td>
  	  <td>-0.5758</td>
  	  <td>0.183</td>
  	  <td>-0.953</td>
  	  <td>-0.199</td>
  	  <td>0.004*</td>
  	</tr>
   	<tr>
  	  <td>意大利语</td>
  	  <td>178.5276</td>
  	  <td>0.072</td>
  	  <td>178.379</td>
  	  <td>178.677</td>
  	  <td>-1.0231</td>
  	  <td>0.360</td>
  	  <td>-1.767</td>
  	  <td>-0.279</td>
  	  <td>0.009*</td>
  	</tr>
  </tbody>
</table>

基于以上结论，我们建立如下的新统计模型

\\[h_c = k^\prime_{\text{法语}} p_{c, \text{法语}} + k^\prime_{\text{意大利语}} p_{c, \text{意大利语}}  + b^\prime + \varepsilon^\prime_c\\]

以非法语、非意大利语人口为基准（绝大部分为德语人口），进行二元统计回归，得到的结果如下。

<table>
  <caption>
    瑞士各州平均身高与语言比例的回归分析（二）
  </caption>
  <thead>
    <tr>
      <th>变量</th>
      <th>系数</th>
      <th>标准误差</th>
      <th>\(\text{P}_{2.5}\)</th>
      <th>\(\text{P}_{97.5}\)</th>
      <th>\(t\)检验的\(p\)值</th>
    </tr>
  </thead>
  <tbody>
  	<tr>
  	  <td>\(k^\prime_\text{法语}\)</td>
  	  <td>-0.6679</td>
  	  <td>0.135</td>
  	  <td>-0.947</td>
  	  <td>-0.388</td>
  	  <td>0.000*</td>
  	</tr>
  	<tr>
  	  <td>\(k^\prime_\text{意大利语}\)</td>
  	  <td>-1.2100</td>
  	  <td>0.259</td>
  	  <td>-1.746</td>
  	  <td>-0.674</td>
  	  <td>0.000*</td>
  	</tr>
  	<tr>
  	  <td>\(b^\prime\)</td>
  	  <td>178.6972</td>
  	  <td>0.062</td>
  	  <td>178.569</td>
  	  <td>178.825</td>
  	  <td>-</td>
  	</tr>
  </tbody>
</table>

### 小结

- 瑞士非法语、非意大利语的男性人口平均身高为178.70 ± 0.06 cm（基准值）
- 法语人口的平均身高低于基准值0.66 ± 0.13 cm
- 意大利语人口的平均身高低于基准值1.21 ± 0.26 cm。

## 身高与社会经济因素的关系

我们从瑞士联邦统计局的网站是获取了瑞士各州的[人均GDP](https://www.bfs.admin.ch/bfs/de/home/statistiken/kataloge-datenbanken/tabellen.assetdetail.15304855.html)和[最高学历分布](https://www.bfs.admin.ch/bfs/de/home/statistiken/kataloge-datenbanken/tabellen.assetdetail.11607510.html)数据。我们计算下面的变量：

- 2018年各州的人均GDP的对数（以10为底数，以现价瑞士法郎计价）
- 2016年各州最高学历为第三级别（高等学校和高等职业教育）的人口比例

并对各州身高按照各州身高的样本量加权，进行线性回归分析，得到的结果如下。

<table>
  <caption>
    瑞士各州平均身高与社会经济因素的回归分析
  </caption>
  <thead>
    <tr>
      <th>变量</th>
      <th>线性相关系数</th>
      <th>标准误差</th>
      <th>\(\text{P}_{2.5}\)</th>
      <th>\(\text{P}_{97.5}\)</th>
      <th>\(t\)检验的\(p\)值</th>
    </tr>
  </thead>
  <tbody>
  	<tr>
  	  <td>\(\log_{10}\)人均GDP</td>
  	  <td>0.4317</td>
  	  <td>0.759</td>
  	  <td>-0.575</td>
  	  <td>-1.136</td>
  	  <td>0.575</td>
  	</tr>
  	<tr>
  	  <td>高等教育人口比例</td>
  	  <td>0.6461</td>
  	  <td>1.372</td>
  	  <td>-2.186</td>
  	  <td>3.478</td>
  	  <td>0.642</td>
  	</tr>
  </tbody>
</table>

我们发现各州平均身高与该州的人均GDP和高等教育人口比例均有正相关关系，但是这些相关关系并不显著。

### 巴塞尔城乡的身高差异

1833年，瑞士原巴塞尔州分裂成巴塞尔城市州和巴塞尔乡村州。我们比较这两个州兵役体质测试的男性身高，发现城市州高于乡村州。我们按照男性身高标准差为6.5 cm估算平均身高的误差，发现两州身高的差异边缘显著（p = 0.03）。

<table>
  <caption>
    瑞士巴塞尔城市和乡村州男性平均身高
  </caption>
  <thead>
    <tr>
      <th>州</th>
      <th>样本量</th>
      <th>平均身高（cm）</th>
      <th>标准误差（cm）</th>
      <th>高等教育比例</th>
      <th>人均GDP（瑞士法郎）</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>巴塞尔乡村</td>
      <td>4962</td>
      <td>178.84</td>
      <td>0.09</td>
      <td>33%</td>
      <td>72639</td>
    </tr>
    <tr>
      <td>巴塞尔城市</td>
      <td>2039</td>
      <td>179.13</td>
      <td>0.14</td>
      <td>44%</td>
      <td>197491</td>
    </tr>
  </tbody>
</table>

## 结论

瑞士各州的身高与该州的语言比例有着显著的关系：德语人口平均身高更高、法语人口次之、意大利语人口再次之，但与该州的人均GDP和高等教育人口比例仅存在微弱的正相关关系。