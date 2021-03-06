
## 通用服务端接口

### 商品下单

**说明**：聚合支付模式和账户托管模式在您的应用发起支付前，都必须先调用商品下单接口来进行下单，并将应答的 pay_info 透传给米大师 SDK，拉起支付。

**接口**：unified_order

**地址**：`https://api.openmidas.com/v1/r/$appid$/unified_order`

> 注意：聚合支付模式下 `appid` 为您 MobileLine 控制台上移动支付的 `offerid`。
> 托管账户模式下 `appid` 为该托管账户的 id。

**请求方式**：POST

**参数格式**：k=v

**参数说明如下**：


字段名 | 类型 | 必填 | 含义
---- | --- | ---- | ---
user\_id | string[255] | 是 | 用户 ID，长度不小于 5 位，仅支持字母和数字的组合。
out\_trade\_no |	string[32]|	是	| 应用支付订单号，米大师要求应用订单号保持唯一性（建议根据当前系统时间加随机序列来生成订单号）。重新发起一笔支付要使用原订单号，避免重复支付；已支付过或已调用关单的订单号不能重新发起支付。仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。
product\_id |	string[128] |	是 |	商品 id，仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。
currency\_type |	string[3]	| 是	| ISO 货币代码，CNY
amount |	int |	是	| 支付金额，单位： 分
product\_name	| string[128] | 	是	| 商品名称
product\_detail |	string[255] | 是	 | 商品详情
ts |	string[10]	| 是	| UNIX 时间戳（格林威治时间），精确到秒。
sign |	string[32] |	是	| [请求签名](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97.md)
sub\_appid	| string[50] |	否	| 子应用 ID
channel |	string[10]|	否	| 指定支付渠道：wechat：微信支付； qqwallet：QQ 钱包
type | string[10] | 否 | 如果是账户托管充值，值必须为 save
metadata |	string[255]	| 否	| 透传字段，支付成功回调透传给应用，用于业务透传自定义内容。
num	| int	| 否	| 购买数量，托管账户充值下有效。
callback_url | string[255] | 否 | Web 端回调地址
wx\_openid | string[50] | 否 | 微信公众号支付时，需要传微信下的openid
platform\_income | int | 否 | 平台应收金额，单位：分
buss\_settle\_amount | int | 否 | 结算应收金额，单位：分
platform\_income\_detail | array[100] | 否 | 平台应收金额详情。格式：json，如：{"A":50,"B":50}
sub\_out\_trade\_no\_list | array[512] | 否 | 子订单信息列表

子订单信息列表格式：子订单号、子应用 ID、金额
```
[
	{
	    "sub_out_trade_no" : "2017112700001",
	    "sub_appid" : "1450000766",
	    "amount" : 999,
	    "product_name" : "子订单物品名称",
	    "product_detail" : "子订单物品描述",
	    "platform_income" : 100,
	    "buss_settle_amount" : 899,
	    "platform_income_detail" : {"A":50,"B":50},
	    "metadata" : "test1"
	},
	{
	    "sub_out_trade_no" : "2017112700002",
	    "sub_appid" : "900000490",
	    "amount" : 888,
	    "product_name" : "子订单物品名称",
	    "product_detail" : "子订单物品描述",
	    "platform_income" : 100,
	    "buss_settle_amount" : 788,
	    "platform_income_detail" : {"A":50,"B":50},
	    "metadata" : "test2"
	}
]
```

返回 JSON 结果

字段名 | 类型 | 含义
---- | ---- | ---
ret |	int	 | 结果码 0：成功；其他：失败
msg	| string[512] | 	失败的错误信息。
transaction_id |	string[32] |	米大师的交易订单。
out\_trade\_no |	string[32] |	应用支付订单号
pay_info	| string[512]	 | 支付参数，透传给米大师 SDK


**示例请求**

```
POST https://api.openmidas.com/v1/r/TC100008/unified_order?amount=1&channel=wechat&currency_type=CNY&original_amount=1&out_trade_no=open_1519638186904&product_detail=你懂得&product_id=product_test&product_name=金元宝&sign=ZyF0PZ0IzuyZtND6QPD78Cb59qEHd/VCugJ90ID8S+p9umy6M/6wbLDbhIEV+xwQEqGjMomHa/L+AgNtfT5xLQ==&ts=1519609386&user_id=rickenwang HTTP/1.1
User-Agent: Fiddler
Host: api.openmidas.com
Content-Length: 0

```

