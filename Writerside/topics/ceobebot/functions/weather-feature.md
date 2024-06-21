# 天气预报

对于很多功能，比如 [random-restaurant](random-restaurant.md)，需要根据当前用户的地理位置信息，天气信息等来进行更好的推荐。这个插件便是使用了 [彩云天气](https://www.caiyunapp.com) 所提供的天气接口来获取天气信息。

## 插件配置

如果需要启用此插件，需要在 ceobebot 的插件配置中添加如下配置：

```yaml
weather_forecast_configs:
  # 彩云天气的 API 配置
  caiyun_api:
    # 彩云天气的 API Token，必填
    api_token: "{{YOUR_TOKEN}}"
    
    # 彩云天气的 API Base URL，选填，不填默认为 https://api.caiyunapp.com
    base_url: "https://api.caiyunapp.com"
    
    # 彩云天气的 API Version，选填，不填默认为 v2.6
    api_version: "v2.6"
    
    # 行政区划到经纬度的映射表，选填，不填为彩云天气提供的默认映射表
    location_mapping: "{{YOUR_LOCATION_MAPPING}}"
    
  # 手动指定的特定地理位置的经纬度信息，会覆盖 location_mapping 中的信息
  address_mapping:
    # 格式为 "地名": [经度, 纬度]，机器人会以 map[string][2]float64 的形式解析
    "上海": [121.472644, 31.231706]
```

## 指令功能

### 设置群聊/私聊地理位置

#### 触发命令

1. 设置群聊地理位置

    ```text
    设置群位置 <地名>
    ```

2. 设置私聊地理位置

    ```text
    设置位置 <地名>
    ```
   
#### 调用权限

1. 群聊地理位置设置需要管理员权限
2. 私聊地理位置设置不需要权限

#### 响应

1. 设置成功

    ```text
    已成功设置地理位置为 <地名>，经度为 <经度>，纬度为 <纬度>
    ```
   
2. 设置失败，没有找到地理位置

    ```text
    未找到地理位置 <地名>
    ```

### 查询实时天气

#### 触发命令

1. 查询群聊地理位置的实时天气

    ```text
    查询群实时天气
    ```
   
2. 查询私聊地理位置的实时天气

    ```text
    查询实时天气
    ```
   
3. 查询特定位置的实时天气

    ```text
    查询实时天气 <地名>
    ```

#### 调用权限

1. 查询实时天气不需要权限

#### 响应

1. 查询成功

    ```text
    已同步<时间><地名>的实时天气情况，现为您播报：
    天气情况：<天气>
    地表温度：<温度>
    地表湿度：<湿度>
    体感温度：<体感温度>
    可视距离：<能见度>
    短波辐射：<紫外线指数>
    地表风速：<风速>
    空气质量：<空气质量>
    未来预报：<未来预报>
    气象预警：<气象预警信息>
    实时天气将缓存一小时，重复查询不会更新
    ```
   
2. 查询失败，当前未设置地理位置

    ```text
    当前未设置地理位置，请使用 "设置群位置" 命令设置
    ```
   
3. 查询失败，没有找到地理位置

    ```text
    未找到地理位置 <地名>
    ```

## 插件接口

如果需要在其他插件内调用本插件的功能，您需要使用下面的方式来获取插件实例：

```Go
import (
    "github.com/alioth-center/ceobebot-plugins/plugins/weather"
    "github.com/alioth-center/ceobebot-core/core"
)

var wfi = core.GetIfrace[weather.WFI](weather.IfraceName)
```

使用时你需要注意，`WFI` 接口不会返回无意义的错误，`error` 不为 `nil` 时，表示调用失败，此时 `Response` 为无效值。

### 获取行政区划经纬度

#### 接口定义

```Go
GetLocation(ctx context.Context, cityName string) (Location, error)
```

获取行政区划的经纬度信息，如果没有找到对应的行政区划，会返回一个空的 `Location` 结构体并报错。

#### 参数说明

|   参数名称   |       参数类型        |  参数说明  |          示例值           |
|:--------:|:-----------------:|:------:|:----------------------:|
|   ctx    | `context.Context` | 上下文对象  | `context.Background()` |
| cityName |     `string`      | 行政区划名称 |        `"上海市"`         |

注意，`cityName` 将会使用

```SQL
select * from loc_map where address like '%${cityName}%' limit 1;
```

方式进行查询，如果有多个匹配项，将会返回第一个匹配项。比如 `长宁` 会匹配到：`上海市长宁区` 和 `四川省宜宾市长宁县`，此时将会返回 `上海市长宁区`，如果需要精确匹配，请使用完整的行政区划名称。

#### 响应说明

`Location` 结构体如下：

|       字段名称       |   字段类型    |  字段说明  |     示例值      |
|:----------------:|:---------:|:------:|:------------:|
|      Adcode      | `string`  | 行政区划代码 |  `"310000"`  |
| FormattedAddress | `string`  | 行政区划名称 |   `"上海市"`    |
|    Longitude     | `float64` |   经度   | `121.472644` |
|     Latitude     | `float64` |   纬度   | `31.231706`  |

### 获取天气实时信息

#### 接口定义

```Go
GetWeather(ctx context.Context, location Location) (Weather, error)
```

获取行政区划的实时天气信息，如果没有找到对应的行政区划，会返回一个空的 `Weather` 结构体并报错。

#### 参数说明

|   参数名称   |       参数类型        |  参数说明  |            示例值            |
|:--------:|:-----------------:|:------:|:-------------------------:|
|   ctx    | `context.Context` | 上下文对象  |  `context.Background()`   |
| location |    `Location`     | 行政区划信息 | `{121.472644, 31.231706}` |

#### 响应说明

结构体较大，不适合在文档中展示，可以到 [彩云天气 - 实时天气数据](https://docs.caiyunapp.com/weather-api/v2/v2.6/1-realtime.html) 或者 [caiyun/weather.go](https://github.com/sunist-c/infrastructure/blob/b4fa2ec66f01d29a97c02d1f9cd0e337872d6a52/thirdparty/caiyun/model.go#L5) 查看具体的结构体定义。

### 获取群聊/私聊地理位置

```Go
GetUserLoc(ctx context.Context, id int64, type string) (Location, error)
```

获取群聊/私聊设置的地理位置信息，如果群聊/私聊没有设置地理位置信息，会返回一个空的 `Location` 结构体并报错。

#### 参数说明

| 参数名称 |       参数类型        |   参数说明   |          示例值           |
|:----:|:-----------------:|:--------:|:----------------------:|
| ctx  | `context.Context` |  上下文对象   | `context.Background()` |
|  id  |      `int64`      | 群聊/私聊 ID |      `1234567890`      |
| type |     `string`      | 群聊/私聊类型  | `"group"`/`"private"`  |

#### 响应说明

同上文 `Location` 结构体。

## 进阶能力

### 自制地理位置映射表

地理位置映射表是一个将行政区划映射到经纬度的表格，可以通过配置文件的方式进行配置。如果不配置，插件会使用彩云天气提供的默认映射表。截止 2024年6月，彩云天气提供的默认映射表包含了中国大陆的所有行政区划，可以转到 [20240617-adcode](https://docs.caiyunapp.com/weather-api/20240617-adcode.csv) 查看其具体内容。

如果需要自制地理位置映射表，可以参考以下格式：

|       字段名称        |  字段类型   |  字段说明  |   字段示例值    |
|:-----------------:|:-------:|:------:|:----------:|
|      adcode       | string  | 行政区划代码 |   310000   |
| formatted_address | string  | 行政区划名称 |    上海市     |
|        lng        | float64 |   经度   | 121.472644 |
|        lat        | float64 |   纬度   | 31.231706  |

制作完成后，可以将其保存为 CSV 文件，然后在配置文件中使用 `location_mapping` 字段指定其路径。

下面是一个示例的地理位置映射表：

```
adcode,formatted_address,lng,lat
310000,上海市,121.472644,31.231706
```

请注意：

1. 地理位置映射表的编码 **必须** 为 UTF-8，否则可能会出现乱码问题，如果您使用 `Microsoft Excel` 制作，请务必注意此项。
2. 如果需要使用原有的地理位置映射表，需要在彩云天气提供的默认映射表的基础上进行增加。
3. 自制的地理位置映射表中，行政区划代码 `adcode` 非必须字段，自制映射表时，可以不填写 `adcode` 字段。
4. 作者也不知道 彩云天气 提供的 api 是否能够支持国外的地理位置，如果不行的话欢迎提出 issue 或 discuss 以解决问题。

## 参考文档

- [彩云天气 - 生活质量指数映射](https://docs.caiyunapp.com/weather-api/v2/v2.6/tables/lifeindex.html)
- [彩云天气 - 气候现象映射](https://docs.caiyunapp.com/weather-api/v2/v2.6/tables/skycon.html)
- [彩云天气 - 实施天气数据](https://docs.caiyunapp.com/weather-api/v2/v2.6/1-realtime.html)
- [彩云天气 - 天气预警信息](https://docs.caiyunapp.com/weather-api/v2/v2.6/5-alert.html)