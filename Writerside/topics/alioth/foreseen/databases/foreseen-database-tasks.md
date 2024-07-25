# Tasks表结构信息

## 概念

`Tasks` 表记录了任务相关的信息，包括任务ID、集成ID、用户ID、客户端ID、账户ID、模板ID、参数、状态、创建时间和更新时间。

## 相关接口

## 表结构

|       字段名        |   字段类型    |       默认值        |       说明       |                        示例值                        |
|:----------------:|:---------:|:----------------:|:--------------:|:-------------------------------------------------:|
|       `id`       | `bigint`  |  auto_increment  |      任务ID      |                         1                         |
| `integration_id` | `bigint`  |       NULL       |      集成ID      |                         1                         |
|    `user_id`     | `bigint`  |       NULL       |      用户ID      |                         1                         |
|   `client_id`    | `bigint`  |       NULL       |     客户端ID      |                         1                         |
|   `account_id`   | `bigint`  |       NULL       |      账户ID      |                         1                         |
|  `template_id`   | `bigint`  |       NULL       |      模板ID      |                         1                         |
|    `argument`    |  `text`   |       NULL       |       参数       | `{"project":"project_name", "status": "offline"}` |
|     `status`     | `integer` |        0         |   任务状态，见附加信息   |                         0                         |
|   `created_at`   | `bigint`  | auto_create_time |   创建时间，毫秒时间戳   |                   1721818960936                   |
|   `updated_at`   | `bigint`  | auto_update_time |   更新时间，毫秒时间戳   |                   1721818960936                   |

## 索引信息

|         索引名称         |                     索引字段                     | 是否唯一 |        索引用途         |
|:--------------------:|:--------------------------------------------:|:----:|:-------------------:|
|         `id`         |                     `id`                     |  ✓   |         主键          |
| `idx_integration_id` |               `integration_id`               |      |  索引集成ID，使用集成ID检索任务  | 
|    `idx_user_id`     |                  `user_id`                   |      |  索引用户ID，使用用户ID检索任务  | 
|   `idx_client_id`    |                 `client_id`                  |      | 索引客户端ID，使用客户端ID检索任务 | 
|   `idx_account_id`   |                 `account_id`                 |      |  索引账户ID，使用账户ID检索任务  | 
|  `idx_template_id`   |                `template_id`                 |      |  索引模板ID，使用模板ID检索任务  | 
|     `idx_status`     |                   `status`                   |      |  索引任务状态，使用任务状态检索任务  | 
|      `idx_task`      | `integration_id`, `client_id`, `template_id` |      |    索引任务信息，定位一批任务    | 

## 附加信息

### 任务状态枚举

| 类型 | 说明  |
|:--:|:---:|
| 0  | 待处理 |
| 1  | 处理中 |
| 2  | 已完成 |
| 3  | 已失败 |

### 建表语句

```SQL
CREATE TABLE tasks (
    id BIGSERIAL PRIMARY KEY,
    integration_id BIGINT NOT NULL,
    user_id BIGINT NOT NULL,
    client_id BIGINT NOT NULL,
    account_id BIGINT NOT NULL,
    template_id BIGINT NOT NULL,
    argument TEXT NOT NULL,
    status INTEGER NOT NULL DEFAULT 0,
    created_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint,
    updated_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint
);

CREATE INDEX idx_integration_id ON tasks (integration_id);
CREATE INDEX idx_user_id ON tasks (user_id);
CREATE INDEX idx_client_id ON tasks (client_id);
CREATE INDEX idx_account_id ON tasks (account_id);
CREATE INDEX idx_template_id ON tasks (template_id);
CREATE INDEX idx_status ON tasks (status);
```

### ORM定义

```Go
type Task struct {
	ID            int64  `gorm:"column:id;primaryKey;autoIncrement"`
	IntegrationID int64  `gorm:"column:integration_id;type:bigint;notnull;index:idx_integration_id"`
	UserID        int64  `gorm:"column:user_id;type:bigint;notnull;index:idx_user_id"`
	ClientID      int64  `gorm:"column:client_id;type:bigint;notnull;index:idx_client_id"`
	AccountID     int64  `gorm:"column:account_id;type:bigint;notnull;index:idx_account_id"`
	TemplateID    int64  `gorm:"column:template_id;type:bigint;notnull;index:idx_template_id"`
	Argument      string `gorm:"column:argument;type:text;notnull"`
	Status        int32  `gorm:"column:status;type:integer;notnull;default:0;index:idx_status"`
	CreatedAt     int64  `gorm:"column:created_at;autoCreateTime"`
	UpdatedAt     int64  `gorm:"column:updated_at;autoUpdateTime"`
}
```