# 虚空咏者配置项

> 此文档内容定期更新，可能与最新版本有一定出入，如果需要获取最新的配置列表，请转到 [akasha-whisper/config.example.yaml](https://github.com/alioth-center/akasha-whisper/blob/main/config/config.example.yaml) 

## 配置项详细信息

- `http_engine`: 这是 `akasha-whisper` 的网络服务器配置，决定系统将在什么路径，什么端口提供服务
  - `serve_url`: 服务路径，必须以 `/` 开头，不以 `/` 结尾
  - `serve_addr`: 服务地址，必须是 `ip:port` 的格式
  - `enable_management_apis`: 是否启用管理接口，如果设置为 `false` 或者不设置，将不会提供管理接口服务
- `logger`: 这是 `akasha-whisper` 的日志配置，决定系统将如何记录日志
  - `log_to_file`: 是否记录日志到文件，设置为 `false` 表示记录到标准输出
  - `log_split`: 是否按天切割日志文件，日志文件名将会是类似 `2021-01-01_akasha_whisper_logs_stdout.jsonl` 的格式
  - `log_directory`: 日志目录，必须在启动前创建，否则会导致程序崩溃
  - `log_level`: 输出日志级别，可选值为 `debug`, `info`, `warn`, `error`, `panic`, `fatal`
  - `log_file_suffix`: 日志文件后缀，必须以 `_` 开头，以扩展名结尾，默认是 `_akasha_whisper_logs_stdout.jsonl`
- `bloom_filter`: 这是 `akasha-whisper` 的布隆过滤器配置，决定系统是否启用布隆过滤器，布隆过滤器将用于拦截、判断请求的 API 密钥是否存在
  - `enable`: 是否启用布隆过滤器
  - `filter_size`: 布隆过滤器大小
  - `false_rate`: 布隆过滤器误判率，误判率越低，内存占用越大
- `database`: 这是 `akasha-whisper` 的数据库配置，设置数据库类型和各类机密信息
  - `driver`: 数据库驱动，可选值为 `mysql(>= 5.7)`, `postgres(>= 9.6)`, `sqlite(>= 3.9)`
  - `host`: 数据库地址，例如：`db.yourdomain.com`, `192.168.1.1`, `./data/sqlite.db (仅适用于 sqlite)`
  - `port`: 数据库端口，例如：`3306`, `5432`, `0 (仅适用于 sqlite)`
  - `username`: 数据库用户名，例如：`root`, `postgres`, `空 (仅适用于 sqlite)`
  - `password`: 数据库密码，例如：`123456`, `空 (仅适用于 sqlite)`
  - `database`: 数据库名称，例如：`test`, `空 (仅适用于 sqlite)`
  - `location`: 数据库时区，必须是一个有效的时区，如果使用 docker，请设置为 `UTC`
- `app`: 这是 `akasha-whisper` 的应用配置
  - `max_token`: 全局最大 token 数，必须大于 0
  - `management_token`: 管理 token，必须设置，空表示禁用管理接口
  - `price_token_unit`: 价格 token 单位，必须大于 0，例如：如果 $5 = 1M tokens，你的 price_token_unit = 1000000，然后 prompt_price 或 completion_price = 5
  - `login_token_key`: 管理接口登录使用的 token 的 cookie name，必须设置，空表示禁用 cookie 登录

## 完整配置示例

这是 `v0.0.2` 版本的 `akasha-whisper` 的完整配置示例：

```yaml
http_engine:
  serve_url: '/v1' # serve url, must start with '/' and end without '/'
  serve_addr: '0.0.0.0:10000' # serve address, must be ip:port
  enable_management_apis: true # enable management apis, if set to false or unset, management apis will not serve
logger:
  log_to_file: true # log to file, set to false means log to stdout
  log_split: true # split log file by day, log file name will be like '2021-01-01_akasha_whisper_logs_stdout.jsonl'
  log_directory: './log' # log directory, must be created before starting, or it will panic the program
  log_level: 'info' # enum: debug, info , warn, error, panic, fatal
  log_file_suffix: '_stdout.jsonl' # log file suffix, must start with '_' and end with extension, default is '_akasha_whisper_logs_stdout.jsonl'
bloom_filter:
  enable: true # enable bloom filter
  filter_size: 1000000 # bloom filter size
  false_rate: 0.0001 # false positive rate
database:
  driver: 'mysql' # enum: mysql(>= 5.7), postgres(>= 9.6), sqlite(>= 3.9)
  host: '127.0.0.1' # example: db.yourdomain.com, 192.168.1.1, ./data/sqlite.db(only for sqlite)
  port: 3306 # example: 3306, 5432, 0(only for sqlite)
  username: 'your_username' # example: root, postgres (empty only for sqlite)
  password: 'your_password' # example: 123456, empty(only for sqlite)
  database: 'your_database' # example: test, empty(only for sqlite)s
  location: 'UTC' # database location, must be a valid location, default is Asia/Shanghai, if you use docker, set it to UTC
app:
  max_token: 128000 # global max token, must be greater than 0
  management_token: 'your_management_token' # management token, must be set, empty means disable management apis
  price_token_unit: 1000 # price token unit, must be greater than 0, means if $5 = 1M tokens, your price_token_unit = 1000000, and prompt_price or completion_price = 5
  login_token_key: 'akasha_whisper_login_token' # login token key, must be set, empty means disable cookie login
```