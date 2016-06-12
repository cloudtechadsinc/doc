# 投放机接口设计及流程

| 撰写人  | 更新时间       | 动作描述 |
| ---- | ---------- | ---- |
| 段晶璟  | 2016年6月12日 | 新建   |



# 实现功能

## 1.     支持创意类型

a)      图片

b)      视频

c)       Html模版

## 2.     支持落地类型

a)      应用下载

b)      外部浏览器开启页面

c)       内部浏览器开启页面

d)      深度链接

e)      订阅

## 3.     支持两段创意

a)      前端创意包含Html模版，图片

b)      后端创意包含 Html模版，图片，视频



# 处理流程

 ![图](图.png)



# 请求参数

| **字段名**      | **类型** | **是否必填** | **字段含义**                                 |
| :----------- | :----- | :------- | :--------------------------------------- |
| token        | 字符串    | 必填       | 广告位标识                                    |
| gaid         | 字符串    | 必填       | Google  Advertising Id                   |
| aid          | 字符串    | 选填       | 设备  Android ID                           |
| imei         | 字符串    | 选填       | 设备  IMEI                                 |
| creativetype | 字符串    | 必填       | 需要的素材类型  img:图片素材  html:H5代码  eg:  creativetype =img,html 返回图片素材和 H5 代码   creativetype=img 仅返回图片 |
| os           | 字符串    | 必填       | 操作系统  可选值：android,ios                    |
| osv          | 浮点     | 必填       | Android：Build.VERSION.SDK  iOS：[[[UIDevice currentDevice] systemVersion] floatValue]; |
| dt           | 字符串    | 必填       | 设备类型  可选值：phone,tablet,ipad,watch        |
| ip           | 字符串    | 选填       | 当前用户IP                                   |
| icc          | 字符串    | 必填       | ISO  country code SIM卡国家代码  Android：TelephonyManager.getNetworkCountryIso()  iOS：  [[NSLocale  currentLocale] objectForKey:NSLocaleCountryCode]; |
| nt           | 整形     | 必填       | 网络类型  Android：NetworkInfo.getType()  iOS：[[dataNetWorkItemView valueForKey:@"dataNetworkType"]  integerValue] |
| adnum        | 整形     | 必填       | 请求广告数量，默认为1                              |
| gp           | 整形     | 选填       | 1:已安装google play  2:未安装google play       |
| Img_rule     | 整形     | 选填       | 图片素材匹配的规则:  1:宽高绝对值完全匹配  -比如宽高填写 1200*627,只返回 1200*627 尺寸的图片。 2:宽高比完全匹配,宽高数值最多向下浮动 20%  -比如宽高填写 1200*627,只要是 1.91 : 1,宽大于 1200*0.8,高 大于 627*0.8 的图片都可以返回  3:宽高比最多上下浮动 0.1,宽高数值最多向下浮动 20% -比如宽高填写 1200*627,只要宽高比在{1.81:1 - 2.01:1}之间,宽高 大于 1200*0.8 x 627*0.8 的图片都可以返回  **缺省值默认是 3** |
| imgw         | 整形     | 选填       | 仅当  creativetype=img 时生效。表示需要的图片素材的宽(单位像 素)  缺省值为  slot 的宽 |
| imgh         | 整形     | 选填       | 仅当  creativetype=img 时生效。表示需要的图片素材的高(单位像 素)  缺省值为  slot 的高 |
| category     | 字符串    | 选填       | 应用分类，参加附录（SSP）                           |
| dmf          | 字符串    | 选填       | 设备制造厂商                                   |
| dml          | 字符串    | 选填       | 设备型号                                     |
| dpd          | 字符串    | 选填       |                                          |
| so           | 整形     | 选填       | 0：未知，1表示竖屏，2表示横屏                         |
| ds           | 浮点数    | 选填       | 屏幕密度                                     |
| mcc          | 字符串    | 选填       | MCC  Code                                |
| mnc          | 字符串    | 选填       | MNC  Code                                |
| cn           | 字符串    | 选填       | Carrier  name 运营商名称                      |
| la           | 浮点数    | 选填       | 纬度                                       |
| lo           | 浮点数    | 选填       | 经度                                       |
| tz           | 字符串    | 选填       | 时区                                       |
| pn           | 字符串    | 必填       | 当前宿主包名                                   |
| lang         | 字符串    | 选填       | 当前系统语言                                   |
| sv           | 字符串    | 必填       | SDK版本号                                   |

 

