文件修订记录



|变更版本|修订日期|原因与修改情况描述|位置|修订人|审核人|
| - | - | - | - | - | - |
|V1.0|2017\.02.23|根据蜂巢系统 API 制定本文 档|全部|田延杰 李鲁艳|刘绪峰|
|V1.1|2017\.03.21|增加移动办公使用的接口|4\.6 4.7 4.8 6.5|田延杰 李鲁艳|刘绪峰|
|V1.2|2017\.05.25|<p>增加 CAS、SMS相关接口</p><p>user 返回数据增加 partOrgs 信息</p>|4\.8 4.7|高健 李鲁艳|刘绪峰|
|V1.3|2017\.06.07|<p>增加 group 接口</p><p>User 返 回 数 据 增 properties 信息</p><p>加</p>|4\.1 4.2 4.4|张开会 李鲁艳|刘绪峰|
|V1.4|2017\.08.23|增加生成子票据和票据注销 接口|4\.8.2 4.8.3|高健|田延杰|
|V1.5|2017\.09.27|增加和修改代办中心接口|4\.6|刘璐|田延杰|
|V1.6|2017\.12.26|修改单点登录 cas 接口失败 返回信息|4\.8|高健|田延杰|
|V1.7|2018\.01.19|<p>增加接口定义 InterfaceDef 相关接口；</p><p>增加 Application</p><p>中与接口</p><p>相关的接口</p>|4\.9.1~4.9.6 4.4.10~4.4.13|刘璐|田延杰|
|V1.8|2018\.01.25|增加查询未设置 username 的 用户信息接口|4\.1.49|刘璐|田延杰|
|V1.9|2018\.03.16|增加 mail 接口，修改 sms 接 口|4\.10.1、4.7.1|刘璐|田延杰|
|V1.10|2018\.05.09|<p>修改 4.8.1 票据授权接口返 回 400 时的返回内容。</p><p>增加 4.1.50 解锁用户接口</p>|4\.8.1、4.1.50|刘璐|田延杰|
|V1.11|2018\.08.27|增加 4.1.51 修改密码（不需 要原密码）接口|4\.1.51|刘璐|田延杰|
|V1.12|2018\.10.10|修改 2.7 状态码|2\.7|刘璐|田延杰|
|V1.13|2018\.12.06|<p>修改 4.1.1、 4.1.2、 4.1.49 内容</p><p>新增 4.1.52 获取用户详情</p>|4\.1.1、4.1.2、 4.1.49、4.1.52|刘璐|田延杰|
|V1.14|2018\.12.26|修改 4.1.1、4.1.2 增加参数|4\.1.1、4.1.2|刘璐|田延杰|
|V1.15|2019\.1.7|新增查询个税扣除项接口|4\.1.53|刘璐|田延杰|
|V1.16|2019\.2.14|修正 4.1.15、4.1.13 节参数 解释|4\.1.15、4.1.13|刘璐|田延杰|
|V1.17|2019\.3.8|修改 4.1.52 节，增加返回参 数及说明|4\.1.52|刘璐|田延杰|

API接口规范![ref1]

1. 前言
1. 功能描述

本文总结了蜂巢  RESTful  API设计相关的一些原则和描述相应的接口规范，只覆 盖了常见的场景。

2. 阅读对象

具有相关知识的技术人员。

3. 业务术语

loginKey：可以是userName、email、phoneNo、idcard、employeeNumber之 一，代表唯一的用户user。

2. 接口规范
1. 协议

API与用户的通信协议使用HTTP协议，支持1.1及以上版本。

2. 域名

API部署在专用域名之下：<http://api.zts.com.cn>

3. **API**演进

就和系统迭代一样，API也是快速迭代开发的。API基本的URI没变，但是会同时 存在两个版本，版本间不影响现有的业务。现在使用的API的版本号放入URL， 如: <http://api.zts.com.cn/v1/>

4. **PATH**

我们一般用URI来定义希望对外暴露的服务。结构基本类似schema:// yourCompanyDomain/{version}/{someService}。schema可以是http，version指 的是你这个API的版本，someService就是这个子系统对外提供的服务。当然，如

1![ref2]
API接口规范![ref1]

果按照业务为边界划分，也可将业务维度相同但隶属于底层不同的系统的服务定 义为一个application。对于这种Rest请求，常见的响应结果就是XML或者是 JSON形式。 在蜂巢系统的RESTful架构中PATH示例如下：

[http://api.zts.com.cn/v1/groups/ ](http://api.zts.com.cn/v1/groups/)[http://api.zts.com.cn/v1/permissions/ ](http://api.zts.com.cn/v1/permissions/)<http://api.zts.com.cn/v1/users/>

5. **HTTP**动词

对于资源的具体操作类型，由**HTTP**动词表示。常用的**HTTP**动词有下面四个：

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- DELETE（DELETE）：从服务器删除资源。



|HTTP动词|安全性|幂等性|
| - | - | - |
|GET|√|√|
|POST|×|×|
|PUT|×|√|
|DELETE|×|√|

说明![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.003.png)![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.004.png)

- 安全性 ：不会改变资源状态，可以理解为只读的；
- 幂等性  ：执行1次和执行N次，对资源状态改变的效果 是等价的。
6. 过滤信息

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。下面是 一些常见的参数：

- ?page=1&size=100
- ?gender=1
- ?idcard=**\***
7. 状态码

服务器向用户返回的状态码和提示信息，常见的有以下:



|Http Status|Code|说明|
| - | - | - |
|200||成功。|
|201||用户新建或修改数据成 功。|
|204||用户删除数据成功。|
|400||请求失败。|
|400|400100|密码过期。|
|400|400101|密码错误达到最大次 数。|
|400|400102|用户已经被锁定。|
|401||Unauthorized [\*]：表示 用户没有权限（令牌、 用户名、密码错误）。|
|403||Forbidden [\*] 表示用户 得到授权（与401错误相 对），但是访问是被禁 止的。|
|404||NOT FOUND [\*]：用户 发出的请求针对的是不 存在的记录，服务器没 有进行操作，该操作是 幂等的。|
|406||Not Acceptable [GET]： 用户请求的格式不可得 （比如用户请求JSON 格式，但是只有XML格 式）。|



|410||Gone -[GET]：用户请求 的资源被永久删除，且 不会再得到的。|
| - | :- | :- |
|422||<p>Unprocesable entity</p><p>[POST/PUT/PATCH] 当 创建一个对象时，发生 一个验证错误。</p>|
|426||弱密码|
|500||<p>INTERNAL SERVER</p><p>ERROR [\*]：服务器发生 错误，用户将无法判断 发出的请求是否成功</p>|
|503||<p>SERVICE\_UNAVAILABL (503, "Service</p><p>Unavailable"),（重构代 码时建议使用的状态码 和提示信息）</p>|

注意![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.005.png)![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.006.png)

- 2\*\*  正常响应，可以获取对应的对象或列表结果集或空 集。
- 4\*\* 异常响应，可获取对应的异常结果集。
- 5\*\* 服务器异常。
8. 错误处理

正确设置http状态码，不要自定义。

**Response body** 提供：

1. 错误的代码（日志/问题追查）
1. 错误的描述文本（展示给用户）

{![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.007.png)

"timestamp": 1480492468879,

"status": 400, "error": "Bad Request",![ref3]

"exception": "com.zts.common.spec.exception.InvalidArgumentException", "message": "对照关系冲突",

"path": "/v1/users/convert"

}

9. 返回结果

针对不同操作，服务器向用户返回的结果符合以下规范：

- GET /collection：返回资源对象的列表（数组）
- GET /collection/resource：返回单个资源对象
- POST /collection：返回新生成的资源对象
- PUT /collection/resource：返回完整的资源对象
- DELETE /collection/resource：返回一个空文档

列表类返回结果如下： ![ref4]{

"total": 400,

"count": 9,

"page":2,

"size":9,

"limit":9,

"offset":9

"items": [

{

"userId": 90046,

"convertId": "baigw", "appCode": "M0006",

"status": "1", "employeeNumber": "64M0201737", "id": 104148

},

{

"userId": 90046,

"convertId": "baigw1", "appCode": "M0006",

"status": "0", "employeeNumber": "64M0201737", "id": 132019

},

......] }![ref5]

对象类返回结果如下： {

"userName": "ceshi", "name": "测试人员",

"email": "lily@zts.com.cn",

"phoneNo": "13905395017",

"officePhone": "0531-00095017",

"gender": "2",

"status": "1",

"chgPassFlag": null,

"orgs": "0157:10,0008:0,0193:0", "employeeNumber": "82F1618259",

"\_links": {

"self": {

"href": "http://api.zts.com.cn/v1/users/95017" }},

"id": 95017

}

10. 返回格式

服务器返回的数据格式使用JSON。![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.011.png)

3. 应用系统说明



|应用系统代码|应用系统名称|用户体系与蜂巢系 统是否一致|是否调用API|
| - | - | :- | - |
|M0001|人力资源系统|否|是|
|M0002|OA系统|是|是|
|M0003|财富管理平台|否||
|M0004|产品中心|是|是|
|M0005|咨询数据中心|否||
|M0006|综合数据平台|否|是|
|M0007|研究与服务管理系 统|否||


|M0008|财务系统|否|是|
| - | - | - | - |
|M0009|综合金融管理平台|否|是|
|M0010|法律事务|是||
|M0011|IT资产管理系统|否|是|
|M0012|机构CRM系统|否|是|
|M0013|大投行系统|否||
|M0014|全面风险管理系统|否||
|M0015|集中监控系统|否||
|M0016|合规管理系统|是||
|M0017|审计管理系统|否||
|M0018|领导驾驶舱|否|是|
|M0019|用户管理系统|是|是|
|M0020|XTP管理工具(交 易网)|是|是|
|M0021|新门户|是|是|
|M0022|互联网接入 (owncloud)|是|是|

4. **API**列表
1. 用户**USER**
1. 获取用户列表（身份证信息除外）

使用本接口获取部分或所有用户的信息（身份证信息除外），支持条件过滤，查 询结果默认按照employeeNumber排序，支持分页查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.012.png)

`  `http://localhost:8080/v1/users

请求示例：

`  `http://localhost:8080/v1/users?userName=test

参数说明



|参数|是否必须|说明|
| - | - | - |
|userName|否|登入用户名，支持模糊 查询|
|status|否|状态0:无效;1:有效|
|grade|否|等级|
|name|否|姓名，支持模糊查询|
|gender|否|性别 2:女;1:男|
|email|否|邮箱，支持模糊查询|
|phoneNo|否|手机号，支持模糊查询|
|officePhone|否|工作电话，支持模糊查 询|
|employeeNumber|否|员工编号|
|idcard|否|身份证|
|type|否|员工类型（预留）|
|jobCode|否|岗位编号|
|jobStatus|否|岗位状态|
|empStatus|否|员工状态|
|workPlace|否|工作地点，支持模糊查 询|
|officeShortNo|否|办公短号|
|id|否|用户id，精确查询|
|createPerson|否|创建人|
|createTime|否|创建时间，查询在此时 间（含）之后的数据|
|updatePerson|否|修改人|
|updateTime|否|修改时间，查询在此时 间（含）之后的数据|



|remark|否|备注说明，支持模糊查 询|
| - | - | :- |
|APP-CODE|否|头信息，应用系统的代 码|
|pageBound|否|分页查询条件， 不录入时，默认 page=1&size=100，即 约定每一页显示100条数 据，显示当前第1页|
|datasource|否|数据源|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.013.png)

`  `{

`      `"date":"Fri,03 Feb 2017 07:47:49 GMT"

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding":&nbsp;"chunked"

`      `"x-application-context":&nbsp;"application",

`      `"content-type":&nbsp;"application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`   `"total": 1,

`   `"count": 1,

`   `"items": [   {

`      `"userName": "liss",       "name": "李山山",

`      `"email": "mail00395@zts.com.cn",

`      `"phoneNo": "15421254451",

`      `"officePhone": "0531-00000395",

`      `"gender": "1",

`      `"status": "1",

`      `"chgPassFlag": "0",

`      `"orgs": "D3236:0",

`      `"employeeNumber": "7788string",

`      `"leaveDate": "2015-10-18",  // 离职日期       "userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",![ref6]

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",

`            `"jobCode": "301",

`            `"jobName": "分支机构管理岗",

`            `"jobGroupCode": null,

`            `"jobGroupName": 7788string          },

`                  `{

`            `"id": 36,

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗",

`            `"jobGroupCode": null,

`            `"jobGroupName": 7788string          }

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,![ref7]

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 2,

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 4,

`            `"userId": 395,

`            `"userKey": "key3",

`            `"userValue": "val3",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 8008,

`            `"userId": 395,

`            `"userKey": "eid",

`            `"userValue": "17548",

`            `"employeeNumber": 7788string

`        `}

`    `],

`      `"\_links": {"self": {"href": "http://localhost:8080/v1/ users/395"}},

`      `"id": 395

`   `}]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,   "items": []   }

2. 获取用户列表（含身份证信息）

使用本接口获取部分或所有用户的信息（包括身份证信息），支持条件过滤，查 询结果默认按照employeeNumber排序，支持分页查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref8]

`  `http://localhost:8080/v1/users/withidcard

请求示例：

`  `http://localhost:8080/v1/users/withidcard?userName=test

参数说明



|参数|是否必须|说明|
| - | - | - |
|userName|否|登入用户名，支持模糊 查询|
|status|否|状态0:无效;1:有效|
|grade|否|等级|
|name|否|姓名，支持模糊查询|
|gender|否|性别 2:女;1:男|
|email|否|邮箱，支持模糊查询|
|phoneNo|否|手机号，支持模糊查询|
|officePhone|否|工作电话，支持模糊查 询|
|employeeNumber|否|员工编号|
|idcard|否|身份证|
|type|否|员工类型（预留）|
|jobCode|否|岗位编号|
|jobStatus|否|岗位状态|
|empStatus|否|员工状态|
|workPlace|否|工作地点，支持模糊查 询|
|officeShortNo|否|办公短号|
|id|否|用户id，精确查询|
|createPerson|否|创建人|



|createTime|否|创建时间，查询在此时 间（含）之后的数据|
| - | - | :- |
|updatePerson|否|修改人|
|updateTime|否|修改时间，查询在此时 间（含）之后的数据|
|remark|否|备注说明，支持模糊查 询|
|APP-CODE|否|头信息，应用系统的代 码|
|pageBound|否|分页查询条件， 不录入时，默认 page=1&size=100，即 约定每一页显示100条数 据，显示当前第1页|
|datasource|否|数据源|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.017.png)

`  `{

`      `"date":"Fri,03 Feb 2017 07:47:49 GMT"

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding":&nbsp;"chunked"

`      `"x-application-context":&nbsp;"application",

`      `"content-type":&nbsp;"application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`   `"total": 1,

`   `"count": 1,

`   `"items": [   {

`      `"userName": "liss",       "name": "李山山",

`      `"email": "mail00395@zts.com.cn",       "phoneNo": "15421254451",

`      `"officePhone": "0531-00000395",       "gender": "1",

`      `"status": "1",

`      `"chgPassFlag": "0",

`      `"orgs": "D3236:0",![ref6]

`      `"employeeNumber": "7788string",

`      `"idcard": "370100101000000395",

`      `"leaveDate": "2015-10-18",  //离职日期       "userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗", ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.018.png)            "jobGroupCode": null,

`            `"jobGroupName": null

`         `}

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 2,

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 4,

`            `"userId": 395,

`            `"userKey": "key3",

`            `"userValue": "val3",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 8008,

`            `"userId": 395,

`            `"userKey": "eid",

`            `"userValue": "17548",

`            `"employeeNumber": 7788string

`        `}

`    `],

`      `"\_links": {"self": {"href": "http://localhost:8080/v1/ users/395"}},

`      `"id": 395

`   `}]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,

`  `"items": []

`  `}![ref9]

**4.1.3**查询某一用户信息

使用本接口根据主键id查询某一用户。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref10]

`  `http://localhost:8080/v1/users/{id}

请求示例：

`  `http://localhost:8080/v1/users/5288

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|APP-CODE|否|头信息，应用系统的代 码，可根据该头信息返 回用户某一应用系统的 账户信息。|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.021.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 05:31:14 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`      `"userName": "liss",

`      `"name": "李山山",

`      `"email": "mail00395@zts.com.cn",       "phoneNo": "15421254451",

`      `"officePhone": "0531-00000395",       "gender": "1",

`      `"status": "1",![ref11]

`      `"chgPassFlag": "0",

`      `"orgs": "D3236:0",

`      `"employeeNumber": "7788string",

`      `"userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗", ![ref6]            "jobGroupCode": null,

`            `"jobGroupName": null

`         `}

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 2,

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 4,

`            `"userId": 395,

`            `"userKey": "key3",

`            `"userValue": "val3",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 8008,

`            `"userId": 395,

`            `"userKey": "eid",

`            `"userValue": "17548",

`            `"employeeNumber": 7788string

`        `}

`    `],

`    `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/395"     }

`  `},

`  `"id": 395

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486359180864,

`  `"status": 404,![ref3]

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[125853]对应的用户不存在",

`  `"path": "/v1/users/125853"

`  `}

4. 新增用户

使用本接口新增user信息，传入参数为json格式。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.023.png)

`  `http://localhost:8080/v1/users

请求示例：

`  `http://localhost:8080/v1/users

参数说明



|参数|是否必须|说明|
| - | - | - |
|userModel|是|用户信息，接收数据 格式为**json**，可传入 userName、name、emai 字段|

l、employeeNumber、phoneNo、officePhone、gender、enrtyDate、createPerson、createTime、updatePerson、updateTime

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.024.png) {

` `"userName":"tom",

` `"email":"tom@163.cn",

` `"phoneNo":"1212",

` `"name":"tom",

` `"status":"1",

` `"employeeNumber":"121321",  "idcard":"5666446"

}

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.025.png)

`  `{![ref6]

`  `"date": "Sat, 04 Feb 2017 05:56:47 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"userName": "tom",

`  `"name": "tom",

`  `"email": "tom@zts.com.cn",

`  `"phoneNo": "1212",

`  `"officePhone": null,

`  `"gender": null,

`  `"status": "1",

`  `"chgPassFlag": "1",

`  `"orgs": "",

`  `"employeeNumber": "121321",

`  `"userMaps": null,

`  `"type": 1,

`  `"jobCode": null,

`  `"jobName": null,

`  `"jobGroupCode":"15",

"jobGroupName":"运营类-信息技术",

`  `"jobStatus": "1",

`  `"empStatus": "1",

`  `"workPlace": null,

`  `"phoneShortNo": null,

`  `"officeShortNo": null,

`  `"datasource": "API",

`  `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 12584,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 121321         },

`        `{

`            `"id": 2,

`            `"userId": 12584,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 121321         }

`    `],![ref12]

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/12584"     }

`  `},

`  `"id": 12584

` `}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486186648444,

`  `"status": 422,

`  `"error": "Unprocessable Entity", 

"exception":"com.zts.common.spec.exception.UnprocessableEntityException"，

`  `"message": "service.error.unprocessable\_entity",   "path": "/v1/users"

`  `}

5. 修改用户信息(根据主键)

使用本接口根据主键id修改用户信息。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.027.png)

`  `http://localhost:8080/v1/users/{id}

请求示例：

`  `http://localhost:8080/v1/users/12585

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|用户的id|
|userModel|是|要修改的用户信息项及 对应修改的值,接收数据 格式为**json**，loginKey必 须传入，可修改用户的 email、officePhone、offi 字段.|

ceShortNo、phoneNo、phoneShortNo、remark

参数示例如下：![ref8]

` `{

` `"workPlace":"BEIJING" }

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.028.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 05:42:24 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：

`  `{

`  `"timestamp": 1486359829584,

`  `"status": 422,

`  `"error": "Unprocessable Entity",

`  `"exception":

` `"com.zts.common.spec.exception.UnprocessableEntityException",   "message": "service.error.unprocessable\_entity",

`  `"path": "/v1/users/12585"

`  `}

6. 修改用户信息(根据**loginKey)**

使用本接口根据LoginKey修改用户信息。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![ref8]

`  `http://localhost:8080/v1/users

请求示例：

`  `http://localhost:8080/v1/users

参数说明



|参数|是否必须|说明|
| - | - | - |
|userModel|是|要修改的用户信息项及 对应修改的值,接收数据 格式为**json**，loginKey必 须传入，可修改用户的 email、officePhone、offi 字段.|

ceShortNo、phoneNo、phoneShortNo、remark

参数示例如下： ![ref13] {

` `"loginKey":"tom",  "phoneNo":"1212554" }

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.030.png)

`  `{

`  `"date": "Sat, 04 Feb 2017 05:56:47 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486186648444,

`  `"status": 422,

`  `"error": "Unprocessable Entity", 

"exception":"com.zts.common.spec.exception.UnprocessableEntityException"，

`  `"message": "service.error.unprocessable\_entity",   "path": "/v1/users"

`  `}

7. 删除用户

使用本接口根据主键id删除某一用户。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref8]

`  `http://localhost:8080/v1/users/{id}

请求示例：

`  `http://localhost:8080/v1/users/12585

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|用户的id|

返回说明

正确情况下的返回HTTP头如下：![ref14]

`  `{

`  `"date": "Mon, 06 Feb 2017 05:42:24 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486359980242,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "用户不存在！",

`  `"path": "/v1/users/55555"

`  `}

8. 查询用户的系统准入权限(根据主键**id)**

使用本接口根据用户主键id查询用户可以访问的应用系统列表，也可以传入应用 系统主键id或code查询某一指定的应用系统。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.032.png)

`  `http://localhost:8080/v1/users/{id}/applications

请求示例：

`  `http://localhost:8080/v1/users/12585/applications?applicationId=T111

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|applicationId|否|应用系统的id或code|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.033.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 05:52:41 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"appCode": "T545",

`      `"appName": "test0118002",       "description": null,

`      `"uniauthType": "2",

`      `"redirectUrl": null,

`      `"appSecret": null,![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.034.png)

`      `"status": "0",

`      `"authInfo": null,

`      `"unifiedFlag": "0",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/26"         }

`      `},

`      `"id": 26

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包如下：

`  `{

`  `"total": 0,   "count": 0,   "items": [] }

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486360446674,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[125854]对应的用户不存在",

`  `"path": "/v1/users/125854/applications"

`  `}

9. 查询用户的某一系统准入权限(根据主键**id)**

使用本接口根据用户主键id和应用系统主键id查询用户可以访问的应用系统信 息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref15]

`  `http://localhost:8080/v1/users/{id}/applications/{applicationId}

请求示例：

`  `http://localhost:8080/v1/users/12585/applications/26

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|applicationId|是|应用系统的id或code|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.036.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 05:58:26 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"appCode": "T545",

`  `"appName": "test0118002",

`  `"description": null,

`  `"uniauthType": "2",

`  `"redirectUrl": null,

`  `"appSecret": null,

`  `"status": "0",

`  `"authInfo": null,

`  `"unifiedFlag": "0",

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/26"     }

`  `},

`  `"id": 26

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486360663943,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "用户无系统访问权限",

`  `"path": "/v1/users/12585/applications/32"

`  `}![ref9]

10. 查询用户的系统准入权限(根据**loginKey)**

使用本接口根据LoginKey查询该用户的应用系统准入权限。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref10]

`  `http://localhost:8080/v1/users/applications

请求示例：

`  `http://localhost:8080/v1/users/applicationss?loginKey=tomy

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|applicationId|否|应用系统的id或code|

返回说明

正确情况下的返回HTTP头如下：![ref16]

`  `{

`  `"date": "Sat, 04 Feb 2017 08:43:29 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"appCode": "T545",

`      `"appName": "test0118002",       "description": null,

`      `"uniauthType": "2",![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.038.png)

`      `"redirectUrl": null,

`      `"appSecret": null,

`      `"status": "0",

`      `"authInfo": null,

`      `"unifiedFlag": "0",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/26"         }

`      `},

`      `"id": 26

`    `}

`  `]

` `}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486197852516,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/applications"

`  `}

**4.1.11**新增用户的系统准入权限

使用本接口为某一用户新增某应用系统的准入权限。

前提条件:![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.039.png)![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.040.png)

- 存在该应用系统对应的系统准入权限
- 存在某一逻辑组，已分配了该系统的准入权限

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.041.png)

`  `http://localhost:8080/v1/users/applications

请求示例：

`  `http://localhost:8080/v1/users/applications

或

`  `http://localhost:8080/v1/users/applications? loginKey=lily&appCode=T12345![ref17]

参数说明

支持两种传参方式：json格式查询参数格式![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.043.png)