**示例响应**

```
HTTP/1.1 200 OK
Server: nginx/1.12.2
Date: Mon, 26 Feb 2018 10:22:05 GMT
Content-Type: text/html;charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
UUID: 5a93dfcd-bb51-41be-96bb-a49438514eae
METRIC: 1

{"ret" : 0,"transaction_id" : "E-180226180100230001","out_trade_no" : "open_1519640480282","pay_info" : "data=%7B%22appid%22%3A%22TC100008%22%2C%22user_id%22%3A%22rickenwang%22%2C%22out_trade_no%22%3A%22open_1519640480282%22%2C%22product_id%22%3A%22product_test%22%2C%22pay_method%22%3A%22wechat%22%7D&sign=cQHQYAdrn4ECpiAGiWqZEcynw%2BeNhc2AdXqtYoOqZvXn2%2BCupvB4fcH%2BcUQEPZfikgjwxM2oGyImPTtFIMp1HXUJz0sNH5C1ZEUR00%2FQ%2FkdEQyZg32MwC3Pom0d6IbM3L%2BBLcX8NbIf3QD7mrneL9ahng%2BZ8eo50jXhRNQv3UGPHiqSoR7JVjpIQEq45vj8eVjzi5SSDFF6OGmZgn8WiYXdGXd7bOTpTakOnDukLOlNzwUHKvKk70k5PjaRneJIbEu0xG0pQCpk0253fg8RhJola2Ej8iGFAYpbnjpH7oScDGSPir0PYTtD3zKolL5dFV8FzKi%2FVufIB3yrO4z0knQ%3D%3D"}
```

## 聚合支付服务端接口

### 查询订单

**说明**：根据订单号，或者用户 ID，查询支付订单状态

**接口**：query_order

**地址**：`https://api.openmidas.com/v1/r/$appid$/query_order`

> 物品直购下 `appid` 为您 MobileLine 控制台上移动支付的 `offerid`。
> 账户充值下 `appid` 为该账户的 id。

**请求方式**：POST

**参数格式**：k=v

**参数说明如下**：



<table>
   <tr>
      <td>字段名</td>
      <td>类型</td>
      <td>必填</td>
      <td>含义</td>
   </tr>
   <tr>
      <td>user_id</td>
      <td>string[255]</td>
      <td>是</td>
      <td>用户ID，长度不小于 5 位，仅支持字母和数字的组合。</td>
   </tr>
   <tr>
      <td>type</td>
      <td>string[10]</td>
      <td>是</td>
      <td>type=by_order，根据用户 id 查订单；
      type=by_user，根据订单号查订单</td>
   </tr>
   <tr>
      <td>ts</td>
      <td>string[10]</td>
      <td>是</td>
      <td>unix时间戳（格林威治时间）,精确到秒。</td>
   </tr>
   <tr>
      <td>sign</td>
      <td>string[32]</td>
      <td>是</td>
      <td>请求签名</td>
   </tr>

   <tr>
      <td colspan=4><center>type=by_order，以下两个字段，必须传一个，都传的话，优先使用out_trade_no</center></td>
   </tr>
   <tr>
      <td>out_trade_no</td>
      <td>string[32]</td>
      <td>否</td>
      <td>应用支付订单号</td>
   </tr>
   <tr>
      <td>transaction_id</td>
      <td>string[32]</td>
      <td>否</td>
      <td>调用下单接口获取的米大师交易订单。</td>
   </tr>
   <tr>
      <td colspan=4><center>type=by_user</center></td>
   </tr>
   <tr>
      <td>starttime</td>
      <td>string[64]</td>
      <td>是</td>
      <td>查询的起始时间，格式：unix 时间戳（格林威治时间）,精确到秒。</td>
   </tr>
   <tr>
      <td>endtime</td>
      <td>string[64]</td>
      <td>是</td>
      <td>查询的结束时间，格式：unix 时间戳（格林威治时间）,精确到秒。</td>
   </tr>
   <tr>
      <td>num</td>
      <td>int</td>
      <td>是</td>
      <td>每页返回的记录数。根据用户号码查询订单列表时需要传，用于分页展示。</td>
   </tr>
   <tr>
      <td>off_set</td>
      <td>int</td>
      <td>是</td>
      <td>记录数偏移量，默认从 0 开始。根据用户号码查询订单列表时需要传，用于分页展示。</td>
   </tr>
