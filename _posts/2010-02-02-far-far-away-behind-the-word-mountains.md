# push V3版本推送接口—详细版

# 一、集成准备

## 1、获取AppKey和AppSecret

注册应用申请Mob的 AppKey 和 AppSecret，详情可以**[点击查看注册流程](http://bbs.mob.com/forum.php?mod=viewthread&tid=8212&extra=page%3D1)**

## 2、概述

MobPush不仅提供完善的SDK API供开发者APP调用，且提供完全遵循Rest规范的HTTP接口，适用各开发语言环境调用，Mob官网提供不同服务端语言包，请参见**[服务端SDK](http://www.mob.com/wiki/detailed?wiki=serverSDKfeilei&id=136)**。

# 二、配置集成

## 1、在Mob后台填写服务器IP地址绑定，仅绑定的ip才有调用权限

![](http://download.sdk.mob.com/2019/12/26/12/1577335156918/1295_472_59.3.jpg)

# 三、代码调用

## 1 集成说明         

### 1.1、约束规范

前置条件：
需要有接入APPKEY信息（申请MobSDK的appkey流程），开通MobPush产品；
官网MobPush->推送设置填写服务器IP地址绑定，仅绑定的ip才有调用权限；
接口使用RestAPI规范，所有请求接口新增、修改使用POST方法；所有查询接口使用GET方法；
请求Header信息需要增加key和sign两个参数，请求内容参数使用JSON格式，并且使用UTF-8编码。

### 1.2、接口频率限制

MobPush webapi接口频率主要针对每个appkey 在每分钟的访问请求次数限制。

主要有两层请求控制频率限制，推送接口（发送+查询）的接口频率限制，默认500次/分钟；webapi全部接口的请求频率限制，默认800次/分钟。

注：如有更高需求，可联系我们商务 或技术支持调整相关webapi接口频率限制

### 1.3、鉴权

MobPush Rest Api 采用MD5参数签名认证的方式。

避免AppSecret的直接传输；
可对请求参数进行完整性校验，保障数据不可篡改。
基本步骤：

获取Mob官网提供的appkey、App Secret
将请求参数JSON内容生成一个字符串
将上面生成的字符串尾部拼接App Secret值得到新字符串（如果请求参数为空则此步骤得到字符串就是App Seret）
将拼接后的字符串MD5得到签名sign
发送请求的HTTP Header加如下两个键参数：key、 sign。key的值appkey, sign值为如上步骤获取的sign值。

## 2、推送接口

### 2.1、发送推送


接口地址： http://api.push.mob.com/v3/push/createPush

请求方式： POST

请求参数格式： JSON

接口访问频率限制：受限，参照 接口频率限制

#### 请求参数

<table>
   <tr>
      <td>名称</td>
      <td></td>
      <td></td>
      <td>类型</td>
      <td>是否必须</td>
      <td>备注</td>
   </tr>
   <tr>
      <td>workno</td>
      <td></td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>任务id</td>
   </tr>
   <tr>
      <td>source</td>
      <td></td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>枚举值 webapi</td>
   </tr>
   <tr>
      <td>appkey</td>
      <td></td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>appkey</td>
   </tr>
   <tr>
      <td>pushTarget</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>是</td>
      <td>推送目标</td>
   </tr>
   <tr>
      <td></td>
      <td>target</td>
      <td></td>
      <td>number</td>
      <td>是</td>
      <td>目标类型：1广播；2别名；3标签；4regid；5地理位置；6用户分群；9复杂地理位置推送</td>
   </tr>
   <tr>
      <td></td>
      <td>rids</td>
      <td></td>
      <td>string[]</td>
      <td>否</td>
      <td>target:4 => 设置推送Registration Id集合["id1","id2"]</td>
   </tr>
   <tr>
      <td></td>
      <td>tags</td>
      <td></td>
      <td>string []</td>
      <td>否</td>
      <td>target:3 => 设置推送标签集合["tag1","tag2"]</td>
   </tr>
   <tr>
      <td></td>
      <td>tagsType</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>target:3 => 标签组合方式：1并集；2交集；3补集(3暂不考虑)</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>labelIds</td>
      <td>string []</td>
      <td>是</td>
      <td>标签列表</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>mobId</td>
      <td>string</td>
      <td>是</td>
      <td>mobId</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>type</td>
      <td>string</td>
      <td>是</td>
      <td>标签搭配方式, 1:与, 2:或, 3:非, 默认2</td>
   </tr>
   <tr>
      <td></td>
      <td>alias</td>
      <td></td>
      <td>string []</td>
      <td>否</td>
      <td>target:2 => 设置推送别名集合["alias1","alias2"]</td>
   </tr>
   <tr>
      <td></td>
      <td>block</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:6 => 用户分群ID</td>
   </tr>
   <tr>
      <td></td>
      <td>city</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:5 => 推送地理位置 城市，地理位置推送时，city, province, country 是有一个不为空</td>
   </tr>
   <tr>
      <td></td>
      <td>province</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:5 => 推送地理位置 省份，地理位置推送时，city, province, country 是有一个不为空</td>
   </tr>
   <tr>
      <td></td>
      <td>country</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:5 => 推送地理位置 国家，地理位置推送时，city, province, country 是有一个不为空</td>
   </tr>
   <tr>
      <td>pushNotify</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>是</td>
      <td>推送展示细节配置</td>
   </tr>
   <tr>
      <td></td>
      <td>plats</td>
      <td></td>
      <td>number []</td>
      <td>是</td>
      <td>1 android;2 ios </td>
   </tr>
   <tr>
      <td></td>
      <td>iosProduction</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>plat = 2下，0测试环境，1生产环境，默认1</td>
   </tr>
   <tr>
      <td></td>
      <td>offlineSeconds</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>离线消息保存时间，默认0</td>
   </tr>
   <tr>
      <td></td>
      <td>content</td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>推送内容</td>
   </tr>
   <tr>
      <td></td>
      <td>title</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>如果不设置，则默认的通知标题为应用的名称。</td>
   </tr>
   <tr>
      <td></td>
      <td>type</td>
      <td></td>
      <td>number</td>
      <td>是</td>
      <td>推送类型：1通知；2自定义</td>
   </tr>
   <tr>
      <td></td>
      <td>androidNotify</td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>android通知消息, type=1, android</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>content</td>
      <td>string []</td>
      <td>否</td>
      <td>推送内容</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>style</td>
      <td>number</td>
      <td>是</td>
      <td>显示样式标识 0,"普通通知"，1,"BigTextStyle通知，点击后显示大段文字内容"，2,"BigPictureStyle，大图模式"，3,"横幅通知"，默认：0</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>warn</td>
      <td>string</td>
      <td>否</td>
      <td>提醒类型： 1提示音；2震动；3指示灯，如果多个组合则对应编号组合如：12 标识提示音+震动</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>sound</td>
      <td>string</td>
      <td>否</td>
      <td>自定义声音</td>
   </tr>
   <tr>
      <td></td>
      <td>iosNotify</td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>ios通知消息, type=1, ios</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>badge</td>
      <td>number</td>
      <td>否</td>
      <td>角标</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>badgeType</td>
      <td>number</td>
      <td>否</td>
      <td>badge类型, 1:绝对值 不能为负数，2增减(正数增加，负数减少)，减到0以下会设置为0</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>category</td>
      <td>string</td>
      <td>否</td>
      <td>apns的category字段，只有IOS8及以上系统才支持此参数推送</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>sound</td>
      <td>string</td>
      <td>否</td>
      <td>APNs通知，通过这个字段指定声音。默认为default，即系统默认声音。 如果设置为空值，则为静音。如果设置为特殊的名称，则需要你的App里配置了该声音才可以正常。</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>subtitle</td>
      <td>string</td>
      <td>否</td>
      <td>副标题</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>slientPush</td>
      <td>number</td>
      <td>否</td>
      <td>如果只携带content-available: 1,不携带任何badge，sound 和消息内容等参数， 则可以不打扰用户的情况下进行内容更新等操作即为“Silent Remote Notifications”</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>contentAvailable</td>
      <td>number</td>
      <td>否</td>
      <td>将该键设为 1 则表示有新的可用内容。带上这个键值，意味着你的 App 在后台启动了或恢复运行了，application:didReceiveRemoteNotification:fetchCompletionHandler:被调用了</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>mutableContent</td>
      <td>number</td>
      <td>否</td>
      <td>需要在附加字段中配置相应参数</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>attachmentType</td>
      <td>number</td>
      <td>否</td>
      <td>ios富文本 0 无 ； 1 图片 ；2 视频 ；3 音频</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>attachment</td>
      <td>string</td>
      <td>否</td>
      <td>ios富文本内容</td>
   </tr>
   <tr>
      <td></td>
      <td>taskCron</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>是否是定时任务：0否，1是，默认0</td>
   </tr>
   <tr>
      <td></td>
      <td>taskTime</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>定时消息 发送时间， taskCron=1时必填</td>
   </tr>
   <tr>
      <td></td>
      <td>customNotify</td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>自定义内容, type=2</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>customType</td>
      <td>string</td>
      <td>否</td>
      <td>自定义消息类型：text 文本消息</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>customTitle</td>
      <td>string</td>
      <td>否</td>
      <td>自定义类型标题</td>
   </tr>
   <tr>
      <td></td>
      <td>extrasMapList</td>
      <td></td>
      <td>object []</td>
      <td>否</td>
      <td>JSON格式 保留字段</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>key</td>
      <td>string</td>
      <td>是</td>
      <td>键</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>value</td>
      <td>string</td>
      <td>是</td>
      <td>值</td>
   </tr>
   <tr>
      <td>pushOperator</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>运营保障相关配置, 默认有个空对象</td>
   </tr>
   <tr>
      <td></td>
      <td>dropType</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>运营保障 消息 修改类型： 1 取消 2 替换 3 撤回</td>
   </tr>
   <tr>
      <td></td>
      <td>dropId</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>撤回的消息的id</td>
   </tr>
   <tr>
      <td></td>
      <td>notifyId</td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>给每条消息设置唯一的消息id 有替换和撤回 1 根据workId 查询到 原来的消息id 2 如果是替换 直接 重置 notifyId 3 如果是撤回 设置dropId 为 notifyId 通知客户端去撤回那条消息</td>
   </tr>
   <tr>
      <td></td>
      <td>template</td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>展示类型, 细节待补充</td>
   </tr>
   <tr>
      <td>pushForward</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>link 相关打开配置</td>
   </tr>
   <tr>
      <td></td>
      <td>url</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>1 link跳转 moblink功能的的uri</td>
   </tr>
   <tr>
      <td></td>
      <td>scheme</td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>2 scheme moblink功能的的scheme</td>
   </tr>
   <tr>
      <td></td>
      <td>schemeDataList</td>
      <td></td>
      <td>object []</td>
      <td>否</td>
      <td>schema参数</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>key</td>
      <td>string</td>
      <td>是</td>
      <td>键</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>value</td>
      <td>string</td>
      <td>是</td>
      <td>值</td>
   </tr>
   <tr>
      <td></td>
      <td> nextType</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>0 打开首页 1 link跳转 2 scheme 跳转</td>
   </tr>
   <tr>
      <td>repate</td>
      <td></td>
      <td></td>
      <td>boolean</td>
      <td>否</td>
      <td>是否重复推送</td>
   </tr>
   <tr>
      <td>parentId</td>
      <td></td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>repate 重复记录原始ID</td>
   </tr>
   <tr>
      <td>isLocal</td>
      <td></td>
      <td></td>
      <td>boolean</td>
      <td>否</td>
      <td>isLocal:是否本地消息</td>
   </tr>
   <tr>
      <td>groupId</td>
      <td></td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>groupId: AB分组测试ID</td>
   </tr>
   <tr>
      <td></td>
   </tr>
</table>

###  **pushTarget ：推送目标**

推送设备对象，表示一条推送可以被推送到哪些设备列表。MobPush的推送的方式支持：广播（全部设备）,别名,标签，regid等。

<table>
   <tr>
      <td>名称</td>
      <td></td>
      <td></td>
      <td>类型</td>
      <td>是否必须</td>
      <td>备注</td>
   </tr>
   <tr>
      <td>pushTarget</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>是</td>
      <td>推送目标</td>
   </tr>
   <tr>
      <td></td>
      <td>target</td>
      <td></td>
      <td>number</td>
      <td>是</td>
      <td>目标类型：1广播；2别名；3标签；4regid；5地理位置；6用户分群；9复杂地理位置推送</td>
   </tr>
   <tr>
      <td></td>
      <td>rids</td>
      <td></td>
      <td>string[]</td>
      <td>否</td>
      <td>target:4 => 设置推送Registration Id集合["id1","id2"]</td>
   </tr>
   <tr>
      <td></td>
      <td>tags</td>
      <td></td>
      <td>string []</td>
      <td>否</td>
      <td>target:3 => 设置推送标签集合["tag1","tag2"]</td>
   </tr>
   <tr>
      <td></td>
      <td>tagsType</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>target:3 => 标签组合方式：1并集；2交集；3补集(3暂不考虑)</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>labelIds</td>
      <td>string []</td>
      <td>是</td>
      <td>标签列表</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>mobId</td>
      <td>string</td>
      <td>是</td>
      <td>mobId</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>type</td>
      <td>string</td>
      <td>是</td>
      <td>标签搭配方式, 1:与, 2:或, 3:非, 默认2</td>
   </tr>
   <tr>
      <td></td>
      <td>alias</td>
      <td></td>
      <td>string []</td>
      <td>否</td>
      <td>target:2 => 设置推送别名集合["alias1","alias2"]</td>
   </tr>
   <tr>
      <td></td>
      <td>block</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:6 => 用户分群ID</td>
   </tr>
   <tr>
      <td></td>
      <td>city</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:5 => 推送地理位置 城市，地理位置推送时，city, province, country 是有一个不为空</td>
   </tr>
   <tr>
      <td></td>
      <td>province</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:5 => 推送地理位置 省份，地理位置推送时，city, province, country 是有一个不为空</td>
   </tr>
   <tr>
      <td></td>
      <td>country</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>target:5 => 推送地理位置 国家，地理位置推送时，city, province, country 是有一个不为空</td>
   </tr>
</table>


### 示例

- 推送给全部（广播）：

``` json
{
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送的内容",
        "type":1
    }
}
```

- 推送给标签

``` json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":3,
        "tags":[
            "男",
            "上海",
            "老师"
        ]
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送的内容",
        "type":1
    }
}
```

- 推送给别名

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":2,
        "alias":[
            "alias_1",
            "alias_2"
        ]
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送的内容",
        "type":1
    }
}
```

- 推送给regid


```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":4,
        "rids":[
            "c262bac10d05ec1c9b04126d"
        ]
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送的内容",
        "type":1
    }
}
```

**pushNotify：推送展示细节配置**

可以设置Android和Ios平台通知的显示样式以及通知的内容。例如可以设置通知是大图或者是横幅通知的样式，可以设置是否自定义通知的声音，以及推送的类型是通知或者是自定义消息（透传消息），可以设置离线消息的时间。

<table>
   <tr>
      <td>名称</td>
      <td></td>
      <td></td>
      <td>类型</td>
      <td>是否必须</td>
      <td>备注</td>
   </tr>
   <tr>
      <td>pushNotify</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>是</td>
      <td>推送展示细节配置</td>
   </tr>
   <tr>
      <td></td>
      <td>plats</td>
      <td></td>
      <td>number []</td>
      <td>是</td>
      <td>1 android;2 ios </td>
   </tr>
   <tr>
      <td></td>
      <td>iosProduction</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>plat = 2下，0测试环境，1生产环境，默认1</td>
   </tr>
   <tr>
      <td></td>
      <td>offlineSeconds</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>离线消息保存时间，默认0</td>
   </tr>
   <tr>
      <td></td>
      <td>content</td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>推送内容</td>
   </tr>
   <tr>
      <td></td>
      <td>title</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>如果不设置，则默认的通知标题为应用的名称。</td>
   </tr>
   <tr>
      <td></td>
      <td>type</td>
      <td></td>
      <td>number</td>
      <td>是</td>
      <td>推送类型：1通知；2自定义</td>
   </tr>
   <tr>
      <td></td>
      <td>androidNotify</td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>android通知消息, type=1, android</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>content</td>
      <td>string []</td>
      <td>否</td>
      <td>推送内容</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>style</td>
      <td>number</td>
      <td>是</td>
      <td>显示样式标识 0,"普通通知"，1,"BigTextStyle通知，点击后显示大段文字内容"，2,"BigPictureStyle，大图模式"，3,"横幅通知"，默认：0</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>warn</td>
      <td>string</td>
      <td>否</td>
      <td>提醒类型： 1提示音；2震动；3指示灯，如果多个组合则对应编号组合如：12 标识提示音+震动</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>sound</td>
      <td>string</td>
      <td>否</td>
      <td>自定义声音</td>
   </tr>
   <tr>
      <td></td>
      <td>iosNotify</td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>ios通知消息, type=1, ios</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>badge</td>
      <td>number</td>
      <td>否</td>
      <td>角标</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>badgeType</td>
      <td>number</td>
      <td>否</td>
      <td>badge类型, 1:绝对值 不能为负数，2增减(正数增加，负数减少)，减到0以下会设置为0</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>category</td>
      <td>string</td>
      <td>否</td>
      <td>apns的category字段，只有IOS8及以上系统才支持此参数推送</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>sound</td>
      <td>string</td>
      <td>否</td>
      <td>APNs通知，通过这个字段指定声音。默认为default，即系统默认声音。 如果设置为空值，则为静音。如果设置为特殊的名称，则需要你的App里配置了该声音才可以正常。</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>subtitle</td>
      <td>string</td>
      <td>否</td>
      <td>副标题</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>slientPush</td>
      <td>number</td>
      <td>否</td>
      <td>如果只携带content-available: 1,不携带任何badge，sound 和消息内容等参数， 则可以不打扰用户的情况下进行内容更新等操作即为“Silent Remote Notifications”</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>contentAvailable</td>
      <td>number</td>
      <td>否</td>
      <td>将该键设为 1 则表示有新的可用内容。带上这个键值，意味着你的 App 在后台启动了或恢复运行了，application:didReceiveRemoteNotification:fetchCompletionHandler:被调用了</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>mutableContent</td>
      <td>number</td>
      <td>否</td>
      <td>需要在附加字段中配置相应参数</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>attachmentType</td>
      <td>number</td>
      <td>否</td>
      <td>ios富文本 0 无 ； 1 图片 ；2 视频 ；3 音频</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>attachment</td>
      <td>string</td>
      <td>否</td>
      <td>ios富文本内容</td>
   </tr>
   <tr>
      <td></td>
      <td>taskCron</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>是否是定时任务：0否，1是，默认0</td>
   </tr>
   <tr>
      <td></td>
      <td>taskTime</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>定时消息 发送时间， taskCron=1时必填</td>
   </tr>
   <tr>
      <td></td>
      <td>customNotify</td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>自定义内容, type=2</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>customType</td>
      <td>string</td>
      <td>否</td>
      <td>自定义消息类型：text 文本消息</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>customTitle</td>
      <td>string</td>
      <td>否</td>
      <td>自定义类型标题</td>
   </tr>
   <tr>
      <td></td>
      <td>extrasMapList</td>
      <td></td>
      <td>object []</td>
      <td>否</td>
      <td>JSON格式 保留字段</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>key</td>
      <td>string</td>
      <td>是</td>
      <td>键</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>value</td>
      <td>string</td>
      <td>是</td>
      <td>值</td>
   </tr>
</table>

- 自定义消息（透传消息）

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":2,
        "customNotify":{
            "customType":"text 文本消息",
            "customTitle":"自定义类型标题"
        }
    }
}
```

