# Clients表结构信息

## 概念

`Clients` 表记录了所有客户端的基本信息，包括客户端ID、客户端名称、客户端密钥、创建时间和更新时间。

## 相关接口

## 表结构

|     字段名      |     字段类型类型     |       默认值        |     说明     |      示例值      |
|:------------:|:--------------:|:----------------:|:----------:|:-------------:|
|     `id`     |    `bigint`    |  auto_increment  |   客户端ID    |       1       |
|    `name`    | `varchar(255)` |       NULL       |   客户端名称    |   "ClientA"   |
|   `secret`   | `varchar(255)` |       NULL       |   客户端密钥    |  "s3cr3tK3y"  |
| `created_at` |    `bigint`    | auto_create_time | 创建时间，毫秒时间戳 | 1721818960936 |
| `updated_at` |    `bigint`    | auto_update_time | 更新时间，毫秒时间戳 | 1721818960936 |

## 索引信息

|     索引名称     |   索引字段   | 是否唯一 |         索引用途         |
|:------------:|:--------:|:----:|:--------------------:|
|     `id`     |   `id`   |  ✓   |          主键          |
|  `idx_name`  |  `name`  |  ✓   | 索引客户端名称，使用客户端名称检索客户端 | 
| `idx_secret` | `secret` |  ✓   | 索引客户端密钥，使用客户端密钥检索客户端 |

## 附加信息

### 建表语句

```SQL
CREATE TABLE clients (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    secret VARCHAR(255) NOT NULL,
    created_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint,
    updated_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint
);

CREATE UNIQUE INDEX idx_name ON Clients (name);
CREATE UNIQUE INDEX idx_secret ON Clients (secret);
```

### ORM定义

```Go
type Client struct {
	ID        int64  `gorm:"column:id;primaryKey;autoIncrement"`
	Name      string `gorm:"column:name;type:varchar(255);notnull;uniqueIndex:idx_name"`
	Secret    string `gorm:"column:secret;type:varchar(255);notnull;uniqueIndex:idx_secret"`
	CreatedAt int64  `gorm:"column:created_at;autoCreateTime"`
	UpdatedAt int64  `gorm:"column:updated_at;autoUpdateTime"`
}
```