|参数|是否必须|说明|
| - | - | - |
|userGrantPara|否|<p>json格式的参数，例如： { "appCode": "string",</p><p>"loginKey": "string" }</p>|
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|appCode|否|应用系统的代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.044.png)

`  `{

`  `"date": "Sat, 04 Feb 2017 08:43:29 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"appCode": "T545",

`      `"appName": "test0118002",       "description": null,

`      `"uniauthType": "2",

`      `"redirectUrl": null,

`      `"appSecret": null,

`      `"status": "0",

`      `"authInfo": null,

`      `"unifiedFlag": "0",

`      `"\_links": {![ref18]

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/26"         }

`      `},

`      `"id": 26

`    `}

`  `]

` `}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486344398180,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "当前应用系统尚未指定权限",

`  `"path": "/v1/users/applications"

`  `}

12. 删除用户的系统准入权限

使用本接口根据LoginKey删除该用户的应用系统准入权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.046.png)

`  `http://localhost:8080/v1/users/applications

请求示例：

`  `http://localhost:8080/v1/users/applications

参数说明

支持两种传参方式：json格式和查询参数格式![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.047.png)



|参数|是否必须|说明|
| - | - | - |
|userGrantPara|json格式传参时必须输入|要删除的用户的某 一应用系统的权限 信息,接收数据格式 为**json**，loginKey和 appCode必须传入|



|loginKey|使用查询参数格式时必 须输入|登录用户名/身份证/邮箱/ 手机号/员工编号|
| - | :- | :- |
|appCode|使用查询参数格式时必 须输入|应用系统代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.048.png)

`  `{

`  `"date": "Sat, 04 Feb 2017 07:17:07 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486192545519,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "当前应用系统尚未指定权限",

`  `"path": "/v1/users/applications"

`  `}

13. 校验用户的系统准入权限

使用本接口校验用户密码，并校验用户是否拥有当前用户的权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.049.png)

`  `http://localhost:8080/v1/users/checkaccess

请求示例：

`  `http://localhost:8080/v1/users/checkaccess

参数说明



|参数|是否必须|说明|
| - | - | - |



|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
| - | - | :- |
|password|是|用户密码|
|encrypt|否|密码加密算法，取值为 MD5时密码为32位MD5 加密，小写；不传时密 码为明文|
|APP-CODE|是|应用系统的代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.050.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 01:32:16 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`      `"userName": "liss",

`      `"name": "李山山",

`      `"email": "mail00395@zts.com.cn",

`      `"phoneNo": "15421254451",

`      `"officePhone": "0531-00000395",

`      `"gender": "1",

`      `"status": "1",

`      `"chgPassFlag": "0",

`      `"orgs": "D3236:0",

`      `"employeeNumber": "7788string",

`      `"userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,![ref6]

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗",

`            `"jobGroupCode": null,

`            `"jobGroupName": null

`         `}

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string         },

`        `{

`            `"id": 2,![ref7]

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 4,

`            `"userId": 395,

`            `"userKey": "key3",

`            `"userValue": "val3",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 8008,

`            `"userId": 395,

`            `"userKey": "eid",

`            `"userValue": "17548",

`            `"employeeNumber": 7788string

`        `}

`    `],

`    `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/395"     }

`  `},

`  `"id": 395

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486345273263,

`  `"status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "用户无系统访问权限",

`  `"path": "/v1/users/checkaccess"

`  `}

14. 验证用户是否存在

使用本接口根据LoginKey查询用户。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.051.png)

`  `http://localhost:8080/v1/users/checkexist

请求示例：

`  `http://localhost:8080/v1/users/checkexist?loginKey=tom

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|APP-CODE|否|应用系统代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.052.png)

`  `{

`  `"date": "Sat, 04 Feb 2017 08:43:29 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：   {

`      `"userName": "liss",

`      `"name": "李山山",

`      `"email": "mail00395@zts.com.cn",       "phoneNo": "15421254451",

`      `"officePhone": "0531-00000395",       "gender": "1",

`      `"status": "1",

`      `"chgPassFlag": "0",

`      `"orgs": "D3236:0",

`      `"employeeNumber": "7788string",       "userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",![ref11]

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗",

`            `"jobGroupCode": null,             "jobGroupName": null          }

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.053.png)

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 2,

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 4,

`            `"userId": 395,

`            `"userKey": "key3",

`            `"userValue": "val3",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 8008,

`            `"userId": 395,

`            `"userKey": "eid",

`            `"userValue": "17548",

`            `"employeeNumber": 7788string

`        `}

`    `],

`    `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/395"     }

`  `},

`  `"id": 395

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486345403916,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/checkexist"

`  `}

15. 校验用户认证信息

使用本接口根据loginKey校验用户密码。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.054.png)

`  `http://localhost:8080/v1/users/checkvalid

请求示例：

`  `http://localhost:8080/v1/users/checkvalid

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|password|是|用户密码|
|encrypt|否|密码加密算法，取值为 MD5时密码为32位MD5 加密，小写；不传时密 码为明文。|
|APP-CODE|否|应用系统的代码，该参 数放到Http Header中|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.055.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 01:55:05 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`      `"userName": "liss",

`      `"name": "李山山",

`      `"email": "mail00395@zts.com.cn",

`      `"phoneNo": "15421254451",![ref6]

`      `"officePhone": "0531-00000395",

`      `"gender": "1",

`      `"status": "1",

`      `"chgPassFlag": "0",

`      `"orgs": "D3236:0",

`      `"employeeNumber": "7788string",

`      `"userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,![ref11]

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗",

`            `"jobGroupCode": null,

`            `"jobGroupName": null

`         `}

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 2,

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 4,

`            `"userId": 395,

`            `"userKey": "key3",

`            `"userValue": "val3",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 8008,

`            `"userId": 395,

`            `"userKey": "eid",

`            `"userValue": "17548",

`            `"employeeNumber": 7788string

`        `}

`    `],

`    `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/395"     }

`  `},

`  `"id": 395

}

错误情况下的返回JSON数据包示例如下： ![ref19]  {

`  `"timestamp": 1486346177904,

`  `"status": 401,

`  `"error": "Unauthorized",

`  `"exception": "com.zts.common.spec.exception.UnauthorizedException",   "message": "密码校验失败！",

`  `"path": "/v1/users/checkvalid"

`  `}

16. 查询用户的系统对应关系(根据主键**id)**

使用本接口根据用户主键id查询系统内用户与其他应用系统用户的对照表，可通 过提供其他应用系统的用户id或应用系统的代码进行详细查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref15]

`  `http://localhost:8080/v1/users/{id}/convert

请求示例：

`  `http://localhost:8080/v1/users/12585/convert?ryId=tomy&appCode=T545

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|ryId|否|其他应用系统用户标识|
|appCode|否|应用系统的代码|

返回说明

正确情况下的返回HTTP头如下：![ref20]

`  `{

`  `"date": "Mon, 06 Feb 2017 02:01:59 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.058.png)

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"userId": 12585,

`      `"convertId": "tomy",

`      `"appCode": "T545",

`      `"status": "1",

`      `"employeeNumber": "5512685",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/63746"         }

`      `},

`      `"id": 63746

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下：

`  `{

`  `"total": 0,   "count": 0,   "items": [] }

17. 查询用户的系统对应关系(根据**loginKey)**

使用本接口根据关键信息（用户名/身份证/邮箱/手机号/员工编号）查询系统内用 户与其他应用系统用户的对照表，可通过提供其他应用系统的用户id或应用系统 的代码进行详细查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.059.png)

`  `http://localhost:8080/v1/users/convert

请求示例：

`  `http://localhost:8080/v1/users/convert? loginKey=tomy&ryId=tomy&appCode=T545

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|ryId|否|其他应用系统用户标识|
|appCode|否|应用系统的代码|

返回说明

正确情况下的返回HTTP头如下：![ref21]

`  `{

`  `"date": "Mon, 06 Feb 2017 02:01:59 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"userId": 12585,

`      `"convertId": "tomy",

`      `"appCode": "T545",

`      `"status": "1",

`      `"employeeNumber": "5512685",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/63745"         }

`      `},

`      `"id": 63745

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下：

`  `{

`  `"total": 0,   "count": 0,

`  `"items": [] }![ref22]

18. 新增用户的系统对应关系(根据主键**id)**

使用本接口通过用户主键id新增系统内用户与其他应用系统用户的对应关系。

前提条件:![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.062.png)![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.063.png)

- 存在该应用系统对应的系统准入权限
- 存在某一逻辑组，已分配了该系统的准入权限

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.064.png)

`  `http://localhost:8080/v1/users/{id}/convert

请求示例：

`  `http://localhost:8080/v1/users/12585/convert

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|userMapPara|是|系统内用户与其他应 用系统用户的对应关 系信息，包括系统用 户标识ryId、系统代码 **appCode,**均必输|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.065.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 06:09:09 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.066.png)

正确情况下的返回JSON数据包如下：

`  `{

`  `"userId": 12585,

`  `"convertId": "tomy",

`  `"appCode": "T545",

`  `"status": "1",

`  `"employeeNumber": "5512685",

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/63746"     }

`  `},

`  `"id": 63746

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486361433999,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "当前应用系统尚未指定权限",

`  `"path": "/v1/users/12585/convert"

`  `}

19. 新增用户的系统对应关系(根据**loginKey)**

使用本接口通过关键信息（用户名/身份证/邮箱/手机号/员工编号）新增系统内用 户与其他应用系统用户的对应关系.

前提条件:![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.067.png)![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.068.png)

- 存在该应用系统对应的系统准入权限
- 存在某一逻辑组，已分配了该系统的准入权限

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.069.png)

`  `http://localhost:8080/v1/users/convert

请求示例：

`  `http://localhost:8080/v1/users/convert

或![ref23]

`  `http://localhost:8080/v1/users/convert? loginKey=tommy&ryId=tommy&appCode=T545

参数说明

支持两种传参方式：json格式和查询参数格式



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|ryId|否|其他应用系统用户标识|
|appCode|否|应用系统的代码|

返回说明

正确情况下的返回HTTP头如下：![ref4]

`  `{

`  `"date": "Mon, 06 Feb 2017 02:09:38 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"userId": 12585,

`      `"convertId": "tomy",

`      `"appCode": "T545",

`      `"status": "1",

`      `"employeeNumber": "5512685",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/63745"         }

`      `},

`      `"id": 63745

`    `}   ] }![ref24]

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486347186373,

`  `"status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "参数< loginkey、convertId、appCode >不能为空!",

`  `"path": "/v1/users/convert"

`  `}

20. 删除用户的应用系统对应关系(根据主键**id)**

使用本接口根据用户主键id删除系统内该用户与指定应用系统的指定用户的对应 关系。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref15]

`  `http://localhost:8080/v1/users/{id}/convert

请求示例：

`  `http://localhost:8080/v1/users/12585/convert

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|ryId|是|其他应用系统用户标识|
|appCode|是|应用系统代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.072.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 06:19:15 GMT",   "server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",![ref25]

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

**4.1.21**删除用户的应用系统对应关系(根据**loginKey)**

使用本接口通过关键信息（用户名/身份证/邮箱/手机号/员工编号）删除系统内用 户与其他应用系统用户的某一条或多条对应关系。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.074.png)

`  `http://localhost:8080/v1/users/convert

请求示例：

`  `http://localhost:8080/v1/users/convert

或

`  `http://localhost:8080/v1/users/convert? loginKey=tommy&ryId=tommy&appCode=T545

参数说明

支持两种传参方式：json格式和查询参数格式![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.075.png)



|参数|是否必须|说明|
| - | - | - |
|userMapPara|json格式传参时必须输入|系统内用户与其他应用 系统用户的对应关系信 息，包括系统内用户关 键信息loginKey（用户 名/身份证/邮箱/手机号/ 员工编号）、其他系统 用户标识ryId、系统代码 appCode；其中系统代 码appCode必输，多个 系统用逗号隔开；用户 名和ryId必须传入一个|



|loginKey|使用查询参数格式时必 须输入|登录用户名/身份证/邮箱/ 手机号/员工编号|
| - | :- | :- |
|ryId|使用查询参数格式时必 须输入|其他应用系统用户标识|
|appCode|使用查询参数格式时必 须输入|应用系统代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.076.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 02:22:18 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回JSON数据包示例如下：   {

`   `"timestamp": 1486347854411,   "status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "参数<appCodes >不能为空!",

`  `"path": "/v1/users/convert"

`  `}

22. 获取系统对应关系列表

使用本接口获取蜂巢系统用户与其他系统用户对应关系的分页列表信息，支持条 件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.077.png)

`  `http://localhost:8080/v1/users/convertlist

请求示例：

`  `http://localhost:8080/v1/users/convertlist?convertId=tomy

参数说明



|参数|是否必须|说明|
| - | - | - |
|userId|否|蜂巢系统用户id|
|convertId|否|其他业务系统用户id|
|appCode|否|其他业务系统的代码|
|status|否|状态（1:启用，0:未启 用）|
|id|否|对应关系的id|
|createPerson|否|创建人|
|createTime|否|创建时间，查询在此时 间（含）之后的数据|
|updatePerson|否|修改人|
|updateTime|否|修改时间，查询在此时 间（含）之后的数据|
|remark|否|备注说明|
|pageBound|否|分页查询条件， 不录入时，默认 page=1&size=100，即 约定每一页显示100条数 据，显示当前第1页|

返回说明

正确情况下的返回HTTP头如下：![ref26]

`  `{

`  `"date": "Mon, 06 Feb 2017 02:01:59 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,   "count": 1,

`  `"items": [![ref27]

`    `{

`      `"userId": 12585,

`      `"convertId": "tomy",

`      `"appCode": "T545",

`      `"status": "0",

`      `"employeeNumber": "5512685",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/63745"         }

`      `},

`      `"id": 63745

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下：

`  `{

`  `"total": 0,   "count": 0,   "items": [] }

23. 查询用户所属逻辑组(根据主键**id)**

使用本接口根据用户主键id查询用户的所属逻辑组的信息，支持查询范围条件查 询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.080.png)

`  `http://localhost:8080/v1/users/{id}/groups

请求示例：

`  `http://localhost:8080/v1/users/12585/groups

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|supper|否|boolean型，所属逻辑组 范围，supper=true时，|



|||除返回该用户的直接所 属逻辑组外，还返回该 用户所属逻辑组的所有 上级逻辑组|
| :- | :- | - |

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.081.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 06:36:35 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"parents": "",

`      `"groupNumber": "SYST545",

`      `"groupName": "T545",

`      `"groupType": "3",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,

`      `"datasource": "API",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/14307"         }

`      `},

`      `"id": 14307

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包如下：

`  `{

`  `"total": 0,   "count": 0,

`  `"items": []

}![ref9]

24. 查询用户所属逻辑组(根据**loginKey)**

使用本接口根据关键信息（用户名/身份证/邮箱/手机号/员工编号）查询用户的所 属逻辑组信息，支持查询范围条件查询.

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref10]

`  `http://localhost:8080/v1/users/groups

请求示例：

`  `http://localhost:8080/v1/users/groups?loginKey=tomy

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|supper|否|boolean型，所属逻辑组 范围，supper=true时， 除返回该用户的直接所 属逻辑组外，还返回该 用户所属逻辑组的所有 上级逻辑组|

返回说明

正确情况下的返回HTTP头如下：![ref28]

`  `{

`  `"date": "Sat, 04 Feb 2017 08:43:29 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：   {

`  `"total": 1,![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.083.png)

`  `"count": 1,

`  `"items": [

`    `{

`      `"parents": "",

`      `"groupNumber": "SYST545",

`      `"groupName": "T545",

`      `"groupType": "3",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,

`      `"datasource": "API",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/14307"         }

`      `},

`      `"id": 14307

`    `}

`  `]

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486349915486,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/groups"

`  `}

25. 查询用户某个所属组(根据主键**id)**

使用本接口根据用户主键id和组id查询用户的所属组信息，用于判断用户是否在 指定组中。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.084.png)

`  `http://localhost:8080/v1/users/{id}/groups/{groupId}

请求示例：

`  `http://localhost:8080/v1/users/12585/groups/9866

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|groupId|否|组的主键id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.085.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 06:36:35 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"parents": "",

`  `"groupNumber": "QTGL",   "groupName": "其他管理",

`  `"groupType": "3",

`  `"subType": null,

`  `"status": "1",

`  `"description": null,   "datasource": null,

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/9866"     }

`  `},

`  `"id": 9866

}

错误情况下的返回JSON数据包如下：   {

`  `"timestamp": 1486364824194,   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未获取指定用户的所属组：groupId=9867",

`  `"path": "/v1/users/12585/groups/9867"

}![ref29]

26. 用户添加多个角色

使用本接口根据用户主键id，将用户增加到多个组，用于判断给用户授予多个角 色。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.087.png)

`  `http://localhost:8080/v1/users/{id}/groups

请求示例：

`  `http://localhost:8080/v1/users/12585/groups

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|groupModels|是|用户即将加入的多个逻 辑组集合，其中必须指 定逻辑组的id，传入格式 为**json**|

参数groupModels示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.088.png)  [

`    `{"id":9866}

`  `]

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.089.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 06:36:35 GMT",   "server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",   }

正确情况下的返回结果如下：

`  `1

查询结果为空的情况下的返回JSON数据包如下： ![ref30]  {

`  `"timestamp": 1486363676842,

`  `"status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "id[1]对应的组不是逻辑组",

`  `"path": "/v1/users/12585/groups"

}

27. 用户添加某个角色

使用本接口根据用户主键id，将用户增加到某一组，用于判断给用户授予某一角 色。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref31]

`  `http://localhost:8080/v1/users/{id}/groups/{groupId}

请求示例：

`  `http://localhost:8080/v1/users/12585/groups/9866

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|groupId|是|用户即将加入的组的主 键id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.092.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 07:10:55 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1![ref32]

错误情况下的返回JSON数据包如下：   {

`  `"timestamp": 1486365089901,   "status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "[9866:14310]该上下级关系已存在，不允许新增！",

`  `"path": "/v1/users/12585/groups/9866"

}

28. 用户删除多个角色

使用本接口根据用户主键id，将用户从多个组中删除，常用于给用户删除多个角 色。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.094.png)

`  `http://localhost:8080/v1/users/{id}/groups

请求示例：

`  `http://localhost:8080/v1/users/12585/groups

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|groupList|是|要删除的上级组集合， 传入数据格式为**json**|

参数groupList示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.095.png)  [

`    `{"id":9866}

`  `]

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.096.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 06:57:39 GMT",![ref33]

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回JSON数据包如下：   {

`  `"timestamp": 1486364293335,   "status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "不存在此上下级关系！",

`  `"path": "/v1/users/12585/groups"

}

29. 用户删除某个角色

使用本接口根据用户主键id，将用户从某一组中删除，常用于给用户删除某一角 色。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref31]

`  `http://localhost:8080/v1/users/{id}/groups/{groupId}

请求示例：

`  `http://localhost:8080/v1/users/12585/groups/9866

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|groupId|是|用户即将退出的组的主 键id|

返回说明

正确情况下的返回HTTP头如下：![ref34]

`  `{

`  `"date": "Mon, 06 Feb 2017 07:02:18 GMT",![ref35]

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回JSON数据包如下：   {

`  `"timestamp": 1486364512421,   "status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "id为[12585]的用户，目前不在id为[9866]的组中",

`  `"path": "/v1/users/12585/groups/9866"

}

30. 获取老**OA**系统的用户对照关系列表

使用本接口获取老oa的所有用户对照分页列表信息，不支持条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref36]

`  `http://localhost:8080/v1/users/oauser

请求示例：

`  `http://localhost:8080/v1/users/oauser

参数说明



|参数|是否必须|说明|
| - | - | - |
|pageBound|否|分页查询条件， 不录入时，默认 page=1&size=100，即 约定每一页显示100条数 据，显示当前第1页|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.101.png)

`  `{![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.102.png)

`  `"date": "Mon, 06 Feb 2017 02:01:59 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 100,

`  `"count": 100,

`  `"items": [

`    `{

`      `"userName": "tomy",

`      `"oaName": "tomy",

`      `"employeeNumber": "454546",

`      `"status": "1",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/0"         }

`       `}

`    `},

...{...}  //此处省略

`  `]

}

31. 查询用户所属机构(根据主键**id)**

使用本接口根据用户主键id查询用户的组织机构信息，支持查询范围条件查询.

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.103.png)

`  `http://localhost:8080/v1/users/{id}/orgs

请求示例：

`  `http://localhost:8080/v1/users/136/orgs?&supper=true

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|



|supper|否|boolean型，所属机构范 围，supper=true时，除 返回该用户的直接所属 部门外，还返回该用户 所属部门的所有上级机 构|
| - | - | :- |

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.104.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 07:15:09 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 3,   "count": 3,   "items": [     {

`      `"parents": "",

`      `"groupNumber": "0001",       "groupName": "A公司",

`      `"groupType": "1",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,       "datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/1"         }

`      `},

`      `"id": 1

`    `},

`    `{

`      `"parents": "0001:45",       "groupNumber": "0031",

`      `"groupName": "B总部",

`      `"groupType": "1",

`      `"subType": null,![ref37]

`      `"status": "1",

`      `"description": null,

`      `"datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/30"         }

`      `},

`      `"id": 30

`    `},

`    `{

`      `"parents": "0031:14",       "groupNumber": "0157",

`      `"groupName": "C分部",

`      `"groupType": "1",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,       "datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/136"         }

`      `},

`      `"id": 136

`    `}

`  `]

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486365365747,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[13666666]对应的用户不存在",

`  `"path": "/v1/users/13666666/orgs"

`  `}

32. 查询用户所属机构(根据**loginKey)**

使用本接口根据关键信息（用户名/身份证/邮箱/手机号/员工编号）查询用户的组 织机构信息，支持查询范围条件查询.

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.106.png)

`  `http://localhost:8080/v1/users/orgs

请求示例：

`  `http://localhost:8080/v1/users/orgs?loginKey=tomy

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|supper|否|boolean型，所属机构范 围，supper=true时，除 返回该用户的直接所属 部门外，还返回该用户 所属部门的所有上级机 构|

返回说明

正确情况下的返回HTTP头如下：![ref38]

`  `{

`  `"date": "Sat, 04 Feb 2017 08:43:29 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 3,   "count": 3,   "items": [     {

`      `"parents": "",

`      `"groupNumber": "0001",       "groupName": "A公司",

`      `"groupType": "1",

`      `"subType": null,![ref6]

`      `"status": "1",

`      `"description": null,

`      `"datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/1"         }

`      `},

`      `"id": 1

`    `},

`    `{

`      `"parents": "0001:45",       "groupNumber": "0031",

`      `"groupName": "B总部",

`      `"groupType": "1",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,       "datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/30"         }

`      `},

`      `"id": 30

`    `},

`    `{

`      `"parents": "0031:14",       "groupNumber": "0157",

`      `"groupName": "C分部",

`      `"groupType": "1",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,       "datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/136"         }

`      `},

`      `"id": 136

`    `}

`  `]

}

错误情况下的返回JSON数据包示例如下： ![ref39]  {

`  `"timestamp": 1486350884715,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/groups"

`  `}

33. 修改用户密码(根据主键**id)**

使用本接口根据用户主键id修改密码，传入格式为明文或密文。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.109.png)

`  `http://localhost:8080/v1/users/{id}/password

请求示例：

`  `http://localhost:8080/v1/users/10585/password

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|oldPassword|是|用户原密码|
|newPassword|是|用户新密码|
|encrypt|否|密码传入方式：明文或 密文（MD5时，传入密 码为32位小写），传入 空时默认为明文|

返回说明

正确情况下的返回HTTP头如下：![ref40]

`  `{

`  `"date": "Mon, 06 Feb 2017 03:19:28 GMT",   "server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",![ref41]

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486365888345,

`  `"status": 401,

`  `"error": "Unauthorized",

`  `"exception": "com.zts.common.spec.exception.UnauthorizedException",   "message": "密码校验失败！",

`  `"path": "/v1/users/10585/password"

`  `}

34. 修改用户密码(根据**loginKey)**

使用本接口根据用户关键信息（用户名/身份证/邮箱/手机号/员工编号）修改密 码，传入格式为明文或密文。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![ref42]

`  `http://localhost:8080/v1/users/password

请求示例：

`  `http://localhost:8080/v1/users/password

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|oldPassword|是|用户原密码|
|newPassword|是|用户新密码|
|encrypt|否|密码传入方式：明文或 密文（MD5时，传入密 码为32位小写），传入 空时默认为明文|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.113.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 03:19:28 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486351148316,

