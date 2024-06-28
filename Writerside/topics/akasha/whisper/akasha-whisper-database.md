# 虚空咏者数据表设计

## 概述

虚空咏者使用 `postgres` 作为数据库，需要使用到的表格如下：

## openai_clients

`openai_clients` 表主要记录的是虚空咏者系统所能使用的各类 openai 兼容 api 的服务端点机密信息。

|    字段名     |     类型      |   说明   |            示例            |
|:----------:|:-----------:|:------:|:------------------------:|
|     id     |   int(11)   |   主键   |            1             |
|    desc    | varchar(64) | 服务端点描述 |         "openai"         |
|  api_key   | varchar(64) | 服务端点密钥 |         "sk_xxx"         |
|  endpoint  | varchar(64) | 服务端点地址 | "https://api.openai.com" |
|   weight   |   int(11)   | 服务端点权重 |           100            |
| created_at |  timestamp  |  创建时间  |   2022-01-01 00:00:00    |
| updated_at |  timestamp  |  更新时间  |   2022-01-01 00:00:00    |

索引：

- `desc` 作为索引，用作描述查询
- `api_key` 作为索引，用作密钥查询
- `endpoint` 作为索引，用作地址查询
- `weight` 作为索引，用作权重查询
- `created_at` 作为索引，用作时序查询
- `updated_at` 作为索引，用作更新查询

## openai_models

`openai_models` 表主要记录的是虚空咏者系统所能使用的各类 openai 兼容 api 的模型信息与定价信息。

|       字段名        |     类型      |          说明           |         示例          |
|:----------------:|:-----------:|:---------------------:|:-------------------:|
|        id        |   int(11)   |          主键           |          1          |
|    client_id     |   int(11)   |     可以使用的服务端点 id      |          1          |
|      model       | varchar(32) |         模型名称          |      "gpt-4o"       |
|    max_tokens    |   int(11)   |      最大 token 数       |        2048         |
|   prompt_price   |  float(11)  |   prompt 的 token 价格   |        0.035        |
| completion_price |  float(11)  | completion 的 token 价格 |         0.1         |
|    rpm_limit     |   int(11)   |        每分钟请求限制        |         100         |
|    tpm_limit     |   int(11)   |     每分钟 token 限制      |         100         |
|    created_at    |  timestamp  |         创建时间          | 2022-01-01 00:00:00 |
|    updated_at    |  timestamp  |         更新时间          | 2022-01-01 00:00:00 |

索引：

- `client_id` 作为索引，对应 `openai_clients` 表
- `created_at` 作为索引，用作时序查询
- `rpm_limit`, `tpm_limit` 作为索引，用作限制查询
- `prompt_price`, `completion_price` 作为索引，用作价格查询
- `model` 作为索引，用作模型查询
- `max_tokens` 作为索引，用作 token 数查询
- `updated_at` 作为索引，用作更新查询

## openai_requests

`openai_requests` 表主要记录的是虚空咏者系统所能使用的各类 openai 兼容 api 的请求信息，包括请求内容和响应内容。

|          字段名           |      类型      |           说明           |         示例          |
|:----------------------:|:------------:|:----------------------:|:-------------------:|
|           id           |   int(11)    |           主键           |          1          |
|       client_id        |   int(11)    |      请求使用的服务端点 id      |          1          |
|        model_id        |   int(11)    |       请求使用的模型 id       |          1          |
|   prompt_token_usage   |   int(11)    |   prompt 使用的 token 数   |         10          |
| completion_token_usage |   int(11)    | completion 使用的 token 数 |         20          |
|      balance_cost      |  float(11)   |        本次请求的花费         |         3.5         |
|       created_at       |  timestamp   |          创建时间          | 2022-01-01 00:00:00 |

索引：

- `client_id`, `model_id` 作为索引，对应 `openai_clients`, `openai_models` 表
- `created_at` 作为索引，用作时序查询
- `balance_cost` 作为索引，用作花费查询

## whisper_users

`whisper_users` 表主要记录的是虚空咏者系统的用户信息。

|    字段名     |      类型       |     说明      |         示例          |
|:----------:|:-------------:|:-----------:|:-------------------:|
|     id     |    int(11)    |     主键      |          1          |
|   email    |  varchar(64)  |  (认证系统的)邮箱  | "user@example.com"  |
|  api_key   |  varchar(64)  |  用户 api 密钥  |      "sk_xxx"       |
|  balance   | decimal(11,6) |    用户余额     |         100         |
|    role    |  varchar(10)  |    用户角色     |   "user"/"system"   |
|  allow_ip  |  varchar(64)  | 允许访问的 ip 地址 |    "192.168.1.1"    |
| created_at |   timestamp   |    创建时间     | 2022-01-01 00:00:00 |
| updated_at |   timestamp   |    更新时间     | 2022-01-01 00:00:00 |

索引：

- `email` 作为索引，用作用户登录
- `api_key` 作为索引，用作用户认证
- `role` 作为索引，用作用户权限管理
- `created_at` 作为索引，用作时序查询

## whisper_user_permissions

`whisper_user_permissions` 表主要记录的是虚空咏者系统的用户权限信息。

|    字段名     |    类型     |       说明       |         示例          |
|:----------:|:---------:|:--------------:|:-------------------:|
|     id     |  int(11)  |       主键       |          1          |
|  user_id   |  int(11)  |     用户 id      |          1          |
| client_id  |  int(11)  | 用户可以使用的服务端点 id |          1          |
|  model_id  |  int(11)  |  用户可以使用的模型 id  |          1          |
| created_at | timestamp |      创建时间      | 2022-01-01 00:00:00 |

索引：

- `client_id`, `model_id` 作为索引，对应 `openai_clients`, `openai_models` 表
- `user_id` 作为索引，对应 `whisper_users` 表
- `created_at` 作为索引，用作时序查询