</table>


**返回 JSON 结果**

字段名 | 类型 | 含义
---- | ---- | ---
ret  |	int	 |结果码 0：成功；其他：失败
msg  |	string[512] |	失败的错误信息。
appid |	string[50]	 | 米大师的应用 ID
sub_appid |	string[50] |	子应用 ID
out_trade_no |	string[255]	 |应用支付订单号
sub_out_trade_no_list	 |array	 |调用下单接口传进来的sub_out_trade_no_list
transaction_id	 |string[32] |	调用下单接口获取的米大师交易订单。
user_id |	string[32] |	用户 ID
pay_channel	 |string[12]	 | 支付渠道：wechat：微信支付；qqwallet：QQ 钱包；bank：网银
product_id |	string[255]	 | 物品 id
metadata	 |string[255]	 |发货标识，由业务在调用米大师下单接口的时候下发。
currency_type |	string[3]	 |ISO 货币代码，CNY
amount |	int |	支付金额，单位：分。
order_state |	string[2]	 | 当前订单的订单状态 0： 初始状态，获取米大师交易订单成功；1： 拉起米大师支付页面成功，用户未支付；2：用户支付成功，正在发货；3：用户支付成功，发货失败；4：用户支付成功，发货成功；5：米大师支付页面正在失效中；6：米大师支付页面已经失效；"
order_time  |	string[24] |	下单时间，格式：YYYY-MM-DD hh:mm:ss
pay_time	 |string[24]	 |支付时间，格式：YYYY-MM-DD hh:mm:ss
callback_time	 |string[24] |	支付回调时间，格式：YYYY-MM-DD hh:mm:ss
refund_time	 |string[24]	 |退款时间，格式：YYYY-MM-DD hh:mm:ss

### 关闭订单

**说明**：根据订单号关闭订单

**接口**：close_order

**地址**：`https://api.openmidas.com/v1/r/$appid$/close_order`
> 物品直购下 `appid` 为您 MobileLine 控制台上移动支付的 `offerid`。
> 账户充值下 `appid` 为该账户的 id。


**请求方式**：POST

**参数格式**：k=v

**参数说明如下**：

<table>
   <tr>
      <td>字段名</td>
      <td>类型</td>
      <td>必填</td>
      <td>含义</td>
   </tr>
   <tr>
      <td>user_id</td>
      <td>string[255]</td>
      <td>是</td>
      <td>用户ID，长度不小于 5 位，仅支持字母和数字的组合。</td>
   </tr>
   <tr>
      <td>ts</td>
      <td>string[10]</td>
      <td>是</td>
      <td>unix时间戳（格林威治时间）,精确到秒。</td>
   </tr>
   <tr>
      <td>sign</td>
      <td>string[32]</td>
      <td>是</td>
      <td>请求签名</td>
   </tr>
   <tr>
      <td colspan=4><center>以下两个字段，必须传一个，都传的话，优先使用out_trade_no</center></td>
   </tr>
   <tr>
      <td>out_trade_no</td>
      <td>string[32]</td>
      <td>否</td>
      <td>应用支付订单号。优先使用应用交易订单号。</td>
   </tr>
   <tr>
      <td>transaction_id</td>
      <td>string[32]</td>
      <td>否</td>
      <td>调用下单接口获取的米大师交易订单。</td>
   </tr>
</table>

**返回 JSON 结果**

字段名 | 类型 | 含义
---- | ---- | ---
ret |	int	|结果码 0：成功；其他：失败
msg	 | string[512]	| 失败的错误信息。


### 退款

