# Accounts表结构信息

## 概念

`Accounts` 表记录了所有账户的基本信息，包括账户ID、集成ID、账号、创建时间和更新时间。

## 相关接口

## 表结构

|      字段名      |      字段类型      |       默认值        |       说明       |        示例值         |
|:-------------:|:--------------:|:----------------:|:--------------:|:------------------:|
|     `id`      |    `bigint`    |  auto_increment  |      账户ID      |         1          |
| `integration` |    `bigint`    |       NULL       | 集成ID，表示账户所属的集成 |        1001        |
|   `account`   | `varchar(255)` |       NULL       |       账号       | "user@example.com" |
| `created_at`  |    `bigint`    | auto_create_time |   创建时间，毫秒时间戳   |   1721818960936    |
| `updated_at`  |    `bigint`    | auto_update_time |   更新时间，毫秒时间戳   |   1721818960936    |

## 索引信息

|       索引名称        |        索引字段         | 是否唯一 |       索引用途        |
|:-----------------:|:-------------------:|:----:|:-----------------:|
|       `id`        |        `id`         |  ✓   |        主键         |
| `idx_account_ids` | `id`, `integration` |  ✓   | 索引账户ID和集成ID，确保唯一性 | 
| `idx_integration` |    `integration`    |      | 索引集成ID，使用集成ID检索账户 |
|   `idx_account`   |      `account`      |      |   索引账号，使用账号检索账户   |

## 附加信息

### 建表语句

```SQL
CREATE TABLE accounts (
    id BIGSERIAL PRIMARY KEY,
    integration BIGINT NOT NULL,
    account VARCHAR(255) NOT NULL,
    created_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint,
    updated_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint
);

CREATE UNIQUE INDEX idx_account_ids ON accounts (id, integration);
CREATE INDEX idx_integration ON accounts (integration);
CREATE INDEX idx_account ON accounts (account);
```

### ORM定义

```Go
package model

type Account struct {
	ID          int64  `gorm:"column:id;primaryKey;autoIncrement;uniqueIndex:idx_account_ids"`
	Integration int64  `gorm:"column:integration;type:integer;notnull;index:idx_integration;uniqueIndex:idx_account_ids"`
	Account     string `gorm:"column:account;type:varchar(255);notnull;index:idx_account"`
	CreatedAt   int64  `gorm:"column:created_at;autoCreateTime"`
	UpdatedAt   int64  `gorm:"column:updated_at;autoUpdateTime"`
}
```