`  `"status": 401,

`  `"error": "Unauthorized",

`  `"exception": "com.zts.common.spec.exception.UnauthorizedException",   "message": "密码校验失败！",

`  `"path": "/v1/users/password"

`  `}

35. 重置用户密码

使用本接口根据用户主键id重置密码，传入格式为明文或密文。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref43]

`  `http://localhost:8080/v1/users/{id}/password

请求示例：

`  `http://localhost:8080/v1/users/10585/password

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户的id|
|newPassword|是|用户新密码|



|encrypt|否|密码传入方式：明文或 密文（MD5时，传入密 码为32位小写），传入 空时默认为明文|
| - | - | - |

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.115.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 07:22:07 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

36. 查询用户所属权限(根据主键**id)**

使用本接口根据用户主键id查询该用户的权限信息，支持按照权限层次和范围进 行过滤。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.116.png)

`  `http://localhost:8080/v1/users/{id}/permissions

请求示例：

`  `http://localhost:8080/v1/users/10585/permissions? appCode=T545&permissionLevel=1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|appCode|否|权限所属系统，传应用 系统的代码|



|supper|否|boolean型，权限范 围，supper=true时，除 返回该用户的直接权限 外，还返回该用户继承 自上级的权限|
| - | - | :- |
|permissionLevel|否|权限层次，1：系统准 入权限；2：功能菜单权 限；3：按钮层次权限|

返回说明

正确情况下的返回HTTP头如下：![ref44]

`  `{

`  `"date": "Mon, 06 Feb 2017 03:22:34 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"permissionName": "T545-",

`      `"permissionNumber": "T55-",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 26,

`      `"permissionLevel": "1",

`      `"url": "string",

`      `"source": "TRUE",

`      `"sourceDescription": "继承自[T545:SYST545]",

`      `"applicationCode": "T545",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/31"         }

`      `},

`      `"id": 31

`    `}![ref45]

`  `]

}

查询结果为空的情况下的返回JSON数据包如下：

`  `{

`  `"total": 0,   "count": 0,   "items": [] }

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486366700624,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[105854]对应的用户不存在",

`  `"path": "/v1/users/105854/permissions"

`  `}

**4.1.37**查询用户所属权限(根据**loginKey)**

使用本接口根据关键信息（用户名/身份证/邮箱/手机号/员工编号）查询该用户的 权限信息，支持按照权限层次和范围进行过滤。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref15]

`  `http://localhost:8080/v1/users/permissions

请求示例：

`  `http://localhost:8080/v1/users/permissions?loginKey=tomy

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|appCode|否|权限所属系统，传应用 系统的代码|



|supper|否|boolean型，权限范 围，supper=true时，除 返回该用户的直接权限 外，还返回该用户继承 自上级的权限|
| - | - | :- |
|permissionLevel|否|权限层次，1：系统准 入权限；2：功能菜单权 限；3：按钮层次权限|

返回说明

正确情况下的返回HTTP头如下：![ref44]

`  `{

`  `"date": "Mon, 06 Feb 2017 03:22:34 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"permissionName": "T545",

`      `"permissionNumber": "T55",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 26,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": "TRUE",

`      `"sourceDescription": "继承自[T545:SYST545]",

`      `"applicationCode": "T545",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/31"         }

`      `},

`      `"id": 31

`    `}![ref45]

`  `]

}

查询结果为空的情况下的返回JSON数据包如下：

`  `{

`  `"total": 0,   "count": 0,   "items": [] }

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486351429329,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/permissions"

`  `}

38. 查询用户某一权限

使用本接口根据用户主键id和权限主键id查询该用户的系统访问权限。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref31]

`  `http://localhost:8080/v1/users/{id}/permissions/{permissionId}

请求示例：

`  `http://localhost:8080/v1/users/12585/permissions/3

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|permissionId|是|权限id|

返回说明

正确情况下的返回HTTP头如下：![ref34]

`  `{

`  `"date": "06 Feb 2017 08:00:23 GMT",![ref46]

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"permissionName": "T545-",

`  `"permissionNumber": "T55-",

`  `"permissionType": "1",

`  `"status": "1",

`  `"applicationId": 26,

`  `"permissionLevel": "1",

`  `"url": "string",

`  `"source": "TRUE",

`  `"sourceDescription": "继承自[T545:SYST545]",

`  `"applicationCode": "T545",

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/31"     }

`  `},

`  `"id": 31

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486368002017,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到用户的权限",

`  `"path": "/v1/users/12585/permissions/26"

`  `}

39. 用户新增权限

使用本接口为某一用户赋予一个或多个权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref47]

`  `http://localhost:8080/v1/users/{id}/permissions![ref48]

请求示例：

`  `http://localhost:8080/v1/users/12585/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|permissionModels|是|权限集合,须指明权限 id，传入格式为**json**|

参数permissionModels示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.122.png)  [

`   `{"id":3}

`  `]

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.123.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 03:22:34 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486367634670,

`  `"status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "当前对象已具备该权限[3]，无需重新授权。",

`  `"path": "/v1/users/12585/permissions"

`  `}

40. 用户新增某一权限

使用本接口为某一用户赋予一项权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref43]

`  `http://localhost:8080/v1/users/{id}/permissions/{permissionId}

请求示例：

`  `http://localhost:8080/v1/users/12585/permissions/26

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|permissionId|是|权限id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.124.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 08:04:53 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486368405922,

`  `"status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "当前对象已具备该权限[26]，无需重新授权。",

`  `"path": "/v1/users/12585/permissions/26"

`  `}

41. 用户删除所有权限

使用本接口给某一用户删除所有权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref8]

`  `http://localhost:8080/v1/users/{id}/permissions

请求示例：

`  `http://localhost:8080/v1/users/12585/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|

返回说明

正确情况下的返回HTTP头如下：![ref14]

`  `{

`  `"date": "Mon, 06 Feb 2017 07:55:35 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`   `"timestamp": 1486367735395,   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[12666]对应的用户不存在",

`  `"path": "/v1/users/12666/permissions"

`  `}

42. 用户删除某一权限

使用本接口给某一用户删除某一项权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.125.png)

`  `http://localhost:8080/v1/users/{id}/permissions/{permissionId}

请求示例：

`  `http://localhost:8080/v1/users/12585/permissions/31

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|permissionId|是|权限id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.126.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 08:02:47 GMT",   "server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

43. 查询用户的全景视图

使用本接口根据关键信息（用户名/身份证/邮箱/手机号/员工编号）查询用户全景 视图信息，包括基本信息、所在机构、权限、其他附加属性信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.127.png)

`  `http://localhost:8080/v1/users/profile

请求示例：![ref17]

`  `http://localhost:8080/v1/users/profile?loginKey=tomy

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|登录用户名/身份证/邮箱/ 手机号/员工编号|
|APP-CODE|是|头信息，应用系统的代 码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.128.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 03:27:31 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"userName": "tomy",

`  `"name": "tomy",

`  `"email": "tomy@163.cn",

`  `"phoneNo": "128745545",

`  `"officePhone": null,

`  `"employeeNumber": "5512685",

`  `"userMaps": [],

`  `"phoneShortNo": null,

`  `"officeShortNo": null,

`  `"properties": {

`    `"total": 0,

`    `"count": 0,

`    `"items": []

`  `},

`  `"applications": {

`    `"total": 1,

`    `"count": 1,

`    `"items": [

`      `{

`        `"appCode": "T545",![ref49]

`        `"appName": "test0118002",

`        `"description": null,

`        `"uniauthType": "2",

`        `"redirectUrl": null,

`        `"appSecret": null,

`        `"status": "0",

`        `"authInfo": null,

`        `"unifiedFlag": "0",

`        `"\_links": {

`          `"self": {

`            `"href": "http://localhost:8080/v1/users/26"           }

`        `},

`        `"id": 26

`      `}

`    `]

`  `},

`  `"orgs": {

`    `"total": 0,

`    `"count": 0,

`    `"items": []

`  `},

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/12585"

`    `}

`  `},

`  `"id": 12585

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486351651444,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/profile"

`  `}

44. 查询用户的补充信息

使用本接口根据用户主键id查询该用户的补充信息，也可进一步根据补充信息项 的代码查询某一补充信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.130.png)

`  `http://localhost:8080/v1/users/{id}/properties

请求示例：

http://localhost:8080/v1/users/12585/properties?propertyKey=key1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|propertyKey|否|补充信息项的代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.131.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 08:10:45 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包示例如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"userKey": "key1",

`      `"userValue": "val1",

`      `"employeeNumber": "5512685",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/users/5"         }

`      `},

`      `"id": 5

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.132.png)

`  `{

`  `"total": 0,   "count": 0,   "items": [] }

45. 查询用户的某一补充信息

使用本接口根据用户主键id和补充信息主键id查询该用户的某一补充信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref50]

`  `http://localhost:8080/v1/users/{id}/properties/{propertyId}

请求示例：

http://localhost:8080/v1/users/12585/properties/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|propertyId|是|补充信息项的id|

返回说明

正确情况下的返回HTTP头如下：![ref51]

`  `{

`  `"date": "Mon, 06 Feb 2017 08:10:45 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包示例如下：   {

`  `"userKey": "key1",

`  `"userValue": "val1",

`  `"employeeNumber": "5512685",

`  `"\_links": {![ref33]

`    `"self": {

`      `"href": "http://localhost:8080/v1/users/5"     }

`  `},

`  `"id": 5

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486369453313,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未获取到用户的属性：1",

`  `"path": "/v1/users/12585/properties/1"

}

46. 新增或修改用户的补充信息

使用本接口根据用户主键id新增或更新用户的补充信息。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![ref15]

`  `http://localhost:8080/v1/users/{id}/properties

请求示例：

http://localhost:8080/v1/users/12585/properties

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|userProperties|是|新增或修改的用户补 充信息，传入格式为 **json**，指定补充信息的 主键id时，执行更新操 作，否则，执行新增操 作|

参数userProperties示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.135.png)  [

`  `{

`    `"id":1,

`    `"userKey": "key1",

`    `"userValue": "val1"

`  `},

{

`    `"userKey": "key2",

`    `"userValue": "val2"

`  `},

{

`    `"userKey": "key3",

`    `"userValue": "val3"

`  `}

]

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.136.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 08:12:59 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果，是新增或修改的记录数，如：   3

错误情况下的返回JSON数据包示例如下：

`  `{

`  `"timestamp": 1486369149747,

`  `"status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "用户属性值已存在，请检查",

`  `"path": "/v1/users/12585/properties"

` `}

47. 删除用户的补充信息

使用本接口根据用户主键id删除用户的补充信息，也可进一步根据提供的补充信 息的代码删除某一条补充信息。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.137.png)

`  `http://localhost:8080/v1/users/{id}/properties

请求示例：

`  `http://localhost:8080/v1/users/12585/properties?propertyKey=key3

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|propertyKey|否|补充信息的代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.138.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 08:21:57 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：   no content

48. 删除用户的某一补充信息

使用本接口根据用户主键id和补充信息的主键id删除用户的某一条补充信息。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.139.png)

`  `http://localhost:8080/v1/users/{id}/properties/{propertyId}

请求示例：

`  `http://localhost:8080/v1/users/12585/properties/5![ref29]

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|登录用户id|
|propertyId|否|补充信息的主键id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.140.png)

`  `{

`  `"date": "Mon, 06 Feb 2017 08:27:30 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：   no content

49. 查询未设置**username**的用户信息

使用本接口可以查询未设置username的用户信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.141.png)

`  `http://localhost:8080/v1/users/nousername

请求示例：

`  `http://localhost:8080/v1/users/nousername

参数说明



|参数|是否必须|说明|
| - | - | - |
|name|否|姓名|
|phoneno|否|手机号码|
|idcard|否|身份证号码|



|pageBound|否|pageBound|
| - | - | - |

返回说明

正确情况下返回HTTP头： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.142.png){

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8089",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Thu, 25 Jan 2018 03:14:09 GMT",

`  `"": ""

} 正确情况下返回代码：

200 正确情况下返回内容：

{

`  `"total": 1,   "count": 1,   "items": [     {

`      `"userName": null,       "name": "张文莉",

`      `"email": null,

`      `"phoneNo": "13984323656",       "officePhone": null,

`      `"gender": "2",

`      `"grade": 43,

`      `"type": 1,

`      `"status": "1",

`      `"chgPassFlag": "1",

`      `"orgs": "D2977:4",

`      `"employeeNumber": "90F1722916",       "idcard": "520102199002205829",

`      `"userMaps": null,

`      `"jobCode": "J10473",       "jobName": "客户经理",

`      `"jobGroupCode": "25",

`      `"jobGroupName": "客户经理",       "jobStatus": "1",

`      `"empStatus": "1",

`      `"workPlace": "贵州贵阳",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",![ref49]

`      `"partOrgs": null,

`      `"groupFullName": "陕西分公司/营销部",   //部门全称

`      `"properties": [

`        `{

`          `"id": 57903,

`          `"userId": 26215,

`          `"userKey": "eid",

`          `"userValue": "18914",

`          `"employeeNumber": "90F1722916"         },

`        `{

`          `"id": 58253,

`          `"userId": 26215,

`          `"userKey": "emptype",

`          `"userValue": "1",

`          `"employeeNumber": "90F1722916"         },

`        `{

`          `"id": 58603,

`          `"userId": 26215,

`          `"userKey": "contract",           "userValue": "是",

`          `"employeeNumber": "90F1722916"

`        `}

`      `],

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.6.94:8089/v1/users/26215"         }

`      `},

`      `"id": 26215

`    `}

`  `]

} 异常情况下返回：

401 Unauthorized 403 Forbidden 404 Not Found

50. 解锁用户

用户登陆时，密码错误次数超过限制，会将该用户置为锁定状态，在一定的时间 内不允许登陆，使用本接口可以根据主键id解锁用户。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.143.png)

`  `http://localhost:8080/v1/users/{id}/lock

请求示例：

`  `http://localhost:8080/v1/users/1/lock

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|用户主键id|

返回说明

正确情况下返回：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.144.png)

204 错误情况下返回：

401 Unauthorized 403 Forbidden

51. 修改密码（不需要原密码）

不需要原密码情况下，根据用户关键信息（用户名/身份证/邮箱/手机号/员工编 号）修改密码，传入格式为明文或密文 。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![ref52]

`  `http://localhost:8080/v1/users/password/withoutcheck

请求示例：

`  `http://localhost:8080/v1/users/password/withoutcheck? loginKey=zhangkh6&newPassword=111111

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|用户名/身份证/邮箱/手机 号/员工编号|



|newPassword|是|新密码|
| - | - | - |
|encrypt|否|密码传入方式：明文或 密文（MD5时，传入密 码为32位小写），传入 空时默认为明文|

返回说明

正确情况下返回内容：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.146.png)

1 正确情况下返回代码：

201 错误情况下返回其他内容及代码：

401 Unauthorized 403 Forbidden 404 Not Found

例如：

{

`  `"code": "404",

`  `"timestamp": "2018-08-27 13:14:30",

`  `"path": "/v1/users/password/withoutcheck",   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户"

}

52. 获取用户详情

根据用户id获取用户详情。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.147.png)

`  `http://localhost:8080/v1/users/{id}/detail

请求示例：

`  `http://localhost:8080/v1/users/7785/detail

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|用户id|

返回说明

正确情况下返回内容： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.148.png){

`    `"userId": 5816,

`    `"cardType": "1",   //证件类型

`    `"salaryComp": null,   //工资单位

`    `"salaryDep": null,   //工资部门

`    `"adjustComp": null,  //核算单位

`    `"adjustDep": null, //核算部门

`    `"birthday": "1985-06-12",   //出生年月

`    `"country": 41,         //国籍

`    `"agentName": "中泰证券",//扣缴义务人名称

`    `"agentNo": "123",//扣缴义务人识别号

`    `"agentChangeDate": "2019-03-08"//变更时间

} 错误返回码详见2.7节内容。

53. 查询个税扣除项

查询个税扣除项

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.149.png)

`  `http://localhost:8080/v1/users/freetax

请求示例：

`  `http://localhost:8080/v1/users/freetax

参数说明



|参数|是否必须|类型|说明|
| - | - | - | - |
|page|是|int|页码，从1开始|
|size|是|int|每页数据量|



|startDate|否|String|开始日期，格式 YYYY-MM，默认 为当月，不传开始 日期和结束日期返 回当月的数据|
| - | - | - | :- |
|endDate|否|String|结束日期，格式 YYYY-MM，默认 为当月|

返回说明



|属性|类型|说明|
| - | - | - |
|name|String|姓名|
|idcard|String|身份证号码|
|month|String|月份,格式YYYY-MM|
|childrenPayment|float|子女教育|
|educationPayment|float|继续教育|
|loadInterest|float|住房贷款利息|
|rent|float|住房租金|
|elderPayment|float|赡养老人费用|
|count|int|返回记录数，0代表已经 没有数据|

示例 ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.150.png)/v1/users/freetax?page=1&size=500

{

`    `"total": 2,

`    `"count": 2,

`    `"items": [

`        `{

`            `"name": "张三",

`            `"idcard": 3711111111,             "month": 2018-09,

`            `"childrenPayment": 1000,             "educationPayment": 400,             "loadInterest": 1000,

`            `"rent": 0,![ref41]

`            `"elderPayment": 1000         },

`        `{

`            `"name": "李四",

`            `"idcard": 370001122,             "month": 2018-09,

`            `"childrenPayment": 0,

`            `"educationPayment": 300,             "loadInterest": 1000,

`            `"rent": 0,

`            `"elderPayment": 1000

`        `}

`    `]

}

2. 组**GROUPS**
1. 获取组列表

使用本接口获取组的分页信息，支持条件过滤。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.151.png)

`  `http://localhost:8080/v1/groups

请求示例：

`  `http://localhost:8080/v1/groups?groupName=li501

参数说明



|参数|是否必须|说明|
| - | - | - |
|groupNumber|否|组编号,长度至少4位|
|groupName|否|组名称|
|groupType|否|组类型 (1:ORG;2:IDENTIFY;3:L|
|subType|否|组类型细类(1:总公司 2、分公司 3、营业 部、4、一级部门、5、|

OGIC)



|||二级部门、6、工作组、 9、其他 )|
| :- | :- | :- |
|status|否|状态状态(1:启用，0:未 启用)|
|description|否|描述|
|id|否|组id，精确查询|
|createPerson|否|创建人|
|createTime|否|创建时间，查询在此时 间（含）之后的数据|
|updatePerson|否|修改人|
|updateTime|否|修改时间，查询在此时 间（含）之后的数据|
|remark|否|备注说明，支持模糊查 询|
|pageBound|否|分页查询条件， 不录入时，默认 page=1&size=100，即 约定每一页显示100条数 据，显示当前第1页|

返回说明

正确情况下的返回HTTP头如下： ![ref53]   {

`      `"date": "Sat, 04 Feb 2017 06:12:00 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下的返回JSON数据包如下：     {

`      `"total": 1,

`      `"count": 1,

`      `"items": [

`        `{

`          `"parents": "",

`          `"groupNumber": "pli501",![ref27]

`          `"groupName": "li501",

`          `"groupType": "2",

`          `"subType": null,

`          `"status": "1",

`          `"description": "用户li501的识别组",

`          `"datasource": "API",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/groups/10238"             }

`          `},

`          `"id": 10238

`        `}

`      `]

`    `}

查询结果为空的情况下的返回JSON数据包示例如下:

`    `{

`      `"total": 0,       "count": 0,       "items": []     }

2. 查询组(根据主键**id)**

使用本接口可以根据组的主键查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref54]

`  `http://localhost:8080/v1/groups/{id}

请求示例：

`  `http://localhost:8080/v1/groups/55

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|

返回说明

正确情况下的返回HTTP头如下:![ref55]

`    `{![ref46]

`      `"date": "Tue, 07 Feb 2017 02:19:06 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下的返回的JSON数据包如下：     {

`      `"parents": "",

`      `"groupNumber": "testliu",

`      `"groupName": "string",

`      `"groupType": "1",

`      `"subType": "1",

`      `"status": "1",

`      `"description": "string",

`      `"datasource": "API",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/groups/14311"         }

`      `},

`      `"id": 14311

`    `}

错误情况下的返回JSON数据包如下：      {

`      `"timestamp": 1486434078506,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[1411111]对应的组不存在！",

`      `"path": "/v1/groups/1411111"

`    `}

3. 新增组

使用本接口新增组。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref47]

`  `http://localhost:8080/v1/groups![ref48]

请求示例：

`  `http://localhost:8080/v1/groups

参数说明



|参数|是否必须|说明|
| - | - | - |
|groupModel|否|组信息，传送格式 为**json**，可传入 createOrg，createPerson|

，createTime，description，groupName，groupNumber，groupType，id，remark，status，subType，updateOrg，updatePerson，updateTime

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.155.png) {

`   `"createOrg": "string",

`   `"createPerson": 0,

`   `"createTime": "2017-02-04T06:44:26.802Z",    "description": "string",

`   `"groupName": "string",

`   `"groupNumber": "string",

`   `"groupType": "string",

`   `"id": 0,

`   `"remark": "string",

`   `"status": "string",

`   `"subType": "string",

`   `"updateOrg": "string",

`   `"updatePerson": 0,

`   `"updateTime": "2017-02-04T06:44:26.802Z"  }

返回说明

正确情况下的返回HTTP头如下： ![ref28]    {

`      `"date": "Sat, 04 Feb 2017 07:37:50 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的JSON数据包如下：     {

`      `"parents": "",

`      `"groupNumber": "string",![ref46]

`      `"groupName": "string",

`      `"groupType": "0",

`      `"subType": "0",

`      `"status": "0",

`      `"description": "string",

`      `"datasource": "API",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/groups/14308"         }

`      `},

`      `"id": 14308

`    `}

错误情况下的返回JSON数据包如下：

`    `{

`      `"timestamp": 1486194024835,

`      `"status": 500,

`      `"error": "Internal Server Error",

`      `"exception":   

` `"org.springframework.dao.DataIntegrityViolationException",

`      `"message": "\r\n### Error updating database.  Cause:

` `org.postgresql.util.PSQLException: ERROR: value too long

` `for type character varying(1)\r\n### The error may involve

` `com.zts.user.data.mappers.GroupMapper.insert-Inline\r\n### The

` `error occurred while setting parameters\r\n### SQL: INSERT INTO  t\_group  (group\_number, group\_name, group\_type, status, sub\_type,  remark, description, create\_time, create\_person, update\_time,

` `datasource, update\_person) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?,  'API', ?)\r\n### Cause: org.postgresql.util.PSQLException: ERROR:  value too long for type character varying(1)\n; SQL []; ERROR:

` `value too long for type character varying(1); nested exception is  org.postgresql.util.PSQLException: ERROR: value too long for type  character varying(1)",

`      `"path": "/v1/groups"

`    `}

4. 删除组

使用本接口可以根据组的主键删除组。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref47]

`  `http://localhost:8080/v1/groups/{id}![ref48]

请求示例：

`  `http://localhost:8080/v1/groups/14023

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|

返回说明

正确情况下的返回HTTP头如下: ![ref56]    {

`      `"date": "Tue, 07 Feb 2017 05:16:27 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下的返回的结果如下：

`    `no content

错误情况下的返回JSON数据包如下：     {

`      `"timestamp": 1486444657130,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[1431388]对应的组不存在！",

`      `"path": "/v1/groups/1431388"

`    `}

**4.2.5**修改组

使用本接口可以根据组的主键更新组信息。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.157.png)

`  `http://localhost:8080/v1/groups/{id}

请求示例：

`  `http://localhost:8080/v1/groups/55![ref29]

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupModel|是|要修改的组信息项及对 应修改的值，接收数据 格式为**json**，可以传入 createOrg，createPerson|

，createTime，description，groupName，groupNumber，groupType，id，remark，status，subType，updateOrg，updatePerson，updateTime

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.158.png){

`  `"createOrg": "test",

`  `"createPerson": 0,

`  `"createTime": "2017-02-07T01:42:59.168Z",   "description": "string",

`  `"groupName": "string",

`  `"groupNumber": "testliu",

`  `"groupType": "1",

`  `"id": 6467,

`  `"remark": "1",

`  `"status": "1",

`  `"subType": "1",

`  `"updateOrg": "test",

`  `"updatePerson": 0,

`  `"updateTime": "2017-02-07T01:42:59.168Z" }

返回说明

