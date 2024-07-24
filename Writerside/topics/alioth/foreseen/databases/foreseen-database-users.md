# Users表结构信息

## 概念

`Users` 表记录了所有用户的基本信息，包括用户ID、用户类型、用户名、创建时间和更新时间。

## 相关接口

## 表结构

|     字段名      |     字段类型类型     |       默认值        |     说明     |      示例值      |
|:------------:|:--------------:|:----------------:|:----------:|:-------------:|
|     `id`     |    `bigint`    |  auto_increment  |    用户ID    |       1       |
|    `type`    |   `integer`    |        0         | 用户类型，见附加信息 |       0       |
|    `name`    | `varchar(255)` |       NULL       |    用户名     |    "Alice"    |
| `created_at` |    `bigint`    | auto_create_time | 创建时间，毫秒时间戳 | 1721818960936 |
| `updated_at` |    `bigint`    | auto_update_time | 更新时间，毫秒时间戳 | 1721818960936 |

## 索引信息

|    索引名称    |  索引字段  | 是否唯一 |       索引用途        |
|:----------:|:------:|:----:|:-----------------:|
|    `id`    |  `id`  |  ✓   |        主键         |
| `idx_name` | `name` |  ✓   |  索引用户名，使用用户名检索用户  | 
| `idx_type` | `type` |      | 索引用户类型，使用用户类型检索用户 |

## 附加信息

### 用户类型枚举

| 类型 |        说明         |
|:--:|:-----------------:|
| 0  |  个人用户，表示一个自然人用户   |
| 1  | 群组用户，表示多个自然人用户的集合 |
| 2  |   系统用户，表示一个系统集成   |

### 建表语句

```SQL
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    type INTEGER NOT NULL DEFAULT 0,
    name VARCHAR(255) NOT NULL,
    created_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint,
    updated_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint
);

CREATE UNIQUE INDEX idx_name ON Users (name);
CREATE INDEX idx_type ON Users (type);
```

### ORM定义

```Go
type User struct {
	ID        int64  `gorm:"column:id;primaryKey;autoIncrement"`
	Type      int32  `gorm:"column:type;type:integer;notnull;default:0;index:idx_type"`
	Name      string `gorm:"column:name;type:varchar(255);notnull;uniqueIndex:idx_name"`
	CreatedAt int64  `gorm:"column:created_at;autoCreateTime"`
	UpdatedAt int64  `gorm:"column:updated_at;autoUpdateTime"`
}
```