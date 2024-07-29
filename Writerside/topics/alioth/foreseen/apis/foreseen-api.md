# 远焦接口文档

## 接口信息

|   接口名    | 接口协议 |            接口说明             |             对应文档              |
|:--------:|:----:|:---------------------------:|:-----------------------------:|
| 获取远焦用户信息 | HTTP |     用于获取远焦系统的用户信息和账户信息      |   [](foreseen-api-users.md)   |
| 创建远焦接收用户 | HTTP |  根据给定的用户信息和账户信息创建远焦接收用户和账户  |   [](foreseen-api-users.md)   |
| 更新远焦用户信息 | HTTP | 根据给定的用户名称和更新操作列表更新远焦接收用户和账户 |   [](foreseen-api-users.md)   |
| 获取远焦信息模板 | HTTP |       用于获取远焦系统的信息模板信息       | [](foreseen-api-templates.md) |
| 创建远焦信息模板 | HTTP |      根据给定的模板信息创建远焦信息模板      | [](foreseen-api-templates.md) |
| 更新远焦信息模板 | HTTP |  根据给定的模板名称和更新操作列表更新远焦信息模板   | [](foreseen-api-templates.md) |
| 预览远焦信息内容 | HTTP |    根据给定的模板名称和参数预览远焦信息内容     | [](foreseen-api-templates.md) |

## SDK封装

我们提供了 Golang SDK 以在程序中使用远焦系统，使用以下指令安装/更新最新的远焦系统 SDK：

```Shell
go get -u github.com/alioth-center/foreseen/sdk@latest
```

随后即可在程序中使用下面的示例代码使用远焦客户端：

```Go
package main

import (
    "os"
    "context"

    "github.com/alioth-center/foreseen/sdk"
)

func main() {
    client := sdk.NewClient(os.Getenv("FORESEEN_TOKEN"))
    response, err := client.GetUserAndAccount(context.Background(), "${username}")
    if err != nil {
        panic(err)
    }
    
    // ...
}
```