正确情况下的返回HTTP头如下: ![ref57]    {

`      `"date": "Tue, 07 Feb 2017 02:24:46 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下的返回的结果如下：

`  `1![ref58]

错误情况下的返回JSON数据包如下：     {

`      `"timestamp": 1486434286472,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "不允许手动修改IDENTIFY类型的组",

`      `"path": "/v1/groups/14311"

`    `}

6. 获取组全景信息(根据主键**id)**

使用本接口可以根据主键id获取组全景信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref54]

`  `http://localhost:8080/v1/groups/{id}/profile

请求示例：

`  `http://localhost:8080/v1/groups/55/profile

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|

返回说明

正确情况下的返回HTTP头如下: ![ref59]    {

`      `"date": "Tue, 07 Feb 2017 05:31:16 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的JSON数据包如下：     {

`      `"parents": "",

`      `"groupNumber": "testliu",![ref60]

`      `"groupName": "string",

`      `"groupType": "1",

`      `"status": "1",

`      `"description": "string",

`      `"datasource": "API",

`      `"properties": {

`        `"total": 0,

`        `"count": 0,

`        `"items": []

`      `},

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/groups/14311"         }

`      `},

`      `"id": 14311

`    `}

错误情况下返回的JSON数据包如下：

`    `{

`      `"timestamp": 1486445535372,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[143115]对应的组不存在！",

`      `"path": "/v1/groups/143115/profile"

`    `}

7. 获取组全景信息(根据**groupNumber)**

使用本接口根据组编号查询组的全景视图信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref61]

`  `http://localhost:8080/v1/groups/profile

请求示例：

`  `http://localhost:8080/v1/groups/profile?groupNumber=0

参数说明



|参数|是否必须|说明|
| - | - | - |



|groupNumber|是|组编号|
| - | - | - |

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.164.png)    {

`      `"date": "Mon, 06 Feb 2017 07:17:56 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的JSON数据包如下：

`    `{

`      `"parents": "",

`      `"groupNumber": "pli501",

`      `"groupName": "li501",

`      `"groupType": "2",

`      `"status": "1",

`      `"description": "用户li501的识别组",

`      `"datasource": "API",

`      `"properties": {

`        `"total": 0,

`        `"count": 0,

`        `"items": []

`      `},

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/groups/10238"         }

`      `},

`      `"id": 10238

`    `}

错误情况下的返回JSON数据包如下：

`    `{

`      `"timestamp": 1486365587336,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "groupNumber[501]对应的组不存在！",

`      `"path": "/v1/groups/profile"

`    `}

8. 获取组全景信息(根据条件模糊查询)

使用本接口根据查询条件查询组的全景视图信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref43]

`  `http://localhost:8080/v1/groups/withprofile

请求示例：

`  `http://localhost:8080/v1/groups/withprofile?groupName=光明路营业部

参数说明



|参数|是否必须|说明|
| - | - | - |
|groupNumber|否|组编号,长度至少4位|
|groupName|否|组名称|
|groupType|否|组类型 (1:ORG;2:IDENTIFY;3:L|
|subType|否|组类型细类(1:总公司 2、分公司 3、营业 部、4、一级部门、5、 二级部门、6、工作组、 9、其他 )|
|status|否|状态状态(1:启用，0:未 启用)|
|description|否|描述|
|id|否|组id，精确查询|
|createPerson|否|创建人|
|createTime|否|创建时间，查询在此时 间（含）之后的数据|
|updatePerson|否|修改人|
|updateTime|否|修改时间，查询在此时 间（含）之后的数据|

OGIC)



|remark|否|备注说明，支持模糊查 询|
| - | - | :- |
|pageBound|否|分页查询条件， 不录入时，默认 page=1&size=100，即 约定每一页显示100条数 据，显示当前第1页|

返回说明

正确情况下的返回HTTP头如下: ![ref21]    {

`      `"date": "Mon, 06 Feb 2017 07:17:56 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的JSON数据包如下：

`    `{

`  `"total": 1,   "count": 1,   "items": [     {

`      `"parents": "0044:11",

`      `"groupNumber": "0283",

`      `"groupName": "光明路营业部",       "groupType": "1",

`      `"status": "1",

`      `"description": null,

`      `"datasource": null,

`      `"properties": {

`        `"total": 5,

`        `"count": 5,

`        `"items": [

`          `{

`            `"groupKey": "divisioncode",

`            `"groupValue": "2110",

`            `"groupNumber": "0283",

`            `"\_links": {

`              `"self": {

`                `"href": "http://localhost:8080/v1/groups/6490"

`              `}![ref11]

`            `},

`            `"id": 6490

`          `},

`          `{

`            `"groupKey": "depproperty",

`            `"groupValue": "2",

`            `"groupNumber": "0283",

`            `"\_links": {

`              `"self": {

`                `"href": "http://localhost:8080/v1/groups/777"               }

`            `},

`            `"id": 777

`          `},

`          `{

`            `"groupKey": "deptype",

`            `"groupValue": null,

`            `"groupNumber": "0283",

`            `"\_links": {

`              `"self": {

`                `"href": "http://localhost:8080/v1/groups/776"               }

`            `},

`            `"id": 776

`          `},

`          `{

`            `"groupKey": "depgrade",

`            `"groupValue": "2",

`            `"groupNumber": "0283",

`            `"\_links": {

`              `"self": {

`                `"href": "http://localhost:8080/v1/groups/775"               }

`            `},

`            `"id": 775

`          `},

`          `{

`            `"groupKey": "orgid",

`            `"groupValue": "2110",

`            `"groupNumber": "0283",

`            `"\_links": {

`              `"self": {

`                `"href": "http://localhost:8080/v1/groups/5892"               }

`            `},

`            `"id": 5892![ref24]

`          `}

`        `]

`      `},

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/groups/259"         }

`      `},

`      `"id": 259

`    `}

`  `]

}

9. 查询组间关系

使用本接口获取组与组、组与人、组与逻辑组之间的关系。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref10]

`  `http://localhost:8080/v1/groups/relations

请求示例：

`  `http://localhost:8080/v1/groups/relations?groupNumber=SYST545

参数说明



|参数|是否必须|说明|
| - | - | - |
|groupNumber|是|组编号|
|groupType|否|与当前组相 关的组的类型 （1:ORG;2:IDENTIFY;3: 默认为空，即返回与当 前组相关的所有类型的 组|

LOGIC），

返回说明

正确情况下的返回HTTP头如下: ![ref34]    {

`  `"date": "Tue, 07 Feb 2017 06:15:33 GMT",![ref11]

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下的返回JSON数据包如下：

`    `{

`  `"total": 2,

`  `"count": 2,

`  `"items": [

`    `{

`      `"groupId": 14307,

`      `"memberId": 7147,

`      `"orderNum": 0,

`      `"groupNumber": "SYST545",

`      `"groupName": "T545",

`      `"memberNumber": "p7147",

`      `"memberName": "lily",

`      `"memberType": "2",

`      `"employeeNumber": "82F1618259",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/groups/100057"         }

`      `},

`      `"id": 100057

`    `},

`    `{

`      `"groupId": 14307,

`      `"memberId": 14310,

`      `"orderNum": 0,

`      `"groupNumber": "SYST545",

`      `"groupName": "T545",

`      `"memberNumber": "ptomy",

`      `"memberName": "tomy",

`      `"memberType": "2",

`      `"employeeNumber": "5512685",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/groups/100059"         }

`      `},

`      `"id": 100059

`    `}

`  `]

}![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.165.png)

查询结果为空的情况下返回的JSON数据包如下：     {

`      `"total": 0,       "count": 0,       "items": []     }

10. 修改组内排序

使用本接口可以修改组内排序。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.166.png)

`  `http://localhost:8080/v1/groups/relations

请求示例：

`  `http://localhost:8080/v1/groups/relations

参数说明



|参数|是否必须|说明|
| - | - | - |
|groupRelationModels|是|可输入组的上下级关系 的id以及排序码|

返回说明

正确情况下的返回HTTP头如下:![ref62]

{

`  `"date": "Tue, 07 Feb 2017 06:20:56 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8" }

正确情况下的返回的结果如下：

`  `1

错误情况下的返回JSON数据包如下：

`    `{![ref39]

`      `"timestamp": 1486433819932,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "未找到相应的对照关系，无法完成更新操作",

`      `"path": "/v1/groups/relations"

`    `}

11. 查询子级组

使用本接口可以根据组的主键id，查询该组的下级组，支持组类型和范围条件查 询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.168.png)

`  `http://localhost:8080/v1/groups/{id}/children

请求示例：

`  `http://localhost:8080/v1/groups/1/children

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupType|否|组的类型 （1:ORG;2:IDENTIFY;3:|
|supper|否|组的范围，supper=true 时，除返回该组的直接 下级组，还递归返回该 下级组的所有下级组|

LOGIC）

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.169.png)    {

`      `"date": "Tue, 07 Feb 2017 02:43:07 GMT",       "server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.170.png)

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下的返回的JSON数据包如下：     {

`      `"total": 1,

`      `"count": 1,

`      `"items": [

`        `{

`          `"parents": "testliu:0",

`          `"groupNumber": "ctestliu",

`          `"groupName": "ccccc",

`          `"groupType": "1",

`          `"subType": "1",

`          `"status": "1",

`          `"description": "childg",

`          `"datasource": "API",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/groups/14313"             }

`          `},

`          `"id": 14313

`        `}

`      `]

`    `}

查询结果为空的情况下返回JSON数据包如下：

`    `{

`      `"total": 0,       "count": 0,       "items": []     }

12. 批量新增子级组

使用本接口可以根据组的主键id，新增该组的下级组(这里只建立组的关联关系， 前提是下级组均已创建)。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref63]

`  `http://localhost:8080/v1/groups/{id}/children

请求示例：![ref22]

`  `http://localhost:8080/v1/groups/1/children

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupModels|否|新增的子级组集 合，传送格式为 **json**，可以传入 createOrg，createPerson|

，createTime，description，groupName，groupNumber，groupType，id，remark，status，subType，updateOrg，updatePerson，updateTime

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.172.png)    [

`       `{

`         `"createOrg": "test",

`         `"createPerson": 0,

`         `"createTime": "2017-02-07T01:42:59.168Z",          "description": "childg",

`         `"groupName": "ccccc",

`         `"groupNumber": "ctestliu",

`         `"groupType": "1",

`         `"id": 14313,

`         `"remark": "1",

`         `"status": "1",

`         `"subType": "1",

`         `"updateOrg": "test",

`         `"updatePerson": 0,

`         `"updateTime": "2017-02-07T01:42:59.168Z"        }

`   `]

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.173.png)    {

`      `"date": "Tue, 07 Feb 2017 02:40:30 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.174.png)

正确情况下的返回的结果如下：

`    `1

错误情况下的返回JSON数据包如下：     {

`      `"timestamp": 1486435230031,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[64675]对应的组不存在！",

`      `"path": "/v1/groups/14311/children"

`    `}

13. 删除子级组

使用本接口可以根据组的主键id和子级组的主键id，删除该组的某个子级组。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.051.png)

`  `http://localhost:8080/v1/groups/{id}/children/{groupid}

请求示例：

`  `http://localhost:8080/v1/groups/1/children/63

参数说明 返回说明

正确情况下的返回HTTP头如下:![ref59]

`  `{

`  `"date": "Tue, 07 Feb 2017 06:29:13 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下返回的结果如下：

`  `1

错误情况下的返回JSON数据包如下： ![ref39]  {

`  `"timestamp": 1486448953222,   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "不存在此上下级关系！",

`  `"path": "/v1/groups/14037/parents/33"

}

14. 批量删除子级组

使用本接口可以根据组的主键id，删除该组的多个子级组。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.175.png)

`  `http://localhost:8080/v1/groups/{id}/children

请求示例：

`  `http://localhost:8080/v1/groups/55/children

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupModel|是|要删除的子级组的集合 （子级组的主键**id**必须 提供），数据传入格 式为**json**，可以传入 createOrg，createPerson|

，createTime，description，groupName，groupNumber，groupType，id，remark，status，subType，updateOrg，updatePerson，updateTime

参数示例如下： ![ref57]   [

`       `{

`         `"createOrg": "test",

`         `"createPerson": 0,

`         `"createTime": "2017-02-07T01:42:59.168Z",          "description": "childg",

`         `"groupName": "ccccc",

`         `"groupNumber": "ctestliu",

`         `"groupType": "1",

`         `"id": 14313,![ref19]

`         `"remark": "1",

`         `"status": "1",

`         `"subType": "1",

`         `"updateOrg": "test",

`         `"updatePerson": 0,

`         `"updateTime": "2017-02-07T01:42:59.168Z"        }

`   `]

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.176.png)    {

`      `"date": "Tue, 07 Feb 2017 02:45:23 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下的返回的结果如下：

`    `no content

错误情况下的返回JSON数据包如下：     {

`      `"timestamp": 1486435583975,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "不存在此上下级关系！",

`      `"path": "/v1/groups/14311/children"

`    `}

15. 查询父级组

使用本接口可以根据组的主键id查询该组的上级组，支持组类型和范围条件查 询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref64]

`  `http://localhost:8080/v1/groups/{id}/parents

请求示例：

`  `http://localhost:8080/v1/groups/55/parents![ref29]

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupType|否|组的类型 （1:ORG;2:IDENTIFY;3:|
|supper|否|组的范围，supper=true 时，除返回该组的直接 上级组，还递归返回其 上级组的所有上级组|

LOGIC）

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.178.png)    {

`      `"date": "Tue, 07 Feb 2017 03:04:11 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的JSON数据包如下：

`    `{

`      `"total": 1,

`      `"count": 1,

`      `"items": [

`        `{

`          `"parents": "",

`          `"groupNumber": "ptestliu",

`          `"groupName": "ppppp",

`          `"groupType": "1",

`          `"subType": "1",

`          `"status": "1",

`          `"description": "parentgroup",

`          `"datasource": "API",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/groups/14312"             }

`          `},![ref58]

`          `"id": 14312

`        `}

`      `]

`    `}

查询结果为空的情况下返回的JSON数据包如下：

`    `{

`      `"total": 0,       "count": 0,       "items": []     }

16. 新增父级组

使用本接口可以根据组的主键id，新增该组的父级组(这里只建立组的关联关系， 前提是父级组均已创建)。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.179.png)

`  `http://localhost:8080/v1/groups/{id}/parents

请求示例：

`  `http://localhost:8080/v1/groups/55/parents

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupModels|否|新增的父级组集 合，传送格式为 **json**，可以传入 createOrg，createPerson|

，createTime，description，groupName，groupNumber，groupType，id，remark，status，subType，updateOrg，updatePerson，updateTime

参数示例如下： ![ref20]   [

`       `{

`         `"createOrg": "test",

`         `"createPerson": 0,

`         `"createTime": "2017-02-07T01:42:59.168Z",          "description": "parentgroup",

`         `"groupName": "ppppp",

`         `"groupNumber": "ptestliu",![ref32]

`         `"groupType": "1",

`         `"id": 14312,

`         `"remark": "1",

`         `"status": "1",

`         `"subType": "1",

`         `"updateOrg": "test",

`         `"updatePerson": 0,

`         `"updateTime": "2017-02-07T01:42:59.168Z"        }

`   `]

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.180.png)    {

`      `"date": "Tue, 07 Feb 2017 02:58:59 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的结果如下：

`    `1

错误情况下返回的JSON数据包如下：     {

`      `"timestamp": 1486436290787,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[64675]对应的组不存在！",

`      `"path": "/v1/groups/14311/parents"

`    `}

17. 删除父级组

使用本接口可以根据组的主键id和父级组的主键id，删除该组的某个父级组。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref47]

`  `http://localhost:8080/v1/groups/{id}/parents/{groupid}![ref48]

请求示例：

`  `http://localhost:8080/v1/groups/55/parents/23

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupId|是|父级组的主键id|

返回说明

正确情况下的返回HTTP头如下: ![ref65]    {

`      `"date": "Tue, 07 Feb 2017 03:07:09 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的结果如下：     no content

错误情况下返回的JSON数据包如下：     {

`      `"timestamp": 1486436867598,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "不存在此上下级关系！",

`      `"path": "/v1/groups/14311/parents/14318"

`    `}

18. 批量删除父级组

使用本接口可以根据组的主键id和子级组的主键id，删除该组的某个子级组。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref66]

`  `http://localhost:8080/v1/groups/{id}/parents![ref23]

请求示例：

`  `http://localhost:8080/v1/groups/55/parents

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupModels|否|要删除的父级组的 集合（主键**id**必须提 供），传入数据格式 为**json**，可以传入 createOrg，createPerson|

，createTime，description，groupName，groupNumber，groupType，id，remark，status，subType，updateOrg，updatePerson，updateTime

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.183.png)   [

`       `{

`         `"createOrg": "test",

`         `"createPerson": 0,

`         `"createTime": "2017-02-07T01:42:59.168Z",          "description": "parentgroup",

`         `"groupName": "ppppp",

`         `"groupNumber": "ptestliu",

`         `"groupType": "1",

`         `"id": 14312,

`         `"remark": "1",

`         `"status": "1",

`         `"subType": "1",

`         `"updateOrg": "test",

`         `"updatePerson": 0,

`         `"updateTime": "2017-02-07T01:42:59.168Z"        }

`   `]

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.184.png)    {

`      `"date": "Tue, 07 Feb 2017 03:12:22 GMT",       "server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",![ref33]

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的结果如下：     no content

错误情况下返回的JSON数据包如下：     {

`      `"timestamp": 1486437142252,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "不存在此上下级关系！",

`      `"path": "/v1/groups/14311/parents"

`    `}

19. 批量删除权限

使用本接口可以根据组的主键id，批量删除组的所有权限信息。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref31]

`  `http://localhost:8080/v1/groups/{id}/permissions

请求示例：

`  `http://localhost:8080/v1/groups/14037/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|groupId|是|父级组的主键id|

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.185.png)    {

`      `"date": "Tue, 07 Feb 2017 04:10:57 GMT",       "server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",![ref3]

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的结果如下：     no content

20. 查询组的权限

使用本接口可以根据组的主键id查询该组的权限，可根据权限层次和权限查询范 围进行条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.109.png)

`  `http://localhost:8080/v1/groups/{id}/permissions

请求示例：

`  `http://localhost:8080/v1/groups/1/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|appCode|否|应用系统的代码|
|level|否|权限层次（1:准入2:功能 菜单3：按钮）|
|supper|否|权限范围，supper=true 时，除返回该组的直接 权限，还递归返回该组 的上级组的权限|

返回说明

正确情况下的返回HTTP头如下: ![ref40]    {

`      `"date": "Tue, 07 Feb 2017 03:26:01 GMT",       "server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",![ref67]

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的JSON数据包如下：     {

`      `"total": 1,

`      `"count": 1,

`      `"items": [

`        `{

`          `"permissionName": "testn",

`          `"permissionNumber": "testn",

`          `"permissionType": "1",

`          `"status": "1",

`          `"applicationId": 20,

`          `"permissionLevel": "2",

`          `"url": "string",

`          `"source": "FALSE",

`          `"sourceDescription": "直接拥有",

`          `"applicationCode": "M0020",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/groups/34"             }

`          `},

`          `"id": 34

`        `}

`      `]

`    `}

查询为空的情况下返回的JSON数据包如下：

`    `{

`      `"total": 0,       "count": 0,       "items": []     }

21. 批量授予权限

使用本接口可以根据组的主键id,为该组授予一个或多个权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref63]

`  `http://localhost:8080/v1/groups/{id}/permissions

请求示例：![ref22]

`  `http://localhost:8080/v1/groups/14037/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|permissionModels|是|权限集合，数 据传入格式为 **json**，applicationId，cre|

ateOrg，createPerson，createTime，description，id，permissionLevel，permissionName，permissionNumber，permissionType，remark，status，updateOrg，updatePerson，updateTime，url

参数示例如下： ![ref68]   [

`       `{

`       `"applicationId": 20,

`       `"createOrg": "test",

`       `"createPerson": 0,

`       `"createTime": "2017-02-07T02:30:16.948Z",        "description": "testquanxian",

`       `"id": 34,

`       `"permissionLevel": "2",

`       `"permissionName": "testn",

`       `"permissionNumber": "testn",

`       `"permissionType": "1",

`       `"remark": "1",

`       `"status": "1",

`       `"updateOrg": "test",

`       `"updatePerson": 0,

`       `"updateTime": "2017-02-07T02:30:16.948Z",        "url": "string"

`     `}

`   `]

返回说明

正确情况下的返回HTTP头如下: ![ref69]    {

`      `"date": "Tue, 07 Feb 2017 03:23:31 GMT",       "server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }![ref70]

正确情况下返回的结果如下：     1

错误情况下返回的JSON数据包如下：、     {

`      `"timestamp": 1486437761349,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[123456]对应的权限不存在！",

`      `"path": "/v1/groups/14311/permissions"

`    `}

22. 删除组权限

使用本接口可以根据组的主键id，批量删除组的某一指定权限信息。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref31]

`  `http://localhost:8080/v1/groups/{id}/permissions/{permissionId}

请求示例：

`  `http://localhost:8080/v1/groups/14037/permissions/5

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|permissionId|是|权限的主键id|

返回说明

正确情况下的返回HTTP头如下: ![ref71]    {

`      `"date": "Tue, 07 Feb 2017 04:12:39 GMT",       "server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }![ref25]

正确情况下返回的结果如下：     no content

23. 查询组的某权限

使用本接口可以根据组的主键id和权限的主键id查询该组是否拥有指定某一权 限，若有则返回该权限信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.191.png)

`  `http://localhost:8080/v1/groups/{id}/permissions/{permissionId}

请求示例：

`  `http://localhost:8080/v1/groups/1/permissions/23

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|permissionId|是|权限的主键id|

返回说明

正确情况下的返回HTTP头如下: ![ref51]    {

`      `"date": "Tue, 07 Feb 2017 05:22:27 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的JSON数据包如下：     {

`      `"permissionName": "testn",       "permissionNumber": "testn",       "permissionType": "1",

`      `"status": "1",![ref72]

`      `"applicationId": 20,

`      `"permissionLevel": "2",

`      `"url": "string",

`      `"source": "FALSE",

`      `"sourceDescription": "直接拥有",

`      `"applicationCode": "M0020",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/groups/34"         }

`      `},

`      `"id": 34

`    `}

错误情况下返回的JSON数据包如下：

`    `{

`      `"timestamp": 1486444890071,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "未找到用户的权限",

`      `"path": "/v1/groups/14311/permissions/34"

`    `}

24. 授予指定权限

使用本接口可以根据组的主键id和权限的主键id,为该组授予某一指定权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.193.png)

`  `http://localhost:8080/v1/groups/{id}/permissions/{permissionId}

请求示例：

`  `http://localhost:8080/v1/groups/55/permissions/3

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|permissionId|是|权限的主键id|

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.194.png)    {

`      `"date": "Tue, 07 Feb 2017 05:27:22 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的结果如下：     1

错误情况下返回的JSON数据包如下：     {

`      `"timestamp": 1486445206871,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "id[55]对应的权限不存在！",

`      `"path": "/v1/groups/14311/permissions/55"

`    `}

25. 删除组的补充信息

使用本接口可以根据组的主键id，批量删除该组的补充信息，也可提供信息项代 码删除该指定信息。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.195.png)

`  `http://localhost:8080/v1/groups/{id}/properties

请求示例：

`  `http://localhost:8080/v1/groups/55/properties?propertyKey=key1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|



|propertyKey|否|补充信息的代码|
| - | - | - |

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.196.png)    {

`      `"date": "Tue, 07 Feb 2017 05:31:16 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的消息体如下：

`    `no content

26. 查询组的补充信息

根据组的主键Number查询该组的补充信息，也可进一步根据补充信息项的代码 查询某一补充信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.197.png)

`  `http://localhost:8080/v1/groups/properties

请求示例：

`  `http://localhost:8080/v1/groups/properties

参数说明



|参数|是否必须|说明|
| - | - | - |
|groupNumber|是|组编号|
|propertyKey|否|补充信息的代码|

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.198.png)    {

`      `"date": "Mon, 06 Feb 2017 08:16:05 GMT",       "server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",![ref60]

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的JSON数据包如下：

`    `{

`      `"total": 1,

`      `"count": 1,

`      `"items": [

`        `{

`          `"groupKey": "test",

`          `"groupValue": "testvalue",

`          `"groupNumber": "pli501",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/groups/6207"             }

`          `},

`          `"id": 6207

`        `}

`      `]

`    `}

查询结果为空的情况下返回JSON数据包如下：

`    `{

`      `"total": 0,       "count": 0,       "items": []     }

27. 新增组的补充信息