**说明**：如交易订单需退款，可以通过本接口将支付款全部或部分退还给付款方，米大师将在收到退款请求并且验证成功之后，按照退款规则将支付款按原路退回到支付帐号。最长支持 1 年的订单退款。

**接口**：refund

**地址**：`https://api.openmidas.com/v1/r/$appid$/refund`

> 物品直购下 `appid` 为您 MobileLine 控制台上移动支付的 `offerid`。
> 账户充值下 `appid` 为该账户的 id。

**请求方式**：POST

**参数格式**：k=v

**参数说明如下**：

<table>
   <tr>
      <td>字段名</td>
      <td>类型</td>
      <td>必填</td>
      <td>含义</td>
      <td></td>
   </tr>
   <tr>
      <td>user_id</td>
      <td>string[255]</td>
      <td>是</td>
      <td>用户ID，长度不小于 5 位，仅支持字母和数字的组合。</td>
      <td></td>
   </tr>
   <tr>
      <td>refund_id</td>
      <td>string[32]</td>
      <td>是</td>
      <td>退款订单号，仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。</td>
      <td></td>
   </tr>
   <tr>
      <td>amount</td>
      <td>int</td>
      <td>是</td>
      <td>退款金额，单位：分</td>
      <td></td>
   </tr>
   <tr>
      <td>platform_income</td>
      <td>int</td>
      <td>否</td>
      <td>平台应收金额，单位：分</td>
      <td></td>
   </tr>
   <tr>
      <td>buss_settle_amount</td>
      <td>int</td>
      <td>否</td>
      <td>结算应收金额，单位：分</td>
      <td></td>
   </tr>
   <tr>
      <td>platform_income_detail</td>
      <td>array[100]</td>
      <td>否</td>
      <td>平台应收金额详情。格式：json，如：{"A":50,"B":50}</td>
   </tr>

   <tr>
      <td>ts</td>
      <td>string[10]</td>
      <td>是</td>
      <td>unix时间戳（格林威治时间）,精确到秒。</td>
      <td></td>
   </tr>
   <tr>
      <td>sign</td>
      <td>string[32]</td>
      <td>是</td>
      <td>请求签名</td>
      <td></td>
   </tr>
   <tr>
      <td colspan=4><center>以下两个字段，必须传一个，都传的话，优先使用 out_trade_no</center></td>

   </tr>
   <tr>
      <td>out_trade_no</td>
      <td>string[32]</td>
      <td>否</td>
      <td>应用支付订单号。</td>
      <td></td>
   </tr>
   <tr>
      <td>transaction_id</td>
      <td>string[32]</td>
      <td>否</td>
      <td>调用下单接口获取的米大师交易订单。</td>
      <td></td>
   </tr>
   <tr>
      <td colspan=4><center>如果需要对子订单进行退款，需要传以下字段</center></td>

   </tr>
   <tr>
      <td>sub_out_trade_no</td>
      <td>string[32]</td>
      <td>否</td>
      <td>子订单号，一次只支持一个子订单号。</td>
      <td></td>
   </tr>
</table>

**返回 JSON 结果**

字段名 | 类型 | 含义
---- | ---- | ---
ret	| int	| 结果码 0：成功；其他：失败
msg	|string[512]|	失败的错误信息。

### 查询退款结果

**说明**：提交退款申请后，通过调用该接口查询退款状态。退款可能有一定延时，用微信零钱支付的退款约 20 分钟内到账，银行卡支付的退款约 3 个工作日后到账。

**接口**：query_refund

**地址**：`https://api.openmidas.com/v1/r/$appid$/query_refund`

> 物品直购下 `appid` 为您 MobileLine 控制台上移动支付的 `offerid`。
> 账户充值下 `appid` 为该账户的 id。

**请求方式**：POST

**参数格式**：k=v

**参数说明如下**：

字段名 | 类型 | 必填 | 含义
---- | --- | ---- | ---
user_id	|string[255]|	是	|用户 ID，长度不小于 5 位，仅支持字母和数字的组合。
refund_id	|string[32]|	是	|退款订单号，应用需要保持唯一性。仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。"
ts	| string[10]	| 是| 	unix 时间戳（格林威治时间）,精确到秒。
sign|	string[32]	|是	|[请求签名](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97.md)

