# Integrations表结构信息

## 概念

`Integrations` 表记录了所有集成的基本信息，包括集成ID、名称、密钥和创建更新时间。

## 相关接口

## 表结构

|     字段名      |     字段类型类型     |       默认值        |     说明     |       示例值       |
|:------------:|:--------------:|:----------------:|:----------:|:---------------:|
|     `id`     |    `bigint`    |  auto_increment  |    集成ID    |        1        |
|    `name`    | `varchar(255)` |       NULL       |    集成名称    | "Integration1"  |
|  `secret1`   | `varchar(255)` |       NULL       |    密钥1     | "secret1_value" |
|  `secret2`   | `varchar(255)` |       NULL       |    密钥2     | "secret2_value" |
|  `secret3`   | `varchar(255)` |       NULL       |    密钥3     | "secret3_value" |
|  `secret4`   | `varchar(255)` |       NULL       |    密钥4     | "secret4_value" |
| `created_at` |    `bigint`    | auto_create_time | 创建时间，毫秒时间戳 |  1721818960936  |
| `updated_at` |    `bigint`    | auto_update_time | 更新时间，毫秒时间戳 |  1721818960936  |

## 索引信息

|    索引名称    |  索引字段  | 是否唯一 |       索引用途        |
|:----------:|:------:|:----:|:-----------------:|
|    `id`    |  `id`  |  ✓   |        主键         |
| `idx_name` | `name` |  ✓   | 索引集成名称，使用集成名称检索集成 |

### 建表语句

```SQL
CREATE TABLE integrations (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    secret1 VARCHAR(255),
    secret2 VARCHAR(255),
    secret3 VARCHAR(255),
    secret4 VARCHAR(255),
    created_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint,
    updated_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint
);

CREATE UNIQUE INDEX idx_name ON integrations (name);
```

### ORM定义

```Go
type Integration struct {
	ID        int64  `gorm:"column:id;primaryKey;autoIncrement"`
	Name      string `gorm:"column:name;type:varchar(255);notnull;uniqueIndex:idx_name"`
	Secret1   string `gorm:"column:secret1;type:varchar(255)"`
	Secret2   string `gorm:"column:secret2;type:varchar(255)"`
	Secret3   string `gorm:"column:secret3;type:varchar(255)"`
	Secret4   string `gorm:"column:secret4;type:varchar(255)"`
	CreatedAt int64  `gorm:"column:created_at;autoCreateTime"`
	UpdatedAt int64  `gorm:"column:updated_at;autoUpdateTime"`
}
```