使用本接口可以根据组的主键id，批量增加或更新该组的补充信息。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref61]

`  `http://localhost:8080/v1/groups/{id}/properties

请求示例：

`  `http://localhost:8080/v1/groups/55/properties

参数说明



|参数|是否必须|说明|
| - | - | - |



|id|是|组的主键id|
| - | - | - |
|groupProperties|是|新增或修改的组的 补充信息，传入格式 为**json**，可以传入 groupId，groupKey，gro|

upNumber，groupValue，id

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.199.png)   [

`     `{

`       `"groupId": 0,

`       `"groupKey": "test",

`       `"groupNumber": "test",

`       `"groupValue": "testvalue",        "id": 0

`     `}

`   `]

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.200.png)    {

`      `"date": "Tue, 07 Feb 2017 02:01:05 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的结果如下：

`    `1

错误的情况下的返回JSON数据包如下：     {

`      `"timestamp": 1486433276873,

`      `"status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"org.springframework.web.method.annotation.MethodArgumentTypeMismatchException",       "message": "Failed to convert value of type 'java.lang.String'

` `to required type 'java.lang.Long'; nested exception is

` `java.lang.NumberFormatException: For input string: \"li\"",

`      `"path": "/v1/groups/li/properties"

`    `}

28. 删除组的补充信息

使用本接口可以根据组的主键id和补充信息的主键id，删除该组的某一指定的补 充信息。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.201.png)

`  `http://localhost:8080/v1/groups/{id}/properties/{propertiesId}

请求示例：

`  `http://localhost:8080/v1/groups/55/properties/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|propertyId|是|补充信息的主键id|

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.202.png)    {

`      `"date": "Tue, 07 Feb 2017 05:44:50 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的结果如下：     no content

29. 获取组的某一项补充信息

使用本接口可以根据组的主键id和补充信息的主键id查询该组某一指定的补充信 息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.203.png)

`  `http://localhost:8080/v1/groups/{id}/properties/{propertiesId}![ref48]

请求示例：

`  `http://localhost:8080/v1/groups/55/properties/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|propertyId|是|补充信息的主键id|

返回说明

正确情况下的返回HTTP头如下: ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.204.png)    {

`      `"date": "Tue, 07 Feb 2017 05:44:50 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"

`    `}

正确情况下返回的JSON数据包如下：

`    `{

`      `"groupKey": "test82",

`      `"groupValue": "testvalue2",

`      `"groupNumber": "testliu",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/groups/5"         }

`      `},

`      `"id": 5

`    `}

错误的情况下的返回JSON数据包如下：

`    `{

`      `"timestamp": 1486446437203,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "未获取到组的属性：99",

`      `"path": "/v1/groups/14311/properties/99"

`    `}

30. 查询组下用户

使用本接口可以根据组的主键id查询该组下的用户，支持用户范围条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.205.png)

`  `http://localhost:8080/v1/groups/{id}/users

请求示例：

`  `http://localhost:8080/v1/groups/55/users

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|组的主键id|
|supper|否|用户范围，supper=true 时，除返回该组的直接 用户，也递归返回该组 的下级组的所有用户|

返回说明

正确情况下的返回HTTP头如下: ![ref38]    {

`      `"date": "Tue, 07 Feb 2017 05:55:01 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"     }

正确情况下返回的JSON数据包如下：     {

`      `"total": 1,

`      `"count": 1,

`      `"items": [

`        `{

`          `"userName": "weihm",           "name": "wtest",

`          `"email": "test",

`          `"phoneNo": "test",

`          `"officePhone": "test",![ref6]

`          `"gender": "2",

`          `"status": "1",

`          `"chgPassFlag": "0",

`          `"orgs": "1427:10",

`          `"employeeNumber": "16702",

`          `"userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"              }

`          `],

`          `"type": 1,

`          `"jobCode": "J10698",

`          `"jobName": "经理",

`          `"jobStatus": "1",

`          `"jobGroupCode": "15",

`          `"jobGroupName": "运营类-信息技术",           "empStatus": "1",

`          `"workPlace": "山东济南",

`          `"phoneShortNo": null,

`          `"officeShortNo": null,

`          `"datasource": "HR",

`          `"partOrgs":       [

`                      `{

`                `"id": 15,

`                `"groupNumber": "0156",                 "jobCode": "301",

`                `"jobName": "分支机构管理岗",                 "jobGroupCode": null,

`                `"jobGroupName": null

`             `},

`                      `{

`                `"id": 36,

`                `"groupNumber": "0155",                 "jobCode": "300",![ref7]

`                `"jobName": "机房管理岗",

`                `"jobGroupCode": null,

`                `"jobGroupName": null

`             `}

`          `],

`          `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 395,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 7788string

`        `},

`        `{

`            `"id": 2,

`            `"userId": 395,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 7788string

`        `}

`    `],

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/users/4043"             }

`          `},

`          `"id": 4043

`       `}

`      `]

`    `}

查询结果为空的情况下的返回JSON数据包如下：

`    `{

`      `"total": 0,       "count": 0,       "items": []     }

3. 权限**permissions**
1. 查询权限列表

使用本接口可以获取权限列表，支持条件过滤。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.206.png)

`  `http://localhost:8080/v1/permissions

请求示例：

`  `http://localhost:8080/v1/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|permissionNumber|否|权限编码|
|permissionName|否|权限名称|
|permissionType|否|权限类型（1:单个权限,2: 组权限）|
|status|否|状态（1:启用，0:未启 用）|
|applicationId|否|权限应用的系统id|
|permissionLevel|否|权限层次（ 1:准入2:功能 菜单3：按钮）|
|url|否|权限路径|

返回说明

正确情况下的返回HTTP头如下：![ref73]

`  `{

`  `"date": "Sat, 04 Feb 2017 08:43:29 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`   `"total": 32,    "count": 32,    "items": [

`    `{![ref11]

`      `"permissionName": "OA系统的权限",       "permissionNumber": "OAXTDQX",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 2,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": null,

`      `"sourceDescription": null,

`      `"applicationCode": "M0002",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/permissions/2"         }

`      `},

`      `"id": 2

`    `},

`    `{

`      `"permissionName": "财富管理平台的权限",

`      `"permissionNumber": "CFGLPTDQX",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 3,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": null,

`      `"sourceDescription": null,

`      `"applicationCode": "M0003",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/permissions/3"         }

`      `},

`      `"id": 3

`    `}

`  `]

` `}

错误情况下的返回HTTP头示例如下：   {

`  `"timestamp": 1486197852516,   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "未找到对应用户",

`  `"path": "/v1/users/applications"   }![ref17]

2. 新增单个权限

使用本接口新增permisson信息，传入参数为json格式。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.208.png)

`  `http://localhost:8080/v1/permissions

请求示例：

`  `http://localhost:8080/v1/permissions

参数说明



|参数|是否必须|说明|
| - | - | - |
|permissionModel|是|用户信息，接收数据 格式为**json**，可传入 permissionNumber、per 等字段|

missionName、permissionType、status、applicationId、permissionLevel、url、description

参数示例如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.209.png) {

`     `"applicationId": 0,

`     `"createOrg": "string",      "createPerson": 0,

`     `"createTime": "2017-02-06T01:13:51.697Z",      "description": "测试权限",

`     `"id": 3,

`     `"permissionLevel": "2",

`     `"permissionName": "权限1",

`     `"permissionNumber": "CS",

`     `"permissionType": "1",

`     `"remark": "",

`     `"status": "",

`     `"updateOrg": "zts",

`     `"updatePerson": 0,

`     `"updateTime": "2017-02-06T01:13:51.697Z",      "url": "string"

}

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.210.png)

`  `{

`      `"date": "Sat, 04 Feb 2017 05:56:47 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"transfer-encoding": "chunked",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`      `"permissionName": "财富管理平台的权限",

`      `"permissionNumber": "CFGLPTDQX",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 3,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": null,

`      `"sourceDescription": null,

`      `"applicationCode": "M0003",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/permissions/3"         }

`      `},

`      `"id": 3

` `}

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486350880671,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "权限校验重复时需传入：应用系统和权限编号",

`      `"path": "/v1/permissions"

`  `}

3. 查询单个权限

根据主键id查询某一权限

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.211.png)

`  `http://localhost:8080/v1/permissions/{id}

请求示例：

`  `http://localhost:8080/v1/permissions/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是||
返回说明

正确情况下的返回HTTP头如下：![ref74]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 05:32:11 GMT"

`  `}

正确情况下的返回结果如下：

`  `{

`      `"permissionName": "财富管理平台的权限",       "permissionNumber": "CFGLPTDQX",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 3,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": null,

`      `"sourceDescription": null,

`      `"applicationCode": "M0003",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/permissions/3"         }

`      `},

`      `"id": 3

`  `}

错误情况下的返回HTTP头示例如下：![ref32]

`  `{

`      `"timestamp": 1486359229263,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数<ID>不能为空",

`      `"path": "/v1/permissions/0"

`  `}

4. 修改单个权限

根据权限主键id修改某个权限。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.213.png)

`  `http://localhost:8080/v1/permissions/{id}

请求示例：

`  `http://localhost:8080/v1/permissions/1

参数说明

支持json和查询参数格式两种类型



|参数|是否必须|说明|
| - | - | - |
|permissionModel|是|可传入 permissionNumber、per 等字段|

missionName、permissionType、status、applicationId、permissionLevel、url、description

返回说明

正确情况下的返回HTTP头如下：![ref69]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 06:04:26 GMT"   }![ref70]

正确情况下的返回结果如下：

`  `1

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486351905060,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "必须传入id ",

`      `"path": "/v1/permissions/0"

`  `}

5. 删除单个权限

根据权限主键id删除某一指定权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.214.png)

`  `http://localhost:8080/v1/permissions/{id}

请求示例：

`  `http://localhost:8080/v1/permissions/1

参数说明

支持两种传参方式：json格式和查询参数格式![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.215.png)



|参数|是否必须|说明|
| - | - | - |
|id|是||
返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.216.png)

`  `{

`      `"date": "Sat, 04 Feb 2017 07:17:07 GMT",![ref12]

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486351905060,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "必须传入id ",

`      `"path": "/v1/permissions/0"

`  `}

6. 批量新增子权限

使用本接口根据权限主键id新增该权限的一个或多个子权限，传入参数为json格 式。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.217.png)

`  `http://localhost:8080/v1/permissions/{id}/children

请求示例：

`  `http://localhost:8080/v1/permissions/1/children

参数说明



|参数|是否必须|说明|
| - | - | - |
|permissionModel|是|用户信息，接收数据 格式为**json**，可传入 permissionNumber、per 等字段|

missionName、permissionType、status、applicationId、permissionLevel、url、description

参数示例如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.218.png)

`  `[![ref75]

`    `{

`     `"applicationId": 0,     "createOrg": "string",     "createPerson": 0,

`    `"createTime": "2017-02-06T01:13:51.697Z",     "description": "测试权限",

`    `"id": 3,

`    `"permissionLevel": "2",

`    `"permissionName": "权限1",

`    `"permissionNumber": "CS",

`    `"permissionType": "1",

`    `"remark": "",

`    `"status": "",

`    `"updateOrg": "zts",

`    `"updatePerson": 0,

`    `"updateTime": "2017-02-06T01:13:51.697Z",     "url": "string"

`   `}

`  `]

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.220.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 03:07:31 GMT"

`  `}

正确情况下的返回数据如下：     1

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486362175971,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "权限[T545-]不是组权限，不能作为上级",

`      `"path": "/v1/permissions/31/children"

`  `}

7. 查询多个子权限

根据权限主键id查询该权限的子权限，支持权限范围条件查询

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.221.png)

`  `http://localhost:8080/v1/permissions/{id}/children

请求示例：

`  `http://localhost:8080/v1/permissions/21/children

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|父权限主键id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.222.png)

`  `{

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Mon, 06 Feb 2017 05:32:11 GMT"

`  `}

正确情况下的返回结果如下：

`  `{

`   `"total": 1,

`   `"count": 1,

`   `items:

`    `[

`      `{

`          `"permissionName": "测试系统权限",           "permissionNumber": "CSXTQX",

`          `"permissionType": "1",

`          `"status": "1",

`          `"applicationId": 24,

`          `"permissionLevel": "1",

`          `"url": null,![ref72]

`          `"source": "FALSE",

`          `"sourceDescription": "直接下级",

`          `"applicationCode": null,

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/permissions/30"             }

`          `},

`         `"id": 3

`      `}

`    `]

` `}

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486359229263,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数<ID>不能为空",

`      `"path": "/v1/permissions/0"

`  `}

8. 批量删除子权限

根据权限主键id删除该权限指定的多个子权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref15]

`  `http://localhost:8080/v1/permissions/{id}/children

请求示例：

`  `http://localhost:8080/v1/permissions/1/children

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是||


|permissionModels|是|可传入 permissionNumber、per 等字段|
| - | - | :- |

missionName、permissionType、status、applicationId、permissionLevel、url、description

返回说明

正确情况下的返回HTTP头如下：![ref68]

`  `{

`      `"date": "Sat, 04 Feb 2017 07:17:07 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：   no content

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486351905060,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "必须传入id ",

`      `"path": "/v1/permissions/0"

`  `}

9. 新增指定的单个子权限

根据权限主键id新增该权限的一个指定的下级权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.223.png)

`  `http://localhost:8080/v1/permissions/{id}/children/{permissionId}

请求示例：

`  `http://localhost:8080/v1/permissions/1/children/{2}

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|权限主键id|
|permissionId|是|子权限主键id|

返回说明

正确情况下的返回HTTP头如下：![ref56]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 03:07:31 GMT"

`  `}

正确情况下的返回JSON数据包如下：

`    `1

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486364801439,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "权限[人力系统的权限]不是组权限，不能作为上级!",

`      `"path": "/v1/permissions/1/children/2"

`  `}

10. 删除指定的单个子权限

根据权限主键id删除该权限的一个指定的下级权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.224.png)

`  `http://localhost:8080/v1/permissions/{id}/children/{permissionId}

请求示例：

`  `http://localhost:8080/v1/permissions/1/children/{2}

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|父权限主键id|
|permissionId|是|子权限主键id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.225.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 03:07:31 GMT"

`  `}

正确情况下的返回JSON数据包如下：

`    `1

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486364801439,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "权限[人力系统的权限]不是组权限，不能作为上级!",

`      `"path": "/v1/permissions/1/children/2"

`  `}

11. 批量新增父权限

使用本接口根据权限主键id新增该权限的一个或多个父权限，传入参数为json格 式。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.226.png)

`  `http://localhost:8080/v1/permissions/{id}/parents

请求示例：

`  `http://localhost:8080/v1/permissions/1/parents

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|子权限的主键id|
|permissionModel|是|用户信息，接收数据 格式为**json**，可传入 permissionNumber、per 等字段|

missionName、permissionType、status、applicationId、permissionLevel、url、description

参数示例如下： ![ref65]  [

`    `{

`     `"applicationId": 0,     "createOrg": "string",     "createPerson": 0,

`    `"createTime": "2017-02-06T01:13:51.697Z",     "description": "测试权限",

`    `"id": 3,

`    `"permissionLevel": "2",

`    `"permissionName": "权限1",

`    `"permissionNumber": "CS",

`    `"permissionType": "1",

`    `"remark": "",

`    `"status": "",

`    `"updateOrg": "zts",

`    `"updatePerson": 0,

`    `"updateTime": "2017-02-06T01:13:51.697Z",     "url": "string"

`   `}

`  `]

返回说明

正确情况下的返回HTTP头如下：![ref20]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 03:07:31 GMT"

`  `}

正确情况下的返回JSON数据包如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.227.png)

`  `{

`      `"permissionName": "财富管理平台的权限",       "permissionNumber": "CFGLPTDQX",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 3,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": null,

`      `"sourceDescription": null,

`      `"applicationCode": "M0003",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/permissions/3"         }

`      `},

`      `"id": 3

` `}

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486362175971,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "权限[T545-]不是组权限，不能作为上级",

`      `"path": "/v1/permissions/31/children"

`  `}

12. 查询多个父权限

根据权限主键id查询该权限的父权限，支持权限范围条件查询

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref76]

`  `http://localhost:8080/v1/permissions/{id}/parents

请求示例：

`  `http://localhost:8080/v1/permissions/21/parents

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|子权限的主键id|
|super|否|权限范围，supper=true 时，除返回该权限的直 接上级权限，还递归返 回其继承的所有上级权 限|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.229.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 05:32:11 GMT"

`  `}

正确情况下的返回结果如下：

`  `{

`   `"total": 1,

`   `"count": 1,

`   `items:

`    `[

`      `{

`      `"permissionName": "财富管理平台的权限",       "permissionNumber": "CFGLPTDQX",

`      `"permissionType": "1",

`      `"status": "1",

`      `"applicationId": 3,

`      `"permissionLevel": "1",

`      `"url": null,

`      `"source": null,

`      `"sourceDescription": null,

`      `"applicationCode": "M0003",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/permissions/3"

`        `}![ref35]

`      `},

`      `"id": 3       }

`    `]

` `}

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486359229263,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数<ID>不能为空",

`      `"path": "/v1/permissions/0"

`  `}

13. 批量删除父权限

根据权限主键id删除该权限指定的多个父权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref50]

`  `http://localhost:8080/v1/permissions/{id}/parents

请求示例：

`  `http://localhost:8080/v1/permissions/1/parents

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|子权限的主键id|
|permissionModels|是|可传入 permissionNumber、per 等字段|

missionName、permissionType、status、applicationId、permissionLevel、url、description

返回说明

正确情况下的返回HTTP头如下：![ref77]

`  `{![ref75]

`      `"date": "Sat, 04 Feb 2017 07:17:07 GMT",

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：   no content

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486351905060,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "必须传入id ",

`      `"path": "/v1/permissions/0"

`  `}

14. 新增指定的单个父权限

根据权限主键id新增该权限的一个指定的父权限。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref31]

`  `http://localhost:8080/v1/permissions/{id}/parents/{permissionId}

请求示例：

`  `http://localhost:8080/v1/permissions/1/parents/{2}

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|子权限主键id|
|permissionId|是|父权限主键id|

返回说明

正确情况下的返回HTTP头如下：![ref66]

`  `{![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.231.png)

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 03:07:31 GMT"

`  `}

正确情况下的返回JSON数据包如下：

`    `1

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486364801439,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "权限[人力系统的权限]不是组权限，不能作为上级!",

`      `"path": "/v1/permissions/1/children/2"

`  `}

15. 删除指定的单个父权限

根据权限主键id删除该权限的一个指定的父权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref50]

`  `http://localhost:8080/v1/permissions/{id}/parents/{permissionId}

请求示例：

`  `http://localhost:8080/v1/permissions/1/parents/{2}

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|权限主键id|
|permissionId|是|父权限主键id|

返回说明

正确情况下的返回HTTP头如下：![ref77]

`  `{![ref75]

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Mon, 06 Feb 2017 03:07:31 GMT"

`  `}

正确情况下的返回JSON数据包如下：

`    `1

错误情况下的返回HTTP头示例如下：

`  `{

`      `"timestamp": 1486364801439,       "status": 403,

`      `"error": "Forbidden",

`      `"exception": "com.zts.common.spec.exception.ValidateException",       "message": "权限[人力系统的权限]不是组权限，不能作为上级!",

`      `"path": "/v1/permissions/1/children/2"

`  `}

4. 应用**application**
1. 获取应用列表

使用本接口获取应用的分页信息，支持条件过滤。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.232.png)

`  `http://localhost:8080/v1/applications

请求示例：

`  `http://localhost:8080/v1/applications?appName=t

参数说明



|参数|是否必须|说明|
| - | - | - |
|appCode|否|应用代码|
|appName|否|应用名称,支持模糊查询|
|description|否|描述,支持模糊查询|



|uniauthType|否|系统验证类型|
| - | - | - |
|redirectUrl|否|重定向地址|
|appSecret|否|应用共享密钥|
|authInfo|否|授权信息|
|status|否|状态|
|unifiedFlag|否|是否同一用户体系|
|id|否||
|createPerson|否|创建人|
|createTime|否|创建时间，查询在此时 间（含）之后的数据|
|updatePerson|否|修改人|
|updateTime|否|修改时间，查询在此时 间（含）之后的数据|
|remark|否||
|pageBound|否||
返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.233.png)

`  `{

`  `"date": "Tue, 07 Feb 2017 01:05:22 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 2,

`  `"count": 2,

`  `"items": [

`    `{

`      `"appCode": "T545",

`      `"appName": "test0118002",

`      `"description": null,

`      `"uniauthType": "2",

`      `"redirectUrl": null,

`      `"appSecret": null,![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.234.png)

`      `"status": "0",

`      `"authInfo": null,

`      `"unifiedFlag": "0",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/applications/26"         }

`      `},

`      `"id": 26

`    `},

`    `{

`      `"appCode": "T12345",

`      `"appName": "test0118",

`      `"description": null,

`      `"uniauthType": "1",

`      `"redirectUrl": null,

`      `"appSecret": null,

`      `"status": "0",

`      `"authInfo": null,

`      `"unifiedFlag": "0",

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/applications/25"         }

`      `},

`      `"id": 25

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,   "items": []   }

2. 新增应用

使用本接口新增application信息，传入参数为json格式。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.235.png)

`  `http://localhost:8080/v1/applications![ref23]

请求示例：

`  `http://localhost:8080/v1/applications

参数说明



|参数|是否必须|说明|
| - | - | - |
|applicationModel|是|应用信息，接 收数据格式为 **json**，appCode、appNa 字段|

me、appSecret、authInfo、redirectUrl、remark、uniauthType、unifiedFlag、createPerson、createTime、updatePerson、updateTime

参数示例如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.236.png)

{

` `"appCode": "T555",  "appName": "t555",  "appSecret": "",

` `"authInfo": "test",  "redirectUrl": "",  "remark": "string",  "status": "1",

` `"uniauthType": "1" }

返回说明

正确情况下的返回HTTP头如下：![ref53]

`  `{

`  `"date": "Tue, 07 Feb 2017 01:11:52 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"appCode": "T555",

`  `"appName": "t555",

`  `"description": null,

`  `"uniauthType": "1",

`  `"redirectUrl": null,

`  `"appSecret": null,![ref5]

`  `"status": "1",

`  `"authInfo": "test",

`  `"unifiedFlag": "0",

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/applications/28"     }

`  `},

`  `"id": 28

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486429996890,

`  `"status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "关键字【系统编码（appCode）和系统名称（appName）】已被使用，请检查",

`  `"path": "/v1/applications"

`  `}

3. 删除应用

使用本接口根据主键id删除某一应用。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref54]

`  `http://localhost:8080/v1/applications/{id}

请求示例：

`  `http://localhost:8080/v1/applications/26

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|

返回说明

正确情况下的返回HTTP头如下：![ref55]

`  `{![ref18]

`  `"date": "Tue, 07 Feb 2017 01:25:38 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `no content

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486430738075,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "应用不存在！",

`  `"path": "/v1/applications/33"

`  `}

4. 查询应用

使用本接口根据主键id查询某一应用的信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref36]

`  `http://localhost:8080/v1/applications/{id}

请求示例：

`  `http://localhost:8080/v1/applications/26

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.237.png)

`  `{

`  `"date": "Tue, 07 Feb 2017 01:27:46 GMT",

`  `"server": "Apache-Coyote/1.1",![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.238.png)

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"

`  `}

正确情况下的返回JSON数据包如下：

`  `{

`  `"appCode": "T545",

`  `"appName": "test0118002",

`  `"description": null,

`  `"uniauthType": "2",

`  `"redirectUrl": null,

`  `"appSecret": null,

`  `"status": "0",

`  `"authInfo": null,

`  `"unifiedFlag": "0",

`  `"\_links": {

`    `"self": {

`      `"href": "http://localhost:8080/v1/applications/26"     }

`  `},

`  `"id": 26

}

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486430905230,

`  `"status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[99]对应的应用不存在！",

`  `"path": "/v1/applications/99"

`  `}

5. 修改应用

使用本接口根据主键id修改某一应用的信息。

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.239.png)

`  `http://localhost:8080/v1/applications/{id}

请求示例：

`  `http://localhost:8080/v1/applications/26

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|
|applicationModel|是|要修改的应用的信息项 及修改内容，数据传入 格式为**json**，可修改 appCode、appName、a|

ppSecret、authInfo、description、redirectUrl、remark、status、unifiedFlag。

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.240.png)