# 返回数据

## 外层JSON对象

| **字段名**        | **类型** | **是否一定有值** | **字段含义**                                 |
| -------------- | ------ | ---------- | ---------------------------------------- |
| ad_list        | 数组     | 是          | 广告数组                                     |
| adid           | 整形     | 是          | 广告id                                     |
| impid          | 整形     | 是          | 曝光id                                     |
| landing_type   | 字符串    | 是          | 0：应用下载  1：外开落地页  2：内开落地页  3：订阅  4：DeepLink |
| app_download   | 对象     | 否          | title：推广应用标题  description：推广应用说明  app_pkg_nane：推广应用包名  size：应用大小  rate：应用评分  download：下载数  review：评论数  icon_url：图标url  itl_tk_url：安装监测  act_tk_url：激活监测 |
| deeplink       | 对象     | 否          | dlsucc_tk_url：成功跳转  dlfail_tk_url：失败跳转   |
| ad_expire_time | 整形     | 否          | 广告过期时间，单位秒                               |
| clk_url        | 字符串    | 是          | 点击链接，不同情况下处理不同                           |
| pre_creative   | 对象     | 否          | 前置创意对象组                                  |
| bak_creative   | 对象     | 是          | 后置创意对象组                                  |

 

## 前置创意JSON对象

| **字段名**        | **类型** | **是否一定有值** | **字段含义**                         |
| -------------- | ------ | ---------- | -------------------------------- |
| create_type    | 整形     | 是          | -1：表示没有此创意节点  0：图片  1：H5         |
| img            | 对象     | 否          | img_url：图片地址  imgw：图片宽  imgh：图片高 |
| html           | 字符串    | 否          | H5模版字符串                          |
| pre_imp_tk_url | 字符串    | 否          | 前置曝光监测链接                         |
| pre_clk_tk_url | 字符串    | 否          | 前置点击监测链接                         |

 

## 后置创意JSON对象

| **字段名**        | **类型** | **是否一定有值** | **字段含义**                                 |
| -------------- | ------ | ---------- | ---------------------------------------- |
| create_type    | 整形     | 是          | -1：表示没有此创意节点  0：图片  1：H5  2：视频           |
| img            | 对象     | 否          | img_url：图片地址  imgw：图片宽  imgh：图片高         |
| html           | 字符串    | 否          | H5模版字符串                                  |
| video          | 对象     | 否          | video_url：视频播放地址  videow：视频高  videoh：视频宽 |
| bak_imp_tk_url | 字符串    | 否          | 前置曝光监测链接                                 |
| bak_clk_tk_url | 字符串    | 否          | 前置点击监测链接                                 |

 

## JSON样本

{

  "ad_list": [

    {

      "adid": 1000,

      "impid": 1000,

      "landing_type": 0,

      "app_download": {

        "title": "AppName",

        "description": "App Description",

        "app_pkg_nane": "com.xxx.xxx",

        "size": 51,

        "rate": 4.2,

        "download": 1000,

        "review": 10000,

        "icon_url": "icon image url",

        "itl_tk_url": "Install tracking url",

        "act_tk_url": "Active track url"

      },

      "deeplink": {

        "dlsucc_tk_url": "success open web track url",

        "dlfail_tk_url": "fail open web track url"

      },

      "ad_expire_time": 10000,

      "clk_url": "click url",

      "pre_creative": {

        "create_type": 0,

        "img": {

          "img_url": "image url",

          "imgw": "image width",

          "imgh": "image height"

        },

        "html": "html code",

        "pre_clk_tk_url": "pre creative click tk url",

        "pre_imp_tk_url": "pre creative impression tk url"

      },

      "bak_creative": {

        "create_type": 0,

        "img": {

          "img_url": "image url",

          "imgw": "image width",

          "imgh": "image height"

        },

        "html": "html code",

        "video": {

          "video_url": "Video url",

          "videow": "Video width",

          "videoh": "Video height"

        },

        "bak_imp_tk_url": "Impression tracking url",

        "bak_clk_tk_url": "Click tracking url"

      }

    }

  ]

}

 

 

 