- Android通知大图模式

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":1,
        "androidNotify":{
            "content":[
                "Android推送内容1",
                "Android推送内容2"
            ],
            "style":2
        }
    }
}
```

- Android通知横幅通知模式

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":1,
        "androidNotify":{
            "content":[
                "Android推送内容1",
                "Android推送内容2"
            ],
            "style":3
        }
    }
}
```

- Android通知自定义声音：音频文件放到项目res/raw目录下，只需传音频文件的文件名

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":1,
        "androidNotify":{
            "content":[
                "Android推送内容1",
                "Android推送内容2"
            ],
            "style":2,
            "warn":"1",
            "sound":"warn"
        }
    }
}
```

**pushForward：页面跳转相关配置**

push支持三种页面跳转方式：首页（默认），打开网页，应用内跳转（跳转指定页面）

<table>
   <tr>
      <td>名称</td>
      <td></td>
      <td></td>
      <td>类型</td>
      <td>是否必须</td>
      <td>备注</td>
   </tr>
      <td>pushForward</td>
      <td></td>
      <td></td>
      <td>object</td>
      <td>否</td>
      <td>link 相关打开配置</td>
   </tr>
   <tr>
      <td></td>
      <td>url</td>
      <td></td>
      <td>string</td>
      <td>否</td>
      <td>1 link跳转 moblink功能的的uri</td>
   </tr>
   <tr>
      <td></td>
      <td>scheme</td>
      <td></td>
      <td>string</td>
      <td>是</td>
      <td>2 scheme moblink功能的的scheme</td>
   </tr>
   <tr>
      <td></td>
      <td>schemeDataList</td>
      <td></td>
      <td>object []</td>
      <td>否</td>
      <td>schema参数</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>key</td>
      <td>string</td>
      <td>是</td>
      <td>键</td>
   </tr>
   <tr>
      <td></td>
      <td></td>
      <td>value</td>
      <td>string</td>
      <td>是</td>
      <td>值</td>
   </tr>
   <tr>
      <td></td>
      <td> nextType</td>
      <td></td>
      <td>number</td>
      <td>否</td>
      <td>0 打开首页 1 link跳转 2 scheme 跳转</td>
   </tr>
</table>

- 跳转首页并传递附加参数

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":1,
        "androidNotify":{
            "content":[
                "Android推送内容1",
                "Android推送内容2"
            ],
            "style":2,
            "warn":"1",
            "sound":"warn"
        },
        "extrasMapList":[
            {
                "key":"extrakey",
                "value":"extravalue"
            }
        ]
    },
    "pushForward":{
        "nextType":0
    }
}
```

- 跳转到指定界面并且传递携带scheme数据

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":1,
        "androidNotify":{
            "content":[
                "Android推送内容1",
                "Android推送内容2"
            ],
            "style":2,
            "warn":"1",
            "sound":"warn"
        }
    },
    "pushForward":{
        "nextType":2,
        "scheme":"mlink://com.mob.mobpush.linkone",
        "schemeDataList":[
            {
                "key":"schemekey1",
                "value":"schemevalue1"
            }
        ]
    }
}
```

- 打开网页

```json
{
    "source":"webapi",
    "appkey":"moba6b6c6d6",
    "pushTarget":{
        "target":1
    },
    "pushNotify":{
        "plats":[
            1
        ],
        "content":"推送内容",
        "type":1,
        "androidNotify":{
            "content":[
                "Android推送内容1",
                "Android推送内容2"
            ],
            "style":2,
            "warn":"1",
            "sound":"warn"
        }
    },
    "pushForward":{
        "nextType":1,
        "url":"http://www.mob.com"
    }
}
```