`  `{

`  `"date": "Tue, 07 Feb 2017 01:33:47 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回结果如下：

`  `1

错误情况下的返回JSON数据包示例如下：   {

`  `"timestamp": 1486431189966,

`  `"status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "校验失败：应用状态非正常启用！",

`  `"path": "/v1/applications/26"

`  `}

6. 查询拥有某应用系统准入权限的机构(根据**appCode)**

使用本接口根据应用的代码code查询该应用系统的所有机构，也可进行查询范围 进行条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref78]

`  `http://localhost:8080/v1/applications/orgs![ref48]

请求示例：

`  `http://localhost:8080/v1/applications/orgs?appCode=M0003&supper=false

参数说明



|参数|是否必须|说明|
| - | - | - |
|appCode|是|应用系统的代码|
|supper|否|boolean型，查询范围,当 前应用系统对应某一权 限，supper=true时除返 回该应用系统下的直接 机构，也可递归返回该 应用系统作为上级时的 所有下级的机构|

返回说明

正确情况下的返回HTTP头如下：![ref79]

`  `{

`  `"date": "Tue, 07 Feb 2017 01:17:45 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,   "count": 1,   "items": [     {

`      `"parents": "0001:25",

`      `"groupNumber": "0003",       "groupName": "A总部/B分部",

`      `"groupType": "1",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,

`      `"datasource": null,

`      `"\_links": {

`        `"self": {![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.243.png)

`          `"href": "http://localhost:8080/v1/applications/3"         }

`      `},

`      `"id": 3

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,   "items": []   }

7. 查询拥有某应用系统准入权限的机构(根据主键**id)**

使用本接口根据应用的主键id查询该应用系统的所有机构，也可进行查询范围进 行条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref76]

`  `http://localhost:8080/v1/applications/{id}/orgs

请求示例：

`  `http://localhost:8080/v1/applications/26/orgs?supper=false

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的主键id|
|supper|否|boolean型，查询范围,当 前应用系统对应某一权 限，supper=true时除返 回该应用系统下的直接 机构，也可递归返回该 应用系统作为上级时的 所有下级的机构|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.244.png)

`  `{

`  `"date": "Tue, 07 Feb 2017 01:36:11 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,   "count": 1,   "items": [     {

`      `"parents": "0001:25",

`      `"groupNumber": "0003",       "groupName": "A总部/B分部",

`      `"groupType": "1",

`      `"subType": null,

`      `"status": "1",

`      `"description": null,

`      `"datasource": null,

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/applications/3"         }

`      `},

`      `"id": 3

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,   "items": []   }

8. 查询拥有某应用系统准入权限的用户(根据**appCode)**

使用本接口根据应用的代码code查询该应用系统的所有用户，也可进行查询范围 进行条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref80]

`  `http://localhost:8080/v1/applications/users

请求示例：

`  `http://localhost:8080/v1/applications/users? appCode=M0003&status=1&supper=false

参数说明



|参数|是否必须|说明|
| - | - | - |
|appCode|是|应用系统的代码|
|status|否|人员的状态|
|supper|否|boolean型，查询范围,当 前应用系统对应某一权 限，supper=true时除返 回该应用系统下的直接 用户，也可递归返回该 应用系统作为上级时的 所有下级的用户|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.246.png)

`  `{

`  `"date": "Tue, 07 Feb 2017 01:22:20 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"userName": "tomy",

`      `"name": "tomy",

`      `"email": "tomy@163.cn",

`      `"phoneNo": "128745545",![ref6]

`      `"officePhone": null,

`      `"gender": null,

`      `"status": "1",

`      `"chgPassFlag": "1",

`      `"orgs": "",

`      `"employeeNumber": "5512685",

`      `"userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.247.png)

`            `"groupNumber": "0155",             "jobCode": "300",

`            `"jobName": "机房管理岗",

`            `"jobGroupCode": null,

`            `"jobGroupName": null

`         `}

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 12585,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 5512685

`        `},

`        `{

`            `"id": 2,

`            `"userId": 12585,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 5512685

`        `}

`    `],

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/applications/12585"         }

`      `},

`      `"id": 12585

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,

`  `"items": []   }

9. 查询拥有某应用系统准入权限的用户(根据主键**id)**

使用本接口根据应用的主键id查询该应用系统的所有用户，也可进行查询范围进 行条件查询。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.248.png)

`  `http://localhost:8080/v1/applications/{id}/users

请求示例：

`  `http://localhost:8080/v1/applications/26/users?status=1&supper=false

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的主键id|
|status|否|人员的状态|
|supper|否|boolean型，查询范围,当 前应用系统对应某一权 限，supper=true时除返 回该应用系统下的直接 用户，也可递归返回该 应用系统作为上级时的 所有下级的用户|

返回说明

正确情况下的返回HTTP头如下：![ref16]

`  `{

`  `"date": "Tue, 07 Feb 2017 01:38:39 GMT",

`  `"server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",

`  `"x-application-context": "application",

`  `"content-type": "application/json;charset=UTF-8"   }

正确情况下的返回JSON数据包如下：

`  `{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"userName": "tomy",

`      `"name": "tomy",

`      `"email": "tomy@163.cn",

`      `"phoneNo": "128745545",

`      `"officePhone": null,![ref11]

`      `"gender": null,

`      `"status": "1",

`      `"chgPassFlag": "1",

`      `"orgs": "",

`      `"employeeNumber": "5512685",

`      `"userMaps":       [

`                  `{

`            `"id": 395,

`            `"userId": 395,

`            `"convertId": "7788string",

`            `"appCode": "M0001",

`            `"status": "1",

`            `"employeeNumber": "7788string"          },

`                  `{

`            `"id": 36684,

`            `"userId": 395,

`            `"convertId": "liss",

`            `"appCode": "M0002",

`            `"status": "1",

`            `"employeeNumber": "7788string"          }

`      `],

`      `"type": 1,

`      `"jobCode": "J10698",

`      `"jobName": "经理",

`      `"jobStatus": "1",

`      `"jobGroupCode": "15",

`      `"jobGroupName": "运营类-信息技术",       "empStatus": "1",

`      `"workPlace": "山东济南",

`      `"phoneShortNo": null,

`      `"officeShortNo": null,

`      `"datasource": "HR",

`      `"partOrgs":       [

`                  `{

`            `"id": 15,

`            `"groupNumber": "0156",             "jobCode": "301",

`            `"jobName": "分支机构管理岗",             "jobGroupCode": null,

`            `"jobGroupName": null

`         `},

`                  `{

`            `"id": 36,

`            `"groupNumber": "0155",             "jobCode": "300",![ref37]

`            `"jobName": "机房管理岗",

`            `"jobGroupCode": null,

`            `"jobGroupName": null

`         `}

`      `],

`      `"properties": [

`        `{

`            `"id": 3,

`            `"userId": 12585,

`            `"userKey": "key2",

`            `"userValue": "val2",

`            `"employeeNumber": 5512685

`        `},

`        `{

`            `"id": 2,

`            `"userId": 12585,

`            `"userKey": "key1",

`            `"userValue": "val1",

`            `"employeeNumber": 5512685

`        `}

`    `],

`      `"\_links": {

`        `"self": {

`          `"href": "http://localhost:8080/v1/applications/12585"         }

`      `},

`      `"id": 12585

`    `}

`  `]

}

查询结果为空的情况下的返回JSON数据包示例如下:

`  `{

`  `"total": 0,   "count": 0,   "items":

`  `[]

`  `}

10. 删除应用的接口访问权限

使用本接口根据应用的主键id和接口的id列表，删除对应接口的访问权限。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.249.png)

`  `http://localhost:8080/v1/applications/{id}/interface

请求示例：

`  `http://localhost:8080/v1/applications/26/interface

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|
|interfaceIds|是|接口的主键id列表，例如 [1,2]|

返回说明

正确情况下返回状态：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.250.png)

200 正确情况下返回内容：

0 异常情况下返回：

{

`  `"timestamp": 1516349874208,   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "接口不存在",

`  `"path": "/v1/applications/40/interface"

}

11. 查询应用可访问的接口

使用本接口应用主键id查询应用可访问的接口信息。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.251.png)

`  `http://localhost:8080/v1/applications/{id}/interface

请求示例：

`  `http://localhost:8080/v1/applications/26/interface![ref9]

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|

返回说明

正确情况下返回HTTP头： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.252.png){

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Fri, 19 Jan 2018 07:32:04 GMT",

`  `"": ""

} 正确情况下返回状态：

200 正确情况下返回内容：

{

`  `"total": 3,

`  `"count": 3,

`  `"items": [

`    `{

`      `"category": "users",

`      `"method": "GET",

`      `"url": "/v1/users/newtest",

`      `"orderNum": 1,

`      `"status": null,

`      `"name": "新增test接口",       "\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/applications/135"         }

`      `},

`      `"id": 135

`    `},

`    `{

`      `"category": "users",

`      `"method": "GET",

`      `"url": "/v1/users/checkexist/?",

`      `"orderNum": 14,![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.253.png)

`      `"status": null,

`      `"name": "验证用户是否存在",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/applications/14"         }

`      `},

`      `"id": 14

`    `},

`    `{

`      `"category": "users",

`      `"method": "GET",

`      `"url": "/v1/users/\\d{1,}/groups/\\d{1,}/?",

`      `"orderNum": 25,

`      `"status": null,

`      `"name": "查询用户某个所属组(根据主键id)",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/applications/25"         }

`      `},

`      `"id": 25

`    `}

`  `]

} 异常情况下返回内容：

{

`  `"timestamp": 1516346953413,   "status": 404,

`  `"error": "Not Found",

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[134]对应的应用不存在！",

`  `"path": "/v1/applications/134/interface"

}

12. 为应用增加可访问的接口

使用本接口根据应用主键id和接口id列表为应用增加可访问的接口信息。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.254.png)

`  `http://localhost:8080/v1/applications/{id}/interface

请求示例：![ref17]

`  `http://localhost:8080/v1/applications/26/interface

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|
|interfaceIds|是|接口的主键id列表，例如 [1,2]|

返回说明

正确情况下返回状态：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.255.png)

201

正确情况下返回内容： 2(\*返回添加成功的接口数\*) 异常情况下返回：

401

Unauthorized

403

Forbidden

404

Not Found

13. 查询应用无权访问的接口

使用本接口根据应用的主键id查询无权访问的接口。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.256.png)

`  `http://localhost:8080/v1/applications/{id}/interface/nopermission

请求示例：

`  `http://localhost:8080/v1/applications/1/interface/nopermission

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|应用的id|

返回说明

正确情况下返回HTTP头： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.257.png){

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Fri, 19 Jan 2018 07:46:18 GMT",

`  `"": ""

} 正确情况下返回状态：

200 正确情况下返回内容：

{

`  `"total": 2,

`  `"count": 2,

`  `"items": [

`    `{

`      `"category": "applications",       "method": "GET",

`      `"url": "/v1/applications/vo",

`      `"orderNum": 1,       "status": "0",

`      `"name": "获取时间",       "\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/applications/133"         }

`      `},

`      `"id": 133

`    `},

`    `{

`      `"category": null,

`      `"method": "GET",

`      `"url": "111",

`      `"orderNum": 129,

`      `"status": "0",

`      `"name": "zhang-test",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/applications/129"         }

`      `},

`      `"id": 129

`    `}![ref30]

`  `]

} 异常情况下返回：

401 Unauthorized 403 Forbidden 404

Not Found

5. 字典**catalogs**
1. 查询字典列表

使用本接口可以获取字典列表，支持条件过滤。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.258.png)

`  `http://localhost:8080/v1/catalogs

请求示例：

`  `http://localhost:8080/v1/catalogs

参数说明



|参数|是否必须|说明|
| - | - | - |
|cataCode|否|字典类代码|
|cataName|否|字典类名称|
|status|否|字典类状态(1:启用，0: 未启用)|
|id|否|字典类id|

返回说明

正确情况下的返回HTTP头如下：![ref64]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",![ref7]

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回JSON数据包如下：     {

`      `"total": 2,       "count": 2,       "items":

`      `[

`        `{

`          `"cataCode": "STATUS",           "cataName": "状态",

`          `"status": "1",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/catalogs/1"             }

`          `},

`          `"id": 1

`        `},

`        `{

`          `"cataCode": "PERMISSION\_TYPE",           "cataName": "权限类型",

`          `"status": "1",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/catalogs/2"             }

`          `},

`          `"id": 2

`        `}

`      `]

`    `}

错误情况下的返回HTTP头示例如下：

2. 新增单个字典类

使用本接口可以增加单个字典类

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.259.png)

`  `http://localhost:8080/v1/catalogs

请求示例：

`  `http://localhost:8080/v1/catalogs

参数说明



|参数|是否必须|说明|
| - | - | - |
|catalogModel|是|<p>字典类信息，数据传 入格式为json。格式 示例：{ "cataCode":</p><p>"string", "cataName": "string", "createOrg": "string", "createPerson": 0, "createTime": "2017-02-07T00:44:55.85 "id": 0, "remark":</p><p>"string", "status":</p><p>"string", "updateOrg": "string", "updatePerson": 0, "updateTime": "2017-02-07T00:44:55.85</p>|

1Z",

1Z" }

返回说明

正确情况下的返回HTTP头如下：![ref59]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 01:15:36 GMT"

`  `}

正确情况下的返回JSON数据包如下：     {

`      `"cataCode": "test0207",

`      `"cataName": "test0207",![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.260.png)

`      `"status": "1",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/catalogs/10"         }

`      `},

`      `"id": 10

`    `}

错误情况下的返回HTTP头示例如下：     {

`      `"timestamp": 1486430021402,

`      `"status": 500,

`      `"error": "Internal Server Error",

`      `"exception":

` `"org.springframework.dao.DataIntegrityViolationException",

`      `"message": "\r\n### Error updating database.  Cause:

` `org.postgresql.util.PSQLException: ERROR: value too long

` `for type character varying(1)\r\n### The error may involve

` `com.zts.user.data.mappers.CatalogMapper.insert-Inline\r\n### The error  occurred while setting parameters\r\n### SQL: INSERT INTO t\_catalog

`  `(cata\_code, cata\_name, status, remark, create\_time, create\_person,

` `update\_time, update\_person) VALUES (?, ?, ?, ?, ?, ?, ?, ?)\r

\n### Cause: org.postgresql.util.PSQLException: ERROR: value

` `too long for type character varying(1)\n; SQL []; ERROR: value

` `too long for type character varying(1); nested exception is

` `org.postgresql.util.PSQLException: ERROR: value too long for type

` `character varying(1)",

`      `"path": "/v1/catalogs"

`    `}

3. 查询单个字典类

根据字典类的id查询某个字典类

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref76]

`  `http://localhost:8080/v1/catalogs/{id}

请求示例：

`  `http://localhost:8080/v1/catalogs/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|字典类的id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.261.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回JSON数据包如下：     {

`      `"cataCode": "STATUS",       "cataName": "状态",

`      `"status": "1",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.28.50:8080/v1/catalogs/1"         }

`      `},

`      `"id": 1

`    `}

错误情况下的返回HTTP头示例如下：     {

`      `"timestamp": 1486432203927,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数id为空",

`      `"path": "/v1/catalogs/0"

`    `}

4. 修改单个字典类

根据字典类的id修改某个字典类

接口调用请求说明

`  `http请求方式：PUT,HTTP调用![ref43]

`  `http://localhost:8080/v1/catalogs/{id}

请求示例：

`  `http://localhost:8080/v1/catalogs/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|字典项的id|
|catalogModel|是|要修改的字典信息项及 修改值，数据传入格式 为json|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.262.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回结果如下：     1

错误情况下的返回HTTP头示例如下：     {

`      `"timestamp": 1486432203927,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数id为空",

`      `"path": "/v1/catalogs/0"

`    `}

5. 删除单个字典类

根据字典类的id删除该字典类

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref8]

`  `http://localhost:8080/v1/catalogs/{id}

请求示例：

`  `http://localhost:8080/v1/catalogs/1

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|字典类的id|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.263.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回结果如下：     1

错误情况下的返回HTTP头示例如下：     {

`      `"timestamp": 1486432203927,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数id为空",

`      `"path": "/v1/catalogs/0"

`    `}

6. 查询字典类的属性(根据**cataCode)**

根据字典类的代码cataCode查询某个字典类的全部属性

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.264.png)

`  `http://localhost:8080/v1/catalogs/properties/{cataCode}

请求示例：

`  `http://localhost:8080/v1/catalogs?cataCode=test

参数说明



|参数|是否必须|说明|
| - | - | - |
|cataCode|是|字典类的代码|

返回说明

正确情况下的返回HTTP头如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.265.png)

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回JSON数据包如下：     {

`      `"\_embedded": {},

`      `"\_links": [

`        `{

`          `"href": "string",           "rel": "string",           "templated": true         }

`      `],

`      `"count": 0,

`      `"items": [

`        `{

`          `"\_embedded": {},           "\_links": [

`            `{![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.266.png)

`              `"href": "string",               "rel": "string",

`              `"templated": true             }

`          `],

`          `"cataCode": 0,

`          `"cataId": 0,

`          `"detailCode": "string",           "detailName": "string",           "id": 0,

`          `"status": "string"

`        `}

`      `],

`      `"total": 0

`    `}

错误情况下的返回HTTP头示例如下：

7. 查询字典类的属性(根据**id)**

根据字典类的主键id查询某个字典类的全部属性

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref54]

`  `http://localhost:8080/v1/catalogs/{id}/properties

请求示例：

`  `http://localhost:8080/v1/catalogs/9/properties

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|字典类的id|

返回说明

正确情况下的返回HTTP头如下：![ref71]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",

`      `"transfer-encoding": "chunked",![ref11]

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"   }

正确情况下的返回JSON数据包如下：     {

`      `"total": 2,       "count": 2,       "items": [         {

`          `"cataId": 9,

`          `"cataCode": null,           "detailCode": "1",

`          `"detailName": "员工",           "status": "1",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/catalogs/65"             }

`          `},

`          `"id": 65

`        `},

`        `{

`          `"cataId": 9,

`          `"cataCode": null,

`          `"detailCode": "2",

`          `"detailName": "管理员",           "status": "1",

`          `"\_links": {

`            `"self": {

`              `"href": "http://10.29.28.50:8080/v1/catalogs/66"             }

`          `},

`          `"id": 66

`        `}

`      `]

`    `}

错误情况下的返回HTTP头示例如下：      {

`      `"timestamp": 1486434831002,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数<字典id>为空",

`      `"path": "/v1/catalogs/0/properties"     }![ref17]

8. 新增或修改字典类的属性

根据字典类的主键id新增或修改某个字典类的属性

接口调用请求说明

`  `http请求方式：POST、PUT,HTTP调用![ref10]

`  `http://localhost:8080/v1/catalogs/{id}/properties

请求示例：

`  `http://localhost:8080/v1/catalogs/9/properties

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|字典类的id|
|catalogProperties|是|<p>字典条目的json对象， 示例：[ { "cataCode":</p><p>"test2", "cataId": 9, "detailCode": "test2-1", "detailName": "test2-1", "id": 901, "status": "1" } ]</p>|

返回说明

正确情况下的返回HTTP头如下：![ref62]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回结果如下：     1

错误情况下的返回HTTP头示例如下：

`    `{![ref39]

`      `"timestamp": 1486435198162,       "status": 404,

`      `"error": "Not Found",

`      `"exception":

` `"com.zts.common.spec.exception.SourceNotFoundException",       "message": "对象不存在！",

`      `"path": "/v1/catalogs/9/properties"

`    `}

9. 删除字典类的属性

根据字典类的主键id删除某个字典类的全部属性或由detailCode指定的单个属性

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![ref50]

`  `http://localhost:8080/v1/catalogs/{id}/properties

请求示例：

`  `http://localhost:8080/v1/catalogs/9/properties

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|字典类的id|
|detailCode|否|某个属性的代码|

返回说明

正确情况下的返回HTTP头如下：![ref26]

`  `{

`      `"server": "Apache-Coyote/1.1",

`      `"x-application-context": "application",

`      `"content-type": "application/json;charset=UTF-8",       "transfer-encoding": "chunked",

`      `"date": "Tue, 07 Feb 2017 00:47:23 GMT"

`  `}

正确情况下的返回结果如下：    1

错误情况下的返回HTTP头示例如下： ![ref30]     {

`      `"timestamp": 1486434831002,       "status": 400,

`      `"error": "Bad Request",

`      `"exception":

` `"com.zts.common.spec.exception.InvalidArgumentException",       "message": "参数<字典id>为空",

`      `"path": "/v1/catalogs/0/properties"

`    `}

6. 代办中心**taskcenter**
1. 附件同步

使用本接口可以同步待办流程中附件的PDF预览文件。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref81]

`  `http://localhost:8080/v1/taskcenter/attachments/sync

请求示例：

`  `http://localhost:8080/v1/taskcenter/attachments/sync? accessToken=aaaa&fileId=aaaa

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证的token|
|fileId|是|需要同步的表单Id|

返回说明

{![ref81]

`  `"resultCode": 0,

`  `"resultMsg": "同步完成",   "\_returncode\_": 0,

`  `"\_returnmsg\_": null }

2. 同步流程组织架构

使用本接口可以同步蜂巢的组织架构数据，包括上下级关系。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref80]

`  `http://localhost:8080/v1/taskcenter/orgs/dept

请求示例：

`  `http://localhost:8080/v1/taskcenter/orgs/dept?accessToken=x

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证的token|
|deptId|否|部门id（暂未启用）|

返回说明

正确情况下的返回JSON数据包如下：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.268.png)

` `{

`    `"deptId":"591","deptName":"A公司",

`    `"depts":[

{"deptId":"22751","deptName":"办公室A","depts":[],

`         `"users":[ {"userId":"14414","userName":"张三","userAccount":"zhangsan"},

{"userId":"13863","userName":"李四","userAccount":"lisi"}

`         `]

`        `},

{"deptId":"22752","deptName":"办公室B","depts":[],

`         `"users":[ {"userId":"14414","userName":"王三","userAccount":"wangsan"},

{"userId":"13863","userName":"张一","userAccount":"zhangyi"}

`         `]

`        `}     ]

`  `}

3. 查询待办任务

使用本接口可以查询系统中的某人的待办任务。

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.269.png)

`  `http://localhost:8080/v1/taskcenter/tasks

请求示例：

`  `http://localhost:8080/v1/taskcenter/tasks? accessToken=aaaa&userAccount=tianyj&sysCode=M0002

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证的token|
|userAccount|是|蜂巢用户名|
|sysCode|是|系统代码|

返回说明

正确情况下的返回XML格式数据如下：![ref16]

<UserTask xmlns="">

`  `<userAccount>tianyj</userAccount>   <status/>

`    `<sysCode>M0002</sysCode>

`    `<sysName>OA系统</sysName>

`    `<shorder>2</shorder>

`    `<taskNum>1</taskNum>

`    `<taskList>

`      `<taskId/>

`      `<fileId>0</fileId>

`      `<objectClass/>

`      `<userId/>

`      `<userAccount>田延杰</userAccount>

`      `<flowTitle>通用流程</flowTitle>

`      `<taskTitle>集成化信息管理平台技术培训</taskTitle>

`      `<startTime>2016-04-27 13:40:20</startTime>       <taskPriority/>![ref12]

`      `<drawUserName/>

`      `<drawDate/>

`      `<drawDeptName/>

`      `<fileNo/>

`      `<curNodeName/>

`      `<preUserName/>

`      `<userName/>

`      `<todoUrl>http://oa.zts.com.cn/qlzq/ persontasks.nsf/18AB7B6FC04E52AD48256EA80030500B/ E327074CC879597D48257FA2001F28EF?

OpenDocument&view=vwgwdbshow&RestrictToCategory=田延杰/qlzq&Portal=1</ todoUrl>

`    `</taskList>

</UserTask>

4. 查询系统中已完成的流程

使用本接口可以查询系统中的流程 ===== 接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.270.png)

`  `http://localhost:8080/v1/taskcenter/done/flow

请求示例：

`  `http://localhost:8080/v1/taskcenter/done/flow? accessToken=aaaa&userAccount=tianyj&sysCode=M0002&fileId=1&taskId=1&instanceId=1

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证Token|
|userAccount|是|蜂巢用户名|
|sysCode|是|系统代码|
|fileId|是|流程编号|
|taskId|是|任务编号|
|instanceId|是|任务编号|

返回说明

<Flow xmlns="">![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.271.png)

`  `<fileId>181049</fileId>

`  `<flowTitle>信用账户佣金申请</flowTitle>

`  `<taskTitle>ssssssssss的信用账户佣金申请</taskTitle>

`  `<form>

`    `<formGroup>

`      `<groupName>基本信息</groupName>

`      `<formData>