**返回 JSON 结果**

字段名 | 类型 | 含义
---- | ---- | ---
ret	 |int	|结果码 0 ：成功；其他：失败
msg	 | string[512]	| 失败的错误信息。
refund_id |	string[32]	|退款订单号
state	| int	| 退款状态码 1：退款中；2：退款成功；3：退款失败；


## 托管账户服务端接口

### 查询余额

**说明**：查询查询账户余额

**接口**：get_balance

**地址**：`https://api.openmidas.com/v1/r/$accoutid$/get_balance`

**请求方式**：POST

**参数格式**：JSON

请求参数说明：

|字段名|类型|必填|含义|
|:--:|:--|:--|:--|
|user_id|string[255]|是|用户ID，长度不小于5位，仅支持字母和数字的组合。|
|ts|string[10]|是|unix时间戳（格林威治时间），精确到秒。|
|sign|string[32]|是|[签名](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97.md)|


返回的 JSON 结果

|字段名|类型|含义|
|:--|:--|:--|
|ret|int|结果码 0：成功，其他：失败|
|balance|int|当前余额|
|gen_balance|int|当前赠送部分余额|
|save_amt|int|累计充值金额|

### 扣款

**说明**：扣款

**接口**：pay

**地址**：`https://api.openmidas.com/v1/r/$accoutid$/pay`

**请求参数**：POST

**参数格式**：JSON

请求参数说明：

|字段名|类型|必填|含义|
|:--:|:--|:--|:--|
|user_id|string[255]|是|用户ID，长度不小于5位，仅支持字母和数字的组合。|
|amt|int|是|扣款金额|
|billno|string[32]|是|订单号，仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。|
|ts|string[10]|是|unix时间戳（格林威治时间），精确到秒。|
|sign|string[32]|是|[签名](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97.md)|

返回的 JSON 结果

|字段名|类型|含义|
|:--|:--|:--|
|ret|int|结果码 0：成功，其他：失败|
|billno|string[32]|订单号|
|balance|int|当前余额|
|gen_balance|int|当前赠送部分余额|
|used_gen_amt|int|本次支付使用的赠送部分金额|



### 取消扣款

**说明**：取消扣款

**接口**：cancel_pay

**地址**：`https://api.openmidas.com/v1/r/$accoutid$/cancel_pay`

**请求方式**：POST

**参数格式**：JSON

请求参数说明：

|字段名|类型|必填|含义|
|:--:|:--|:--|:--|
|user_id|string[255]|是|用户ID，长度不小于5位，仅支持字母和数字的组合。|
|billno|string[32]|是|订单号，仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。|
|ts|string[10]|是|unix时间戳（格林威治时间），精确到秒。|
|sign|string[32]|是|[签名](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97.md)|

返回的 JSON 结果

|字段名|类型|含义|
|:--|:--|:--|
|ret|int|结果码 0：成功，其他：失败|
|billno|string[32]|订单号|
|balance|int|当前余额|
|gen_balance|int|当前赠送部分余额|


### 赠送

**说明**：赠送接口

**接口**：present

**地址**：`https://api.openmidas.com/v1/r/$accoutid$/present`

**请求方式**：POST

**参数格式**：JSON

请求参数说明：

|字段名|类型|必填|含义|
|:--:|:--|:--|:--|
|user_id|string[255]|是|用户ID，长度不小于5位，仅支持字母和数字的组合。|
|amt|int|是|赠送金额|
|billno|string[32]|是|订单号，仅支持数字、字母、下划线（_）、横杠字符（-）、点（.）的组合。|
|ts|string[10]|是|unix时间戳（格林威治时间），精确到秒。|
|sign|string[32]|是|[签名](https://github.com/tencentyun/tac-documents/blob/master/%E5%BC%80%E5%A7%8B%E4%BD%BF%E7%94%A8/%E6%94%AF%E4%BB%98%20Payment%20%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97.md)|

返回的 JSON 结果

|字段名|类型|含义|
|:--|:--|:--|
|ret|int|结果码 0：成功，其他：失败|
|billno|string[32]|订单号|
|balance|int|当前余额|
|gen_balance|int|当前赠送部分余额|
