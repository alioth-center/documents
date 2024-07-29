# 远焦数据库文档

## 数据表

|      表名      |     描述     |         说明          |                 详细文档                  |
|:------------:|:----------:|:-------------------:|:-------------------------------------:|
|    users     |   用户信息表    |     用于存储用户的基础信息     |    [](foreseen-database-users.md)     |
|   clients    |   客户端信息表   | 用于存储调用客户端的信息，包含密钥信息 |   [](foreseen-database-clients.md)    |
| integrations |   集成密钥表    |  用于存储第三方集成通知的密钥信息   | [](foreseen-database-integrations.md) |
|   accounts   | 用户被通知账户信息表 |  用于存储用户在第三方集成的账号信息  |   [](foreseen-database-accounts.md)   |
|    tasks     |   任务信息表    |    用于存储通知任务的执行信息    |    [](foreseen-database-tasks.md)     |
|  templates   |   模板信息表    |     用于存储通知模板的信息     |  [](foreseen-database-templates.md)   |

## 实体关系

这些数据表的简单实体关系图如下：

```mermaid
erDiagram
    ACCOUNT {
        int64 ID
        int64 Integration
        int64 UserID
        string Account
        int64 CreatedAt
        int64 UpdatedAt
    }
    CLIENT {
        int64 ID
        string Name
        string Secret
        int64 CreatedAt
        int64 UpdatedAt
    }
    INTEGRATION {
        int64 ID
        string Name
        string Secret1
        string Secret2
        string Secret3
        string Secret4
        int64 CreatedAt
        int64 UpdatedAt
    }
    TASK {
        int64 ID
        int64 IntegrationID
        int64 UserID
        int64 ClientID
        int64 AccountID
        int64 TemplateID
        string Argument
        int32 Status
        int64 CreatedAt
        int64 UpdatedAt
    }
    TEMPLATE {
        int64 ID
        string Name
        string Content
        string Arguments
        int64 CreatedAt
        int64 UpdatedAt
    }
    USER {
        int64 ID
        int32 Type
        string Name
        int64 CreatedAt
        int64 UpdatedAt
    }

    TASK }o--|| TEMPLATE : "TemplateID"
    USER ||--o{ ACCOUNT : "UserID"
    INTEGRATION ||--o{ ACCOUNT : "Integration"
    INTEGRATION ||--o{ TASK : "IntegrationID"
    USER ||--o{ TASK : "UserID"
    CLIENT ||--o{ TASK : "ClientID"
    ACCOUNT ||--o{ TASK : "AccountID"
```