`        `<title>标题</title>

`        `<text>ssssssssss的信用账户佣金申请</text>       </formData>

`      `<formData>

`        `<title>申请时间</title>

`        `<text>2017-05-23</text>

`      `</formData>

`      `<formData>

`        `<title>申请人</title>         <text></text>

`      `</formData>

`      `<formData>

`        `<title>客户姓名</title>

`        `<text>sssss</text>

`      `</formData>

`      `<formData>

`        `<title>客户代码</title>

`        `<text>sssss</text>

`      `</formData>

`      `<formData>

`        `<title>信用资金账号</title>         <text>12312312</text>

`      `</formData>

`      `<formData>

`        `<title>拟申请信用账户“信用交易”佣金（‰ ）</title>         <text>123123</text>

`      `</formData>

`      `<formData>

`        `<title>申请理由</title>

`        `<text>11111111</text>

`      `</formData>

`      `<formData>

`        `<title>营业部承诺</title>

`        `<text></text>

`      `</formData>

`      `<formData>![ref11]

`        `<title>营业部名称</title>

`        `<text>信息技术总部/运维中心/运维管理部</text>

`      `</formData>

`      `<formData>

`        `<title>拟申请信用账户“普通交易”佣金（‰ ）</title>         <text>123</text>

`      `</formData>

`      `<formData>

`        `<title>是否曾申请特殊手续费设置</title>

`        `<text></text>

`      `</formData>

`      `<formData>

`        `<title>申请人联系方式</title>

`        `<text></text>

`      `</formData>

`      `<formData>

`        `<title>经办人</title>

`        `<text>董淑光</text>

`      `</formData>

`    `</formGroup>

`  `</form>

`  `<activities>

`    `<activity>

`      `<nodeName>起草</nodeName>

`      `<startTime>2017-05-23 13:59:16</startTime>

`      `<endTime>2017-05-23 13:59:33</endTime>

`      `<deptName>信息技术总部/运维中心/运维管理部</deptName>       <userName>董淑光</userName>

`    `</activity>

`    `<activity>

`      `<nodeName>营业部总经理</nodeName>

`      `<startTime>2017-05-23 13:59:33</startTime>

`      `<endTime>2017-05-23 15:37:40</endTime>

`      `<deptName>信息技术总部/运维中心/运维管理部</deptName>       <userName>董淑光</userName>

`    `</activity>

`    `<activity>

`      `<nodeName>信用业务部审批人</nodeName>

`      `<startTime>2017-05-23 15:37:40</startTime>

`      `<endTime>2017-05-23 15:55:53</endTime>       <deptName>信用业务部/业务运行组</deptName>

`      `<userName>田广健</userName>

`    `</activity>

`    `<activity>

`      `<nodeName>返回申请人</nodeName>

`      `<startTime>2017-05-23 15:55:53</startTime>![ref67]

`      `<endTime>2017-05-23 15:57:10</endTime>

`      `<deptName>信息技术总部/运维中心/运维管理部</deptName>

`      `<userName>董淑光</userName>

`    `</activity>

`  `</activities>

`  `<opinions>

`    `<opinion>

`      `<nodeName>起草</nodeName>

`      `<providerName>信息技术总部/运维中心/运维管理部／董淑光</providerName>

`      `<submitTime>2017-05-23 13:59:16</submitTime>

`      `<opinionContent></opinionContent>

`    `</opinion>

`    `<opinion>

`      `<nodeName>营业部总经理</nodeName>

`      `<providerName>信息技术总部/运维中心/运维管理部／董淑光</providerName>

`      `<submitTime>2017-05-23 15:37:40</submitTime>

`      `<opinionContent>同意</opinionContent>

`    `</opinion>

`    `<opinion>

`      `<nodeName>信用业务部审批人</nodeName>

`      `<providerName>信用业务部/业务运行组／田广健</providerName>

`      `<submitTime>2017-05-23 15:55:53</submitTime>

`      `<opinionContent>同意</opinionContent>

`    `</opinion>

`    `<opinion>

`      `<nodeName>返回申请人</nodeName>

`      `<providerName>信息技术总部/运维中心/运维管理部／董淑光</providerName>

`      `<submitTime>2017-05-23 15:57:10</submitTime>       <opinionContent>同意</opinionContent>

`    `</opinion>

`  `</opinions>

`  `<attachments></attachments>

`  `<relatedFlows></relatedFlows>

</Flow>

5. 提交审批待办任务

使用本接口可以将某项待办审批任务提交。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref63]

`  `http://localhost:8080/v1/taskcenter/task/submit

请求示例：![ref23]

`  `http://localhost:8080/v1/taskcenter/task/submit?accessToken=aaaa

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证的token|
|submitTaskModel|是|<p>任务信息,包括</p><p>actionId(string,optional): 操作ID,fileId (string,</p><p>optional):流程</p><p>FILEID,nextDeptUserIds(</p><p>optional): 已选部门用 户IDs,nextNodeIds</p><p>(Array[string], optional): 已选节点</p><p>IDs,opinionContent (string, optional): 意</p><p>见内容,paramData</p><p>(Array[ParamData],</p><p>optional): 参数数 据,sysCode (string, optional): 系统编 码,taskId (string, optional):任务</p><p>ID,userAccount</p><p>(string,optional): 用 户登录名,userAgent (string, optional):用户</p><p>Agent,userId (string, optional): 用户ID</p>|

Array[NodeDeptUser],

不同的系统，submitTaskModel参数所必须的属性不尽相同，详细说明如下：



|属性|投入产出系 统、产品中|人力系统|财务系统|门户|
| - | - | - | - | - |



||心系统、结构 CRM系统、 投行系统||||
| :- | :- | :- | :- | :- |
|actionId|是|是|否|是|
|fileId|是|是|否|是|
|userId|否|否|否|是|
|opinionConten|t是|是|否|是|
|paramData|否|否|是|是|
|sysCode|是|是|是|是|
|taskId|否|是|否|否|
|userAccount|是|是|是|是|
|userAgent|否|否|否|是|
|nextNodeIds|否|否|否|是|
|nextDeptUserI|ds否|否|否|是|

submitTaskModel参数格式示例如下： ![ref79]{

`  `"actionId": "string",

`  `"fileId": "string",

`  `"nextDeptUserIds": [

`    `{

`      `"deptId": "string",

`      `"nodeId": "string",

`      `"userId": "string"

`    `}

`  `],

`  `"nextNodeIds": [

`    `"string"

`  `],

`  `"opinionContent": "string",   "paramData": [

`    `{

`      `"pkey": "string",

`      `"pvalue": "string"

`    `}

`  `],

`  `"sysCode": "string",

`  `"taskId": "string",

`  `"userAccount": "string",   "userAgent": "string",   "userId": "string"![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.272.png)

}

submitTaskModel参数因系统差异性而具有不同的格式：



|系统|数据格式|备注|
| - | - | - |
|财务系统|<p>{"sysCode":"M0008","use [{"pkey":"","pvalue":"0a66</p><p>fe58987cf52a,0,不同 意"}]}</p>|rAccount":"yuanxc","para 909f-10ce-4930-840c-|
|人力系统|{"sysCode":"M0001","use 意1019"}|rAccount":"jiaohj","fileId":2|
|LiveBos投入产出系统|<p>{"sysCode":"M0011","use 请同意</p><p>1019","actionId":"1"}</p>|rAccount":"jiangyl","fileId":|

mData":

018,"taskId":"112302","actionId":"accept","opinionContent":"同 112437,"opinionContent":"申

返回说明

接口定义了returncode不同的返回值，对应不同的处理情况。 ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.273.png)resultCode=-1时，返回结果如下：

<Response xmlns="">

`  `<resultCode>-1</resultCode>

`  `<resultMsg>该任务可能已经被办理，请刷新列表后重试！</resultMsg>

`  `<\_returncode\_>0</\_returncode\_>   <\_returnmsg\_/>

</Response> resultCode=0时，返回结果如下：

<Response xmlns="">

`  `<resultCode>0</resultCode>

`  `<resultMsg>操作成功</resultMsg>

`  `<\_returncode\_>0</\_returncode\_>   <\_returnmsg\_/>

</Response>

6. 查询系统中的流程

使用本接口可以查询系统中的流程

接口调用请求说明

`  `http请求方式：GET,HTTP调用![ref81]

`  `http://localhost:8080/v1/taskcenter/flow

请求示例：

`  `http://localhost:8080/v1/taskcenter/flow? accessToken=aaaa&userAccount=dongsg&sysCode=M0021&fileId=181049&taskId=11&instanceId=111

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证Token|
|userAccount|是|蜂巢用户名|
|sysCode|是|系统代码|
|fileId|是|流程编号|
|taskId|是|任务编号|
|instanceId|是|任务编号|

返回说明

<Flow xmlns="">![ref16]

`  `<fileId>181049</fileId>

`  `<flowTitle>信用账户佣金申请</flowTitle>

`  `<taskTitle>ssssssssss的信用账户佣金申请</taskTitle>

`  `<form>

`    `<formGroup>

`      `<groupName>基本信息</groupName>

`      `<formData>

`        `<title>标题</title>

`        `<text>ssssssssss的信用账户佣金申请</text>       </formData>

`      `<formData>

`        `<title>申请时间</title>

`        `<text>2017-05-23</text>

`      `</formData>

`      `<formData>

`        `<title>申请人</title>

`        `<text></text>

`      `</formData>![ref11]

`      `<formData>

`        `<title>客户姓名</title>         <text>sssss</text>

`      `</formData>

`      `<formData>

`        `<title>客户代码</title>         <text>sssss</text>

`      `</formData>

`      `<formData>

`        `<title>信用资金账号</title>         <text>12312312</text>

`      `</formData>

`      `<formData>

`        `<title>拟申请信用账户“信用交易”佣金（‰ ）</title>         <text>123123</text>

`      `</formData>

`      `<formData>

`        `<title>申请理由</title>

`        `<text>11111111</text>

`      `</formData>

`      `<formData>

`        `<title>营业部承诺</title>

`        `<text></text>

`      `</formData>

`      `<formData>

`        `<title>营业部名称</title>

`        `<text>信息技术总部/运维中心/运维管理部</text>

`      `</formData>

`      `<formData>

`        `<title>拟申请信用账户“普通交易”佣金（‰ ）</title>         <text>123</text>

`      `</formData>

`      `<formData>

`        `<title>是否曾申请特殊手续费设置</title>

`        `<text></text>

`      `</formData>

`      `<formData>

`        `<title>申请人联系方式</title>

`        `<text></text>

`      `</formData>

`      `<formData>

`        `<title>经办人</title>         <text>董淑光</text>

`      `</formData>

`    `</formGroup>

`  `</form>![ref6]

`  `<actions>     <action>

`      `<actionId>FS</actionId>

`      `<actionName>发送</actionName>     </action>

`    `<action>

`      `<actionId>TH</actionId>

`      `<actionName>驳回</actionName>

`    `</action>

`  `</actions>

`  `<activities>

`    `<activity>

`      `<nodeName>起草</nodeName>

`      `<startTime>2017-05-23 13:59:16</startTime>

`      `<endTime>2017-05-23 13:59:33</endTime>

`      `<deptName>信息技术总部/运维中心/运维管理部</deptName>       <userName>董淑光</userName>

`    `</activity>

`    `<activity>

`      `<nodeName>营业部总经理</nodeName>

`      `<startTime>2017-05-23 13:59:33</startTime>

`      `<endTime>2017-05-23 15:37:40</endTime>

`      `<deptName>信息技术总部/运维中心/运维管理部</deptName>       <userName>董淑光</userName>

`    `</activity>

`    `<activity>

`      `<nodeName>信用业务部审批人</nodeName>

`      `<startTime>2017-05-23 15:37:40</startTime>

`      `<endTime>2017-05-23 15:55:53</endTime>

`      `<deptName>信用业务部/业务运行组</deptName>

`      `<userName>田广健</userName>

`    `</activity>

`    `<activity>

`      `<nodeName>返回申请人</nodeName>

`      `<startTime>2017-05-23 15:55:53</startTime>

`      `<endTime>2017-05-23 15:57:10</endTime>

`      `<deptName>信息技术总部/运维中心/运维管理部</deptName>

`      `<userName>董淑光</userName>

`    `</activity>

`  `</activities>

`  `<opinions>

`    `<opinion>

`      `<nodeName>起草</nodeName>

`      `<providerName>信息技术总部/运维中心/运维管理部／董淑光</providerName>

`      `<submitTime>2017-05-23 13:59:16</submitTime>

`      `<opinionContent></opinionContent>![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.274.png)

`    `</opinion>

`    `<opinion>

`      `<nodeName>营业部总经理</nodeName>

`      `<providerName>信息技术总部/运维中心/运维管理部／董淑光</providerName>

`      `<submitTime>2017-05-23 15:37:40</submitTime>

`      `<opinionContent>同意</opinionContent>

`    `</opinion>

`    `<opinion>

`      `<nodeName>信用业务部审批人</nodeName>

`      `<providerName>信用业务部/业务运行组／田广健</providerName>

`      `<submitTime>2017-05-23 15:55:53</submitTime>

`      `<opinionContent>同意</opinionContent>

`    `</opinion>

`    `<opinion>

`      `<nodeName>返回申请人</nodeName>

`      `<providerName>信息技术总部/运维中心/运维管理部／董淑光</providerName>

`      `<submitTime>2017-05-23 15:57:10</submitTime>       <opinionContent>同意</opinionContent>

`    `</opinion>

`  `</opinions>

`  `<attachments></attachments>

`  `<relatedFlows></relatedFlows>

</Flow>

7. 查询系统中已完成的代办

使用本接口可以查询系统已经完成的代办任务

接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.275.png)

`  `http://localhost:8080/v1/taskcenter/done/tasks

请求示例：

`  `http://localhost:8080/v1/taskcenter/done/tasks? accessToken=aaaa&userAccount=tianyj&sysCode=M0002

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证Token|



|参数|是否必须|说明|
| - | - | - |
|userAccount|是|蜂巢用户名|
|sysCode|是|系统代码|
|pageBound|是|pageBound|

返回说明

正确情况下的返回：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.276.png)

<UserTask xmlns="">

`  `<userAccount>tianyj</userAccount>

`  `<status/>   <items>

`    `<sysCode>M0021</sysCode>

`    `<sysName>蜂巢门户</sysName>

`    `<shorder>1</shorder>

`    `<taskNum>21</taskNum>

`    `<taskList>

`      `<taskId/>

`      `<fileId>182825</fileId>

`      `<objectClass>HGBB</objectClass>       <userId>13769</userId>

`      `<userAccount>tianyj</userAccount>       <flowTitle>测试8</flowTitle>

`      `<taskTitle/>

`      `<startTime>2017-09-21 17:13:39</startTime>

`      `<taskPriority/>

`      `<drawUserName>田延杰</drawUserName>

`      `<drawDate/>

`      `<drawDeptName>信息技术总部/研发中心/应用研发部</drawDeptName>       <fileNo>风控合规报备[2017]13号</fileNo>

`      `<curNodeName/>

`      `<preUserName/>

`      `<userName>田延杰</userName>

`      `<todoUrl>http://staging.portal.zts.com.cn/zts\_portal/

getURLSyncBPM.do? \_BPM\_FUNCCODE=C\_FormSetFormData&\_mode=4&\_form\_control\_design=LABEL&isdialog=1&\_tf\_file\_id=182825&objectclass=HGBB</ todoUrl>

`    `</taskList>

`  `</items>

</UserTask>

8. 根据**url**获取代办 接口调用请求说明

`  `http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.277.png)

`  `http://localhost:8080/v1/taskcenter/tasks/withurl

请求示例：

`  `http://localhost:8080/v1/taskcenter/tasks/withurl? userAccount=tianyj&accessToken=111

参数说明



|参数|是否必须|说明|
| - | - | - |
|accessToken|是|认证Token|
|userAccount|是|蜂巢用户名|
|sysCode|否|系统代码|

返回说明

正常情况下返回：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.278.png)

<UserTask xmlns="">

`  `<userAccount>tianyj</userAccount>

`  `<status/>   <items>

`    `<sysCode>M0021</sysCode>     <sysName>蜂巢门户</sysName>

`    `<shorder>1</shorder>

`    `<taskNum>8</taskNum>

`    `<taskList>

`      `<taskId>1298710</taskId>

`      `<fileId>182825</fileId>

`      `<objectClass>HGBB</objectClass>

`      `<userId>13769</userId>

`      `<userAccount>tianyj</userAccount>

`      `<flowTitle>合规报备</flowTitle>

`      `<taskTitle>测试8</taskTitle>

`      `<startTime>2017-09-21 17:18:43</startTime>       <taskPriority/>

`      `<drawUserName>田延杰</drawUserName>

`      `<drawDate>20170921</drawDate>

`      `<drawDeptName>信息技术总部/研发中心/应用研发部</drawDeptName>       <fileNo>![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.279.png)风控合规报备[2017]13号</fileNo>

`      `<curNodeName>单位负责人审批</curNodeName>

`      `<preUserName>陈琨</preUserName>

`      `<userName>田延杰</userName>

`      `<instanceId/>

`      `<todoUrl>http://staging.portal.zts.com.cn/zts\_portal/

getURLSyncBPM.do? \_BPM\_FUNCCODE=C\_FormSetFormData&amp;\_bpm\_task\_taskid=1298710&amp;\_tf\_file\_id=182825&amp;objectclass=HGBB&amp;realuserid=13769</ todoUrl>

`    `</taskList>

</UserTask>

7. 消息**sms**

**4.7.1** 发送短信

使用本接口可以向指定的手机号发送短信。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.280.png)

`  `http://localhost:8080/v1/sms

请求示例：

1、application/json

需增加http header，Content-Type:application/json;charset=utf-8 url：http://localhost:8080/v1/sms

json：

`  `{

`  `"message": "测试111",

`  `"phoneNo": "18888888888"

`  `}

2、查询参数 url：http://localhost:8080/v1/sms?message=测试111&phoneNo=18888888888

参数说明



|参数|是否必须|说明|
| - | - | - |
|phoneNo|是|手机号码，仅限一个号 码|



|参数|是否必须|说明|
| - | - | - |
|message|是|要发送的信息|

返回说明

正确情况下的返回状态： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.281.png)  http status：201

正确情况下的返回内容如下：

{

`  `"code": "0",

`  `"msg": "发送成功",   "\_links": {

`    `"self": {

`      `"href": "http://10.29.6.95:8088/v1/sms/0"     }

`  `}

}

失败情况下的返回内容如下：     401 Unauthorized

`    `403

`    `Forbidden     404

`    `Not Found

8. 单点登录**CAS**
1. 票据授权

使用本接口可以申请票据授权。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref13]

`  `http://localhost:8080/cas/v1/ticketGrantingTickets

请求示例：

`  `http://localhost:8080/cas/v1/ticketGrantingTickets

参数说明



|参数|是否必须|说明|
| - | - | - |
|username|是|登录名|
|password|是|密码（32位MD5加密， 小写）|
|client|是|客户端（字母，小写， 浏览器为browser，其他 客户端自行命名并进行 备案|

返回说明

正确情况下的返回状态： ![ref74]  http status：201

正确情况下的返回内容如下：

` `TGT-69-kI90oiHnr4AR1XqQRTBCcfCMqOi65Q3hiInvsOCSC5LCBs2RL5- cas01.example.org-hlwpc

失败情况下的返回状态：   http status：401

失败情况下的返回内容如下：

`  `{

`     `"authentication\_exceptions":["FailedLoginException"]   }

失败情况下的返回状态：   http status：400

失败情况下的返回内容有如下五种：

`  `{

`     `"code" : "400000",

`     `"message" : "Invalid payload. 'username' and 'password'form fields  are required."

`  `}

`  `{

`     `"code" : "400000",

`     `"message" : "Invalid payload. 'client' form fields are required."   }

`  `{![ref18]

`     `"code" : "400100",

`     `"message" : "密码使用已超过30天,请及时修改密码"

`  `}

`  `{

`     `"code" : "400101",

`     `"message" : "密码输入错误次数超过限制，账号被锁定，请稍后重试或联系管理员解锁"   }

`  `{

`     `"code" : "400102",

`     `"message" : "账号已锁定，请稍后重试或联系管理员解锁"

`  `}

失败情况下的返回状态：

`  `http status： 426

失败情况下的返回内容如下：

`  `{

`     `"authentication\_exceptions" : ["AccountDisabledException"]   }

2. 生成子票据

使用本接口可以通过已授权票据生成子票据。

接口调用请求说明

`  `http请求方式：POST,HTTP调用![ref82]

`  `http://localhost:8080/cas/v1/ticketGrantingTickets/{tgtId}

请求示例：

`  `http://localhost:8080/cas/v1/ticketGrantingTickets/TGT-69- kI90oiHnr4AR1XqQRTBCcfCMqOi65Q3hiInvsOCSC5LCBs2RL5-cas01.example.org- hlwpc

参数说明



|参数|是否必须|说明|
| - | - | - |
|tgtId|是|授权票据|
|client|是|客户端（字母，小写， 浏览器为browser，其他|



|||客户端自行命名并进行 备案）|
| :- | :- | :- |

返回说明

正确情况下的返回状态： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.283.png)  http status：201

正确情况下的返回内容如下：

` `TGT-77-2QiGxAgNHnDDf46wigJ0LO3vvvd2GujVe3Upi6xSdd4wIxxaL0- cas01.example.org-browser

失败情况下的返回状态：   http status：400

失败情况下的返回内容如下：

`  `Invalid payload. 'client' form fields are equired.

失败情况下的返回状态：   http status：409

失败情况下的返回内容如下：

`  `TicketGrantingTicket was deleted for conflict.

3. 票据注销

使用本接口可以注销已授权票据。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.284.png)

`  `http://localhost:8080/cas/v1/ticketGrantingTickets/{tgtId}

请求示例：

`  `http://localhost:8080/cas/v1/ticketGrantingTickets/ TGT-77-2QiGxAgNHnDDf46wigJ0LO3vvvd2GujVe3Upi6xSdd4wIxxaL0- cas01.example.org-browser

参数说明



|参数|是否必须|说明|
| - | - | - |



|tgtId|是|授权票据|
| - | - | - |

返回说明

正确情况下的返回状态： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.285.png)  http status：200

正确情况下的返回内容如下：

` `TGT-77-2QiGxAgNHnDDf46wigJ0LO3vvvd2GujVe3Upi6xSdd4wIxxaL0- cas01.example.org-browser

4. 注销用户票据信息

使用本接口可以注销用户的票据。

接口调用请求说明

`  `http请求方式：DELETE,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.286.png)

`  `http://localhost:8080/cas/v1/revokeTicketGrantingTickets

请求示例：

`  `http://localhost:8080/cas/v1/revokeTicketGrantingTickets

参数说明



|参数|是否必须|说明|
| - | - | - |
|loginKey|是|用户登录唯一标识|
|accessToken|是|访问校验票据|

返回说明

正确情况下的返回状态： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.287.png)  http status：200

正确情况下的返回内容如下：

` `User zhangsan All TicketGrantingTickets Revoke Successful.

异常情况下返回内容如下：

` `400 Invalid payload. 'loginKey' form fields are required.

` `400 Invalid payload. 'accessToken' form fields are required.

` `400 AccessToken 无效.![ref17]

` `404 User zhangsan could not be found.

9. 接口定义**InterfaceDef**
1. 获取接口定义的分页信息

使用本接口获取接口定义的分页信息，支持条件过滤。

接口调用请求说明

http请求方式：GET,HTTP调用 ![ref42]http://localhost:8080/v1/interfacedef

请求示例：

http://localhost:8080/v1/interfacedef

参数说明



|参数|是否必须|说明|
| - | - | - |
|category|否|接口分类|
|method|否|调用方式|
|url|否|接口URL|
|orderNum|否|顺序|
|status|否|状态0：不可用，1：可 用|
|name|否|接口名称|
|id|否|接口主键id|
|createPerson|否|创建人|
|createOrg|否|创建组织|
|createTime|否|创建时间|
|updatePerson|否|更新人|
|updateOrg|否|更新组织|
|updateTime|否|更新时间|
|remark|否|备注|



|pageBound|否|分页|
| - | - | - |

返回说明

正确情况下返回HTTP头如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.288.png){

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"": ""

} 正确情况下返回状态：

200 正确情况下返回的内容如下：

{

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"category": "applications",       "method": "GET",

`      `"url": "/v1/applications/?",

`      `"orderNum": 94,

`      `"status": "1",

`      `"name": "获取应用列表",       "\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/interfacedef/94"         }

`      `},

`      `"id": 94

`    `}

