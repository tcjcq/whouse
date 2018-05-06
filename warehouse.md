<!-- TOC -->

- [1.仓库接口](#1-仓库接口)
    - [1.1.基础接口](#11-基础接口登陆)
    - [1.1.1.登陆](#111-登陆)
    - [1.1.2.注销](#112-注销)
    - [1.1.3.心跳](#113-心跳)
    - [1.1.4.仓库列表](#114-仓库列表)
    - [1.1.5.物料基础信息](#115-物料基础信息)
    - [1.1.6.供应商基本信息](#116-供应商基本信息)
    - [1.1.7.单据状态基础数据](#117-单据状态基础数据)
    - [1.1.8.入库类型基础数据](#118-入库类型基础数据)
    - [2.1.业务接口](#21-业务接口)
    - [2.1.1.入库单列表](#211-入库单列表)
    - [2.1.2.出库单列表](#212-出库单列表)
    - [2.1.3.调拨单列表](#213-调拨单列表)
    - [2.1.4.入库单详情](#214-入库单详情)
    - [2.1.5.出库单详情](#215-出库单详情)
    - [2.1.6.调拨单详情](#216-调拨单详情)
    - [2.1.7.物料入库](#217-物料入库)
    - [2.1.8.物料出库](#218-物料出库)
    - [2.1.9.物料调拨](#219-物料调拨)
    - [2.1.10.物料盘库](#2110-物料盘库)
    - [2.1.11.更新单据的状态](#2111-更新单据的状态)

<!-- /TOC -->

# 1 仓库接口

## 1.1 基础接口

## 1.1.1 登陆
        http://api.tunnel.cqksl.com:8000/Depot/Login
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| LoginId| string,not null| 登陆Id| |
| Password| string,not null| 登陆密码| `需要md5加密传输`|

- 请求示例

```java
LoginId="admin001"
Password="md5(PASSWORD)"
```

- **TOKEN**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| AccessToken| string,not null| JWT凭证| 所有权限范围的TOKEN凭据|
| ExpiresIn| int,not null| JWT凭证有效时间，单位：秒| |
| TokeyType| string,not null| JWT的授权的方案| |

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":
    {
        "Token":
        {
            "AccessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiTXJCdWciLCJleHAiOjE1MjQ0OTIzMTEuMCwianRpIjoibHVvemhpcGVuZyJ9.wNvRZIksR7NOXMAUdKx85gLrvhAmgap3g6krAytafW4",
            "ExpiresIn":7200,
            "TokeyType":"Bearer"
        }
    }
}
```

## 1.1.2 注销
        http://api.tunnel.cqksl.com:8000/Depot/Logout
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| LoginId| string,not null| 登陆Id| |

- 请求示例

```json
LoginId="admin001",
```

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":{},
}
```

## 1.1.3 心跳
        http://api.tunnel.cqksl.com:8000/Depot/Heartbeat
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**返回参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MaterielVersionMD5| string,not null| 仓库物料基础数据版本信息| |
| SupplierVersionMD5| string,not null| 仓库供应商基础数据版本信息| |

- 返回示例

```json
{
    "errcode": 10001,
    "errmsg": "",
    "data": {
        "MaterielVersionMD5": "30C169E3CEF1EF4D0CED303C55CCBD91",
        "SupplierVersionMD5": "91A576A7B270BA461E7A139429737B5A"
    },
    "status": 200
}
```

## 1.1.4 仓库列表
        http://api.tunnel.cqksl.com:8000/Depot/GeDepotList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| BDID| Guid,not null| 仓库Id| |
| Code| string,not null| 仓库编码| |
| Name| string,not null| 仓库名称| |

- 返回示例

```json
{
    "errcode": 10001,
    "errmsg": "",
    "data": [
        {
            "BDID": "e07cd825-e3d5-4760-8e1e-6b64bb9687ea",
            "Code": "DP00020",
            "Name": "成都中心仓库"
        },
        {
            "BDID": "f0f8eae4-9efe-4bc4-8c70-6f1718b18e12",
            "Code": "DP00019",
            "Name": "重庆中心库"
        }
    ],
    "status": 200
}
```

## 1.1.5 物料基础信息
        http://api.tunnel.cqksl.com:8000/Depot/GetMaterielList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MaterielVersionMD5| string,not null| 仓库物料基础数据版本信息| |

- 请求示例

```java
MaterielVersionMD5="30C169E3CEF1EF4D0CED303C55CCBD91"
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MID| Guid,not null| 物料Id|
| MCode| string,not null| 原始物料编码|
| MName| string,not null| 原始物料名称|
| Spec| string,not null| 规格|
| Unit| string,not null| 单位|
| MinWeight| float,not null| 单品重量，使用g为单位|
| Manufacturer| string,not null| 生产厂家|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "MID":"MID",
            "MCode":"MCode",
            "MName":"MName",
            "Spec":"Spec",
            "Unit":"Unit",
            "MinWeight":64.00,
            "Manufacturer":"Manufacturer"
        },
    ]
}
```

## 1.1.6 供应商基本信息
        http://api.tunnel.cqksl.com:8000/Depot/GetSupplierList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| SupplierVersionMD5| string,not null| 仓库供应商基础数据版本信息| |

- 请求示例

```java
SupplierVersionMD5="91A576A7B270BA461E7A139429737B5A"
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| SID| Guid,not null| 供应商Id|
| SCode| string,not null| 供应商编码|
| SName| string,not null| 供应商名称|
| Materiels| <mark>List\<Materiel></mark>,not null| 物料集合|

**Materiel**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MID| Guid,not null| 物料Id(原始物料Id,非供应商物料Id)|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "SID":"SID",
            "SCode":"SCode",
            "SName":"SName",
            "Materiels":[
                "<MID>",
                "<MID>",
                "<MID>",
            ]
        }
    ]
}
```

## 1.1.7 单据状态基础数据
        http://api.tunnel.cqksl.com:8000/Depot/OrderStatusList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderType| int,not null| 单据状态| 1:入库单,2:出库单,3:调拨单,4:盘库单|
- 请求示例

```java
OrderType=1
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| Name| string,not null| 状态名称| |
| Code| string,not null| 状态标识| |

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":
    [
        {
            "Name":"<Name>",
            "Code":"<Code>"
        },
        {
            "Name":"<Name>",
            "Code":"<Code>"
        }
    ]
}
```

## 1.1.8 入库类型基础数据
        http://api.tunnel.cqksl.com:8000/Depot/OrderTypeList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderType| int,not null| 单据状态| 1:入库单,2:出库单|
- 请求示例

```java
OrderType=1
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| Name| string,not null| 状态名称| |
| Code| string,not null| 状态标识| |

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":
    [
        {
            "Name":"<Name>",
            "Code":"<Code>"
        },
        {
            "Name":"<Name>",
            "Code":"<Code>"
        }
    ]
}
```

## 2.1 业务接口

## 2.1.1 入库单列表
        http://api.tunnel.cqksl.com:8000/Depot/GetWVList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderStatus| int,null| 订单状态| -1:所有,反之按状态过滤|
| SearchStr| string,null| 筛选条件| {OrderCode}|
| StartTime| DateTime,not null| 开始时间|
| EndTime| DateTime,not null| 结束时间|
| PageIndex| int,not null| 分页参数|
| PageSize| int,not null| 分页参数| 默认20|

- 请求示例

```java
OrderStatus=1
SearchStr=""
StartTime="2018-02-03"
EndTime="2018-02-03"
PageIndex=1
PageSize=20
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| TotalCount| int,not null| 总条数|
| OrderDetails| <mark>List\<Details></mark>,not null| 列表详情|

**Details**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| Code| string,not null| 入库单号|
| Depot| string,not null| 入库仓库|
| DeliveryCode| string,not null| 送货单号|
| CreateCode| string,not null| 创建人|
| OperUser| string,not null| 入库操作人|
| Type| string,not null| 入库类型|
| CreateTime| Datetime,not null| 创建时间|
| ExcuteTime| Datetime,not null| 入库时间|
| Remark| string,not null| 备注|
| Status| string,not null| 状态|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "TotalCount":128,
            "OrderDetails":[
            {
            "Code":"Code",
            "Depot":"Depot",
            "DeliveryCode":"DeliveryCode",
            "CreateCode":"CreateCode",
            "OperUser":"OperUser",
            "Type":"Type",
            "CreateTime":"CreateTime",
            "ExcuteTime":"ExcuteTime",
            "Remark":"Remark",
            "Status":"Status"
            },{
            "Code":"Code",
            "DepotID":"DepotID",
            "DeliveryCode":"DeliveryCode",
            "CreateCode":"CreateCode",
            "OperUser":"OperUser",
            "Type":"Type",
            "CreateTime":"CreateTime",
            "ExcuteTime":"ExcuteTime",
            "Remark":"Remark",
            "Status":"Status"
            }
           ]
        }
    ]
}
```

## 2.1.2 出库单列表
        http://api.tunnel.cqksl.com:8000/Depot/GetDVList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderStatus| int,null| 订单状态| -1:所有,反之按状态过滤|
| SearchStr| string,null| 筛选条件| {OrderCode}|
| StartTime| DateTime,not null| 开始时间|
| EndTime| DateTime,not null| 结束时间|
| PageIndex| int,not null| 分页参数|
| PageSize| int,not null| 分页参数| 默认20|

- 请求示例

```java
OrderStatus=1
SearchStr=""
StartTime="2018-02-03"
EndTime="2018-02-03"
PageIndex=1
PageSize=20
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| TotalCount| int,not null| 总条数|
| OrderDetails| <mark>List\<Details></mark>,not null| 列表详情|

**Details**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| Code| string,not null| 出库单号|
| Depot| string,not null| 出库仓库|
| InputDepot| string,not null| 入库仓库|
| Type| string,not null| 出库类型|
| CreateCode| string,not null| 创建人|
| OperUser| string,not null| 出库操作人|
| Verifier| string,not null| 审核人|
| CreateTime| datetime,not null| 创建时间|
| ExcuteTime| datetime,not null| 出库时间|
| CheckTime| datetime,not null| 审核时间|
| Remark| string,not null| 备注|
| Status| string,not null| 状态|
| RefuseRemark| string,not null| 审核不通过原因|


- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "TotalCount":128,
            "OrderDetails":[
                {
                    "Code":"Code",
                    "Depot":"Depot",
                    "InputDepot":"InputDepot",
                    "Type":"Type",
                    "CreateCode":"CreateCode",
                    "OperUser":"OperUser",
                    "CreateCode":"CreateCode",
                    "OperUser":"OperUser",
                    "Verifier":"Verifier",
                    "CreateTime":"CreateTime",
                    "ExcuteTime":"ExcuteTime",
                    "CheckTime":"CheckTime",
                    "Remark":"Remark",
                    "Status":"Status",
                    "RefuseRemark":"RefuseRemark"
                }
             ]
        }
    ]
}
```

## 2.1.3 调拨单列表
        http://api.tunnel.cqksl.com:8000/Depot/GetTVList
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderStatus| int,null| 订单状态| -1:所有,反之按状态过滤|
| SearchStr| string,null| 筛选条件| {OrderCode}|
| StartTime| DateTime,not null| 开始时间|
| EndTime| DateTime,not null| 结束时间|
| PageIndex| int,not null| 分页参数|
| PageSize| int,not null| 分页参数| 默认20|

- 请求示例

```java
OrderStatus=1
SearchStr=""
StartTime="2018-02-03"
EndTime="2018-02-03"
PageIndex=1
PageSize=20
```

**返回参数**<mark>List\<Object></mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| TotalCount| int,not null| 总条数|
| OrderDetails| <mark>List\<Details></mark>,not null| 列表详情|

**Details**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| Code| string,not null| 调拨单号|
| Depot| string,not null| 出库仓库|
| Hospital| string,not null| 所属医院|
| HospitalAddress| string,not null| 医院地址|
| CabinetName| string,not null| 耗材柜名称|
| CabinetCode| string,not null| 耗材柜编号|
| CabinetAddress| string,not null| 耗材柜位置|
| CreateTime| string,not null| 创建时间|
| ExcuteTime| string,not null| 出库时间|
| CheckTime| string,not null| 审核时间|
| VanningTime | string,not null| 入柜时间|
| OutboundPerson| string,not null| 出库操作人|
| InboundPerson| string,not null| 入柜操作人|
| CreateCode| string,not null| 创建人|
| Verifier| string,not null| 审核人|
| Remark| string,not null| 备注|
| Status| string,not null| 状态|
| RefuseRemark| string,not null| 审核不通过原因|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
          "TotalCount":128,
          "OrderDetails":[
                {
                    "Code":"Code",
                    "Depot":"Depot",
                    "Hospital":"Hospital",
                    "HospitalAddress":"HospitalAddress",
                    "CabinetName":"CabinetName",
                    "CabinetCode":"CabinetCode",
                    "CabinetAddress":"CabinetAddress",
                    "CreateTime":"CreateTime",
                    "ExcuteTime":"ExcuteTime",
                    "CheckTime":"CheckTime",
                    "VanningTime":"VanningTime",
                    "OutboundPerson":"OutboundPerson",
                    "InboundPerson":"InboundPerson",
                    "CreateCode":"CreateCode",
                    "Verifier":"Verifier",
                    "Remark":"Remark",
                    "Status":"Status",
                    "RefuseRemark":"RefuseRemark",
                }
            ]
        }
    ]
}
```

## 2.1.4 入库单详情
        http://api.tunnel.cqksl.com:8000/Depot/GetWVDetails
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 单据Code|

- 请求示例

```java
OrderCode="OrderCode"
```

**返回参数**<mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 入库流水号|
| Name| string,not null| 物料名称|
| Code| string,not null| 物料编码|
| Batch| string,not null| 批次号|
| Manufacturer| string,not null| 生产厂家|
| SupplierName| string,not null| 供货商|
| Count| string,not null| 数量|
| Unit| string,not null| 单位|
| Spec| string,not null| 规格|
| ContractTime| datetime,not null| 生产日期|
| ContractExpirationTime| datetime,not null| 到期日期|
| Temperature| string,not null| 存贮温度|
| Humidness| string,not null| 存贮湿度|
| Other| string,not null| 其它|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "OrderCode":"OrderCode",
            "Name":"Name",
            "Code":"Code",
            "Batch":"Batch",
            "Manufacturer":"Manufacturer",
            "SupplierName":"SupplierName",
            "Count":"Count",
            "Unit":"Unit",
            "Spec":"Spec",
            "ContractTime":"ContractTime",
            "ContractExpirationTime":"ContractExpirationTime",
            "Temperature":"Temperature",
            "Humidness":"Humidness",
            "Other":"Other",
        }
    ]
}
```

## 2.1.5 出库单详情
        http://api.tunnel.cqksl.com:8000/Depot/GetDVDetails
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 单据Code|

- 请求示例

```java
OrderCode="OrderCode"
```

**返回参数**<mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 单据Code|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "OrderCode":"OrderCode",
        }
    ]
}
```

## 2.1.6 调拨单详情
        http://api.tunnel.cqksl.com:8000/Depot/GetTVDetails
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 单据Code|

- 请求示例

```java
OrderCode="OrderCode"
```

**返回参数**<mark>
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 单据Code|

- 返回示例

```json
{
    "status":200,
    "errcode":"10001",
    "errmsg":"",
    "data":[
        {
            "OrderCode":"OrderCode",
        }
    ]
}
```

## 2.1.7 物料入库
        http://api.tunnel.cqksl.com:8000/Depot/WV
        http请求方式：POST
        Content-Type=application/json

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| UserId| string,not null| 操作者|
| ExcuteTime| string,not null| 操作时间|
| OrderNo| string,not null| 单据编号|
| Remark| string,not null| 操作备注|
| Materials| List<<mark>Material</mark>>,not null| 物料列表|

- **Material**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MaterielId| string,not null| 物料Id|
| SupplierId| string,not null| 供应商Id|
| Batchs|  List<<mark>Batch</mark>>,not null| 批次列表|

- **Batch**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| BatchNo| string,not null| 批次号|
| Serials| List<<mark>Serial</mark>>,not null| 流水列表|

- **Serial**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| ProductionDate| string,not null| 生产日期|
| GuaranteePeriod| string,not null| 过期日期|
| Count| string,not null| 出入库数量|

- 请求示例

```json
{
    "UserId":"UserId",
    "ExcuteTime":"ExcuteTime",
    "OrderNo":"OrderNo",
    "Remark":"Remark",
    "Materials":
    [
        {
            "MaterielId":"MaterielId",
            "SupplierId":"SupplierId",
            "Batchs":
            [
                {
                    "BatchNo":"BatchNo",
                    "Serials":
                    [
                        {
                            "ProductionDate":"ProductionDate",
                            "GuaranteePeriod":"GuaranteePeriod",
                            "Count":"Count"
                        }
                    ]
                }
            ]
        }
    ]
}
```

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":{},
}
```

## 2.1.8 物料出库
        http://api.tunnel.cqksl.com:8000/Depot/DV
        http请求方式：POST
        Content-Type=application/json

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| UserId| string,not null| 操作者|
| ExcuteTime| string,not null| 操作时间|
| OrderNo| string,not null| 单据编号|
| Remark| string,not null| 操作备注|
| Materials| List<<mark>Material</mark>>,not null| 物料列表|

- **Material**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MaterielId| string,not null| 物料Id|
| SupplierId| string,not null| 供应商Id|
| Batchs|  List<<mark>Batch</mark>>,not null| 批次列表|

- **Batch**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| BatchNo| string,not null| 批次号|
| Serials| List<<mark>Serial</mark>>,not null| 流水列表|

- **Serial**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| ProductionDate| string,not null| 生产日期|
| GuaranteePeriod| string,not null| 过期日期|
| Count| string,not null| 出入库数量|

- 请求示例

```json
{
    "UserId":"UserId",
    "ExcuteTime":"ExcuteTime",
    "OrderNo":"OrderNo",
    "Remark":"Remark",
    "Materials":
    [
        {
            "MaterielId":"MaterielId",
            "SupplierId":"SupplierId",
            "Batchs":
            [
                {
                    "BatchNo":"BatchNo",
                    "Serials":
                    [
                        {
                            "ProductionDate":"ProductionDate",
                            "GuaranteePeriod":"GuaranteePeriod",
                            "Count":"Count"
                        }
                    ]
                }
            ]
        }
    ]
}
```

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":{},
}
```

## 2.1.9 物料调拨
        http://api.tunnel.cqksl.com:8000/Depot/TV
        http请求方式：POST
        Content-Type=application/json

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| UserId| string,not null| 操作者|
| ExcuteTime| string,not null| 操作时间|
| OrderNo| string,not null| 单据编号|
| Remark| string,not null| 操作备注|
| Materials| List<<mark>Material</mark>>,not null| 物料列表|

- **Material**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MaterielId| string,not null| 物料Id|
| SupplierId| string,not null| 供应商Id|
| Batchs|  List<<mark>Batch</mark>>,not null| 批次列表|

- **Batch**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| BatchNo| string,not null| 批次号|
| Serials| List<<mark>Serial</mark>>,not null| 流水列表|

- **Serial**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| ProductionDate| string,not null| 生产日期|
| GuaranteePeriod| string,not null| 过期日期|
| Count| string,not null| 出入库数量|

- 请求示例

```json
{
    "UserId":"UserId",
    "ExcuteTime":"ExcuteTime",
    "OrderNo":"OrderNo",
    "Remark":"Remark",
    "Materials":
    [
        {
            "MaterielId":"MaterielId",
            "SupplierId":"SupplierId",
            "Batchs":
            [
                {
                    "BatchNo":"BatchNo",
                    "Serials":
                    [
                        {
                            "ProductionDate":"ProductionDate",
                            "GuaranteePeriod":"GuaranteePeriod",
                            "Count":"Count"
                        }
                    ]
                }
            ]
        }
    ]
}
```

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":{},
}
```

## 2.1.10 物料盘库
        http://api.tunnel.cqksl.com:8000/Depot/Inventory
        http请求方式：POST
        Content-Type=application/json

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MID| Guid,not null| 物料Id| |

- 请求示例

```java
MID="MID"
```

**返回参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderNo| string,not null| 单据编号|
| Type| int,not null| 单据类型| 0入库 1出库|
| Type2| int,not null| 单据二级类型| |
| CreateTime| string,not null| 创建时间|
| CreateUser| string,not null| 创建人Id|
| DepotId| string,not null| 仓库Id|
| Remark| string,not null| 备注|
| Materials| List<<mark>Material</mark>>,not null| 物料列表|
> `注意`：

```java
Type2:
入库单：
    1-采购入库
    2-调库入库
    3-损耗入库
    10-其他入库

出库单：
    1-调库出库
    2-退货出库
    3-销售出库
    4-损耗出库

调拨单：
    此字段为空
```
- **Material**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| MaterielId| string,not null| 物料Id|
| SupplierId| string,not null| 供应商Id|
| BatchNo| string,not null| 批次号|
| ProductionDate| string,not null| 生产日期|
| GuaranteePeriod| string,not null| 过期日期|
| Count| string,not null| 数量|

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":
    {
        "OrderNo":"",
        "Type":"0",
        "Type2":"1",
        "CreateTime":"2018-02-03",
        "CreateUser":"CreateUser",
        "DepotId":"DepotId",
        "Remark":"Remark",
        "Materials":
        [
            {
                "MaterielId":"MaterielId",
                "SupplierId":"SupplierId",
                "BatchNo":"BatchNo",
                "ProductionDate":"ProductionDate",
                "GuaranteePeriod":"GuaranteePeriod",
                "Count":"100",
            }
        ]
    }
}
```

## 2.1.11 更新单据的状态
        http://api.tunnel.cqksl.com:8000/Depot/UpdateOrderStatus
        http请求方式：POST
        Content-Type=application/x-www-form-urlencoded

**请求参数**
>
| 参数名| 参数类型| 参数说明| 备注|
| :--| :--| :--| :--|
| OrderCode| string,not null| 单据的编码| |
| OrderStatus| int,not null| 单据状态| |

- 请求示例

```java
OrderCode="S001"
OrderStatus=1
```

- 返回示例

```json
{
    "status":200,
    "errcode":"s0001",
    "errmsg":"",
    "data":{},
}
```