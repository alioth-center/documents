# Templates表结构信息

## 概念

`Templates` 表记录了所有模板的信息，包括模板ID、模板名称、模板内容、模板参数、创建时间和更新时间。

## 相关接口

## 表结构

|     字段名      |      字段类型      |       默认值        |         说明          |                        示例值                        |
|:------------:|:--------------:|:----------------:|:-------------------:|:-------------------------------------------------:|
|     `id`     |    `bigint`    |  auto_increment  |        模板ID         |                         1                         |
|    `name`    | `varchar(255)` |       NULL       |        模板名称         |                    "Template1"                    |
|  `content`   |     `text`     |       NULL       |        模板内容         |           "${project} now is ${status}"           |
| `arguments`  |     `text`     |       NULL       | 模板参数，JSON 格式，用于格式校验 | `{"project":"project_name", "status": "offline"}` |
| `created_at` |    `bigint`    | auto_create_time |     创建时间，毫秒时间戳      |                   1721818960936                   |
| `updated_at` |    `bigint`    | auto_update_time |     更新时间，毫秒时间戳      |                   1721818960936                   |

## 索引信息

|    索引名称    |  索引字段  | 是否唯一 |       索引用途        |
|:----------:|:------:|:----:|:-----------------:|
|    `id`    |  `id`  |  ✓   |        主键         |
| `idx_name` | `name` |  ✓   | 索引模板名称，使用模板名称检索模板 | 

## 附加信息

### 建表语句

```SQL
CREATE TABLE templates (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    arguments TEXT NOT NULL,
    created_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint,
    updated_at BIGINT NOT NULL DEFAULT (extract(epoch from now()) * 1000)::bigint
);

CREATE UNIQUE INDEX idx_name ON templates (name);
```

### ORM定义

```Go
package model

type Template struct {
	ID        int64  `gorm:"column:id;primaryKey;autoIncrement"`
	Name      string `gorm:"column:name;type:varchar(255);notnull;uniqueIndex:idx_name"`
	Content   string `gorm:"column:content;type:text;notnull"`
	Arguments string `gorm:"column:arguments;type:text;notnull"`
	CreatedAt int64  `gorm:"column:created_at;autoCreateTime"`
	UpdatedAt int64  `gorm:"column:updated_at;autoUpdateTime"`
}
```