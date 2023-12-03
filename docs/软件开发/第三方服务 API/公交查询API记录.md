---
title: 公交查询API记录
date: 2017-1-23 16:02:25
categories: [笔记]
tags: [Android]
---

## 1. 通过公交路线获取公交路线的代号

* 地址：`http://www.xaglkp.com.cn/Bus/GetBusLineByName`


* 调用方式 `GET`
* 请求参数

|     参数      |  类型  |  必选  |  说明  |  示例  |
| :---------: | :--: | :--: | :--: | :--: |
| buslinename | int  | true | 公交线路 | 600  |

## 2. 请求路线详情及公交情况

* 地址：`http://www.xaglkp.com.cn/Bus/GetRealBusLine`
* 调用方式 `POST`
* 请求参数

|             参数             |   类型   |  必须  |   说明    |                    示例                    |
| :------------------------: | :----: | :--: | :-----: | :--------------------------------------: |
| __RequestVerificationToken | String | true | 服务器验证信息 | Jsh5_yuEuscmEVLg0pm_Cw-R-q_zfp0sutbzkbjMCrBoPFB1fhSMIDJsSaE4kIckrjhpnvhL90NWGRsbBXMXsR9BIaZP6ulYXdAg9hTVYT01 |
|          routeid           |  Int   | true | 公交路线代号  |                    1                     |

### 返回参数部分说明

|      参数      |   类型   |        说明        |  示例   |
| :----------: | :----: | :--------------: | :---: |
| TicketSystem |  int   | `1`为可刷卡 `2`为不可刷卡 |   1   |
| running_type | String |  公交去回，同时会用来请求票价  | 197_1 |
|   run_nums   |  int   |     配车数量（辆）      |  44   |
| avg_interval |  int   |    平均发车间隔（分钟）    |   8   |

---

## 3. 查询路线票价以及运营时间

* 地址 `http://www.xaglkp.com.cn/Bus/GetBusLineInfoBySegmentId`
* 调用方式 `GET`
* 请求参数

|    参数     |   类型   |  必须  |             说明             |  示例   |
| :-------: | :----: | :--: | :------------------------: | :---: |
| segmentId | String | true | 由公交详情请求的`running_type`得到的值 | 132_1 |

---

## 4. 根据已经输入的文本联想

- 地址 `http://www.xaglkp.com.cn/Bus/GetBusLineByName`
- 调用方式 `GET`
- 请求参数

|     参数      |  类型  |  必须  |     说明     |  示例  |
| :---------: | :--: | :--: | :--------: | :--: |
| buslinename | Int  | true | 文本框已经输入的路线 |  6   |

---





[欢迎去我另一处博客做客~](http://installbi.me/?p=77)