`  `]

}

2. 新增接口定义

使用本接口新增接口定义。

接口调用请求说明

http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.289.png)

http://localhost:8080/v1/interfacedef ![ref48]请求示例：

http://localhost:8080/v1/interfacedef

参数说明



|参数|是否必须|说明|
| - | - | - |
|entityModel|是|<p>应用信息，数据传入格 式为json{ "category":</p><p>"string", "createOrg": "string", "createPerson": 0, "createTime": "2018-01-18T02:43:45.00 "id": 0, "method":</p><p>"string", "name": "string", "orderNum": 0, "remark": "string", "status":</p><p>"string", "updateOrg": "string", "updatePerson": 0, "updateTime": "2018-01-18T02:43:45.00 "url": "string" }</p>|

3Z",

3Z",

返回说明

正确情况下返回HTTP头： ![ref73]{

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Thu, 18 Jan 2018 02:52:02 GMT",

`  `"": ""

} 正确情况下返回状态：

201 正确情况下返回内容：

{

`  `"category": "applications",   "method": "GET",

`  `"url": "/v1/applications/vv",   "orderNum": 100,![ref60]

`  `"status": "1",

`  `"name": "获取获取",

`  `"\_links": {

`    `"self": {

`      `"href": "http://10.29.6.95:8088/v1/interfacedef/132"     }

`  `},

`  `"id": 132

} 异常情况返回：

{

`  `"timestamp": 1516243504940,   "status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "当前应用不允许访问接口",

`  `"path": "/v1/interfacedef"

}

{

`  `"timestamp": 1516243849064,

`  `"status": 400,

`  `"error": "Bad Request",

`  `"exception": "com.zts.common.spec.exception.InvalidArgumentException",   "message": "关键字接口名称已被使用，请检查",

`  `"path": "/v1/interfacedef"

}

3. 删除接口定义信息

使用本接口根据接口定义的主键id删除接口定义信息 。

接口调用请求说明

http请求方式：DELETE,HTTP调用 ![ref61]http://localhost:8080/v1/interfacedef/{id}

示例如下：

http://localhost:8080/v1/interfacedef/100

参数说明



|参数|是否必须|说明|
| - | - | - |



|id|是|接口的主键id|
| - | - | - |

返回说明

正确情况下返回HTTP头： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.290.png){

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "date": "Thu, 18 Jan 2018 05:08:13 GMT",

`  `"": ""

} 正确情况下返回状态：

204 正确情况下返回内容：

no content 异常情况下返回：

{

`  `"timestamp": 1516243999950,   "status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",   "message": "已使用不允许删除！",

`  `"path": "/v1/interfacedef/100"

}

{

`  `"timestamp": 1516244226979,

`  `"status": 403,

`  `"error": "Forbidden",

`  `"exception": "com.zts.common.spec.exception.ValidateException",

`  `"message": "校验失败：接口(查询拥有某应用系统准入权限的机构(根据主键id))状态非正常 启用",

`  `"path": "/v1/interfacedef/100" }

4. 查询接口定义信息

使用本接口根据接口定义的主键id查询接口定义信息 。

接口调用请求说明

http请求方式：GET,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.291.png)

http://localhost:8080/v1/interfacedef/{id} ![ref23]请求示例：

http://localhost:8080/v1/interfacedef/94

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|接口定义的主键id|

返回说明

正确情况下的返回HTTP头如下： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.292.png)Response Headers

{

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Thu, 18 Jan 2018 01:25:53 GMT",

`  `"": ""

}

正确情况下返回状态：

200 正确情况下返回值：

{

`  `"category": "applications",   "method": "GET",

`  `"url": "/v1/applications/?",

`  `"orderNum": 94,

`  `"status": "1",

`  `"name": "获取应用列表",   "\_links": {

`    `"self": {

`      `"href": "http://10.29.6.95:8088/v1/interfacedef/94"     }

`  `},

`  `"id": 94

}

5. 更新接口定义信息

使用本接口获取获取接口定义的分页信息，支持条件过滤。

接口调用请求说明

http请求方式：PUT,HTTP调用 ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.293.png)http://localhost:8080/v1/interfacedef/{id}

示例如下

http://localhost:8080/v1/interfacedef/133

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|接口定义的主键id|
|entityModel|是|<p>要修改的接口的信息项 及修改内容，数据传入 格式为json { "category":</p><p>"string", "createOrg": "string", "createPerson": 0, "createTime": "2018-01-18T05:07:41.98 "id": 0, "method":</p><p>"string", "name": "string", "orderNum": 0, "remark": "string", "status":</p><p>"string", "updateOrg": "string", "updatePerson": 0, "updateTime": "2018-01-18T05:07:41.98 "url": "string" }</p>|

6Z",

6Z",

返回说明

正确情况下返回HTTP头： ![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.294.png){

`  `"date": "Thu, 18 Jan 2018 03:23:22 GMT",   "server": "Apache-Coyote/1.1",

`  `"transfer-encoding": "chunked",![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.295.png)

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8"

} 正确情况下返回状态：

201 正确情况下返回内容：

1

6. 查询应用信息

使用本接口根据接口定义的主键id查询应用信息。

接口调用请求说明

http请求方式：GET,HTTP调用 ![ref36]http://localhost:8080/v1/interfacedef/{id}/application

请求示例：

http://localhost:8080/v1/interfacedef/94/application

参数说明



|参数|是否必须|说明|
| - | - | - |
|id|是|接口定义的主键id|

返回说明

正确情况下返回HTTP头： ![ref53]Response Headers

{

`  `"server": "Apache-Coyote/1.1",

`  `"x-application-context": "application:8088",

`  `"content-type": "application/json;charset=UTF-8",   "transfer-encoding": "chunked",

`  `"date": "Thu, 18 Jan 2018 03:16:57 GMT",

`  `"": ""

}

正确情况下返回状态：

200 正确情况下返回内容：

{![ref6]

`  `"total": 1,

`  `"count": 1,

`  `"items": [

`    `{

`      `"appCode": "M0019",

`      `"appName": "用户管理系统",

`      `"description": "用户管理系统",

`      `"uniauthType": "0",

`      `"redirectUrl": "http://10.25.24.71:30080/zts\_usermanagement",

`      `"appSecret": null,

`      `"status": "1",

`      `"authInfo":"{\"proxyPolicy\":{\"@class\": \"org.jasig.cas.services.RegexMatchingRegisteredServiceProxyPolicy\", \"pattern\":\"http://.\*\"},\"logoutType\":\"BACK\_CHANNEL\",\"@class \":\"org.jasig.cas.services.RegexRegisteredService\",\"evaluationOrder \":1,\"name\":\"M0019\",\"usernameAttributeProvider\":{\"@class\": \"com.zts.cas.custom.services.CustomRegisteredServiceUsernameProvider \"},\"description\":\"\\u7528\\u6237\\u7ba1\\u7406\ \u7cfb\\u7edf\",\"theme\":\"M0019\",\"id\":10000019, \"serviceId\":\"^http://((10.25.24.71)|(10.55.8.28)):30080/ zts\_usermanagement.\*\",\"attributeReleasePolicy\": {\"authorizedToReleaseCredentialPassword\":false,\"@class\": \"org.jasig.cas.services.ReturnAllowedAttributeReleasePolicy \",\"principalAttributesRepository\":{\"@class\": \"org.jasig.cas.authentication.principal.DefaultPrincipalAttributesRepository \"},\"authorizedToReleaseProxyGrantingTicket \":false},\"accessStrategy\":{\"@class\": \"org.jasig.cas.services.DefaultRegisteredServiceAccessStrategy\", \"ssoEnabled\":true,\"enabled\":true}}",

`      `"unifiedFlag": "1",

`      `"ips": "^((10.29.6.94)|(10.25.24.71))$",

`      `"\_links": {

`        `"self": {

`          `"href": "http://10.29.6.95:8088/v1/interfacedef/19"

`        `}

`      `},

`      `"id": 19

`    `}

`  `]

}

异常情况下返回： {

`  `"timestamp": 1516252348858,   "status": 404,

`  `"error": "Not Found",![ref25]

`  `"exception": "com.zts.common.spec.exception.SourceNotFoundException",   "message": "id[155]对应的接口不存在！",

`  `"path": "/v1/interfacedef/155/application"

}

10. 邮件**Mail**

**4.10.1** 发送邮件

使用本接口可以向指定的账号发送邮件。

接口调用请求说明

http请求方式：POST,HTTP调用![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.296.png)

http://localhost:8080/v1/mail

请求示例：

1、application/json

需增加http header，Content-Type:application/json;charset=utf-8 url：http://localhost:8080/v1/mail

json：

{

`  `"receiver": "test@zts.com.cn",

`  `"subject": "测试111",

`  `"text": "1111"

}

2、查询参数 url：http://localhost:8080/v1/mail?receiver=test@zts.com.cn&subject=测试邮 件&text=12222

参数说明



|参数|是否必须|说明|
| - | - | - |
|receiver|是|接收人|
|subject|是|主题|
|text|是|邮件内容|

返回说明

正确情况下返回代码：![ref78]

201 ![ref75]正确情况下返回内容：

{

`  `"code": "201",

`  `"msg": "邮件发送成功",   "\_links": {

`    `"self": {

`      `"href": "http://10.29.6.95:8088/v1/mail/0"     }

`  `}

}

错误情况下返回：

401 Unauthorized

403 Forbidden

404

Not Found

5. 数据字典



|字典代码|字典名称|值说明|
| - | - | - |
|STATUS|状态|1:启用;2:未启用|
|PERMISSION\_TYPE|权限类型|1:单个权限;2:组权限|
|PERMISSION\_LEVLE|权限级别|1:准入;2:功能菜单;3:按 钮|
|GENDER|性别|0:女;1:男;2:未知|
|GROUP\_TYPE|组类型|1:ORG;2:IDENTIFY;3:LO|
|EMP\_GRADE|职级|见下面的“职级”字典值表|
|JOB\_STATUS|岗位状态|1:在岗;2:试用;3:实习;4: 内退|
|EMP\_STATUS|员工状态|1:在职;2:离职;3:退休|
|EMP\_TYPE|员工类型|1:员工;2:管理员;99:其 他;3:经纪人|

GIC

“职级”字典值表如下：



|值代码|值名称|
| - | - |
|1|公司领导（职能序列）|
|2|部门正职（职能序列）|
|3|部门副职（职能序列）|
|4|总经理助理（职能序列）|
|5|首席经理（专业技术序列）|
|6|资深经理（专业技术序列）|
|7|资深副经理（专业技术序列）|
|8|高级经理（专业技术序列）|
|9|经理（专业技术序列）|
|10|助理（专业技术序列）|
|11|董事总经理（MD序列）|
|12|执行总经理（MD序列）|
|13|业务总监（MD序列）|
|14|业务副总监（MD序列）|
|15|高级经理（MD序列）|
|16|项目经理（MD序列）|
|17|助理（MD序列）|
|18|部门正职（分支机构）|
|19|部门副职（分支机构）|
|20|总经理助理（分支机构）|
|21|高级业务经理（分支机构）|
|22|业务经理（分支机构）|
|23|员工（分支机构）|
|25|语音代表（专业技术序列）|
|26|董事总经理（MD序列投行）|
|27|执行总经理（MD序列投行）|
|28|总监（MD序列投行）|



|29|副总裁（MD序列投行）|
| - | - |
|30|高级经理（MD序列投行）|
|31|经理（MD序列投行）|
|32|部门正职（内退）|
|33|部门副职（内退）|
|34|总经理助理（内退）|
|35|高级业务经理（内退）|
|36|业务经理（内退）|
|37|员工（内退）|
|38|行政负责人（MD序列）|
|39|首席客户经理（分支机构）|
|40|资深客户经理（分支机构）|
|41|高级客户经理（分支机构）|
|42|客户经理（分支机构）|
|43|助理客户经理（分支机构）|
|44|见习客户经理（分支机构）|
|45|实习生|
|46|首席经理（IT技术开发序列）|
|47|资深经理（IT技术开发序列）|
|48|资深副经理（IT技术开发序列）|
|49|高级业务经理（IT技术开发序列）|
|50|业务经理（IT技术开发序列）|
|51|员工（IT技术开发序列）|

6. 应用场景

业务系统接入蜂巢系统时，新增用户、删除用户、用户变更密码时，需调用以上 API列表的其中几项。根据业务系统的用户体系是否与蜂巢系统的是否一致，可 划分以下两类场景：

1. 业务系统用户与蜂巢用户体系一致，即同一用户体系
2. 业务系统用户与蜂巢用户体系不一致，即不同用户体系
1. 同一用户体系
1. 蜂巢系统的配置 蜂巢系统中需增加该业务系统的代码及对应的权限，具体如下：
1. 增加业务系统S；
1. 为业务系统S配置权限A；
1. 增加角色R；
1. 把权限A分配给角色。
2. 业务系统的配置

包括新增用户、删除用户、用户变更密码三部分。

新增用户流程

1. 业务系统根据身份证号码、手机号等信息调用【验证用户是否存在】接口获 取用户在蜂巢的基础信息，使用蜂巢的用户名在业务系统中新增用户；
1. 调用【新增用户的系统准入权限】接口为用户增加当前业务系统的访问权 限。

注意：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.297.png)

- 如果用户在蜂巢中不存在，业务系统不能新增用户![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.298.png)
- 业务系统的用户名必须与蜂巢的用户名一致

删除用户流程

1. 调用【删除用户的系统准入权限】接口删除用户访问当前业务系统的权限；
1. 业务系统删除对应的用户或者将其改为无效状态。

用户变更密码流程

1\. 业务系统调用【修改用户密码】接口修改用户的密码。

3. 用户同步 存量用户处理

   采用手工方式，在蜂巢系统中给存量用户授系统准入权限。(蜂巢开发人员负责， 开发商协助)

   各业务系统可通过接口获取蜂巢的用户。 参见接口【获取用户列表（身份证信息 除外）】

   批量用户同步

   通过"批量同步接口"获取蜂巢系统用户，定期同步蜂巢的用户。参见接口【获取 用户列表（身份证信息除外）】

   返回数据中status=0的人员，或本地有返回结果中没有的人员，均为减员人员， 业务系统对应减员。

   返回数据中status=1的人员，且本地没有的人员，均为新增人员，业务系统对应 增员。

2. 不同用户体系
1. 蜂巢系统的配置

蜂巢系统中需增加该业务系统的代码及对应的权限，具体如下：

1. 增加业务系统S；
1. 为业务系统S配置权限A；
1. 增加角色R；
1. 把权限A分配给角色；
1. 业务系统提供用户信息，蜂巢配置业务系统用户与蜂巢用户的映射关系。
2. 业务系统的配置

包括新增用户、删除用户、用户变更密码三部分

新增用户流程

1. 业务系统根据身份证号码、手机号等信息调用【验证用户是否存在】接口获 取用户在蜂巢的信息，使用该信息在业务系统中新增用户，  用户名由业务系 统设置 。业务系统的用户名可以与蜂巢用户名一致，也可以不一致;
1. 业务系统调用【新增用户的系统对应关系】接口建立新增的业务系统用户与 蜂巢用户之间的映射关系。

注意：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.299.png)![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.300.png)

- 根据身份证号码、手机号等信息在蜂巢中查不到用户信 息，不能创建用户映射关系
- 建立用户映射关系时，蜂巢自动为用户增加访问当前业 务系统的访问权限

删除用户流程

1. 业务系统调用【删除用户的应用系统对应关系】接口删除业务系统的用户与 蜂巢用户之间的映射关系；
1. 业务系统删除对应的用户或者将其改为无效状态。

注意：![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.301.png)

- 删除用户映射关系时，蜂巢自动删除该用户访问当前业 务系统的权限![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.302.png)

用户变更密码流程

1\. 业务系统调用【修改用户密码】接口修改用户的密码。

3. 用户同步 存量用户处理

   接入时，采用手工对照的方式，导入对应关系到蜂巢数据库。(蜂巢开发人员负 责，开发商协助)

   通过蜂巢用户管理系统给存量用户添加系统准入权限。(蜂巢开发人员负责)

批量用户同步

通过"批量同步接口"获取蜂巢系统用户。参见接口【获取用户列表（身份证信息 除外）】

返回数据中status=0的人员，或本地有返回结果中没有的人员，均为减员人员， 业务系统对应减员。

返回数据中status=1的人员，且没有对照关系（userMap）的人员，均为新增人 员，调用新增接口参见接口 2

3. 蜂巢系统用户减员对业务系统的影响

通过"批量同步接口"获取的数据与本地数据比对，完成本系统内人员的注销或新 增。参见接口【获取用户列表（身份证信息除外）】

对于接入cas的系统而言，因cas会判断蜂巢人员的状态，所以无需关注蜂巢用户 的减员情况。

对于没有接入cas的系统而言，需要在登入时实现蜂巢的校验密码的接口，从而 限制蜂巢减员用户的登入。

"财务系统"因使用独立的密码登入，不接入cas  也不实现蜂巢密码校验接口，此 时必须通过"批量同步接口"来核对增减员情况。参见接口【获取用户列表（身份 证信息除外）】

4. 是否接入**CAS**的选择

接入cas的目的是为了实现统一认证和单点登入，接入cas后，无需进行用户密码 和用户准入的校验。

不接入cas，需通过接口实现蜂巢用户密码和准入的校验 参见接口 【验证用户是 否存在】、【查询用户的某一系统准入权限(根据主键id)】、【校验用户的系统 准入权限】

5. 移动办公

流程附件attachment、组织机构org、待办任务task是供移动办公系统使用的接 口。移动办公系统不支持关系型数据库，需从蜂巢系统中取出待办数据并转化为 系统支持的文档型数据，由蜂巢系统提供接口，接口以XML格式返回所有待办数

据信息（包括标题、当前处理人、审批单详细信息、审批操作列表、流转日志、 意见记录等），移动办公系统定时调用【同步所有待办任务】接口同步待办数 据，调用【附件同步】接口同步格式附件(PDF格式)，调用【提交审批待办任 务】接口实现业务数据审批处理。

**7.** 常见问题

1. 校验用户是否存在

接口说明:在建立对照关系前，先确实蜂巢用户是否存在，并获取蜂巢用户数据。 请求路径:GET![ref80] http://address/v1/users/checkexist?loginKey= 入参: loginKey=[用户名/身份证/邮箱/手机号/员工编号]

出参: user对象

入参类型: url传参

2. 建立对照关系并授权

接口说明:使用post协议，创建用户对照关系。支持一次添加多个系统的关系。 请求路径:POST![ref82] http://address/v1/users/convert

入参: {"loginKey":"蜂巢人员key","ryId":"接入系统人员id","appCode":"系统code,系统code2"} 出参: userMap列表对象

入参类型: body体传参，Content-Type =application/json

补充： 支持url传参 POST http://address/v1/users/convert? loginKey=&ryId=&appCode=

3. 删除对照关系并回收权限

接口说明: 使用DELETE协议，删除用户对照关系。loginKey非必填![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.303.png)

请求路径: DELETE http://address/v1/users/convert

入参: {"loginKey":"蜂巢人员key","ryId":"接入系统人员id","appCode":"系统code"} 出参: 空集

入参类型: body体传参，Content-Type =application/json

补充： 支持url传参 DELETE http://address/v1/users/convert? loginKey=&ryId=&appCode=

4. 查询对照关系

接口说明:使用GET协议，查看用户已有的对照关系。注意：无效的也返回。 请求路径:GET![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.304.png)    http://address/v1/users/convert?loginKey=&appCode=

入参: loginKey=蜂巢人员key&appCode=系统code ![ref48]出参: userMap列表对象

入参类型: url传参

5. 用户准入权限授权接口

接口说明:使用post协议，新增用户的系统访问权限。![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.305.png)

请求路径:POST http://address/v1/users/applications

入参: {"loginKey":"蜂巢人员key","appCode":"系统code"}

出参: 权限对象

入参类型: body体传参，Content-Type =application/json

补充： 支持url传参 POST http://address/v1/users/applications? loginKey=&appCode=

6. 用户准入权限回收接口

接口说明:使用DELETE协议，删除用户的系统访问权限。![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.306.png)

请求路径:DELETE http://address/v1/users/applications 入参: {"loginKey":"蜂巢人员key","appCode":"系统code"}

出参: 空集

入参类型: body体传参，Content-Type =application/json

补充： 支持url传参 DELETE http://address/v1/users/convert? loginKey=&appCode=

7. 修改密码

接口说明: 使用put协议，修改用户密码。支持明文和密文传输，默认明文。 请求路径:![ref52] PUT http://address/v1/users/password

入参: loginKey:蜂巢key，newPassword，oldPassword，encrypt:加密方式 出参: 空集

入参类型: body体传参

8. 校验密码** （不接入**cas**时使用【验证用户是否存在】）

接口说明: POST协议，校验用户密码是否正确。支持明文和密文传输，默认明文。 请求路径:POST![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.307.png) http://address/v1/users/checkvalid

入参: loginKey:蜂巢key，password，encrypt:加密方式

出参: user对象

入参类型: body体传参

235![ref2]
API接口规范![ref1]

9. 获取用户准入权限（不接入**cas**时使用，与校验密码结合使用 【查询用户的某一系统准入权限(根据主键id)】）

接口说明: GET协议，获取用户有哪些系统的准入权限。 请求路径:get![ref83]

http://address/v1/users/applications?loginKey=&applicationId= 入参: loginKey:蜂巢key，applicationId：应用系统appcode，非必填

出参: Application对象列表

入参类型: url传参

10. 校验密码和当前系统的权限（不接入**cas**时使用【校验用户的 系统准入权限】）

接口说明: POST协议，校验用户密码是否正确，同时判断有无当前系统的权限。支持明文和密文传 输，默认明文。![ref83]

请求路径: POST http://address/v1/users/checkaccess

入参: loginKey:蜂巢key，password，encrypt:加密方式

出参: user对象

入参类型: body体传参

11. 批量获取蜂巢的用户【获取用户列表（身份证信息除外）】

接口说明: GET协议，获取拥有系统准入权限的用户列表。![](Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.309.png)

请求路径: get http://address/v1/applications/users?appCode= 入参: appCode：应用系统appcode，必填

出参: user对象列表

入参类型: url传参

237![ref2]


![ref84]![ref85]

[ref1]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.001.png
[ref2]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.002.png
[ref3]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.008.png
[ref4]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.009.png
[ref5]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.010.png
[ref6]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.014.png
[ref7]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.015.png
[ref8]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.016.png
[ref9]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.019.png
[ref10]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.020.png
[ref11]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.022.png
[ref12]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.026.png
[ref13]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.029.png
[ref14]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.031.png
[ref15]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.035.png
[ref16]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.037.png
[ref17]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.042.png
[ref18]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.045.png
[ref19]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.056.png
[ref20]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.057.png
[ref21]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.060.png
[ref22]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.061.png
[ref23]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.070.png
[ref24]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.071.png
[ref25]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.073.png
[ref26]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.078.png
[ref27]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.079.png
[ref28]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.082.png
[ref29]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.086.png
[ref30]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.090.png
[ref31]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.091.png
[ref32]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.093.png
[ref33]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.097.png
[ref34]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.098.png
[ref35]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.099.png
[ref36]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.100.png
[ref37]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.105.png
[ref38]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.107.png
[ref39]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.108.png
[ref40]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.110.png
[ref41]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.111.png
[ref42]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.112.png
[ref43]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.114.png
[ref44]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.117.png
[ref45]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.118.png
[ref46]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.119.png
[ref47]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.120.png
[ref48]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.121.png
[ref49]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.129.png
[ref50]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.133.png
[ref51]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.134.png
[ref52]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.145.png
[ref53]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.152.png
[ref54]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.153.png
[ref55]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.154.png
[ref56]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.156.png
[ref57]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.159.png
[ref58]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.160.png
[ref59]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.161.png
[ref60]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.162.png
[ref61]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.163.png
[ref62]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.167.png
[ref63]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.171.png
[ref64]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.177.png
[ref65]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.181.png
[ref66]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.182.png
[ref67]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.186.png
[ref68]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.187.png
[ref69]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.188.png
[ref70]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.189.png
[ref71]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.190.png
[ref72]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.192.png
[ref73]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.207.png
[ref74]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.212.png
[ref75]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.219.png
[ref76]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.228.png
[ref77]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.230.png
[ref78]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.241.png
[ref79]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.242.png
[ref80]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.245.png
[ref81]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.267.png
[ref82]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.282.png
[ref83]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.308.png
[ref84]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.310.png
[ref85]: Aspose.Words.bed15eb6-5863-4dad-bb0e-0c85c29c390f.311.png
