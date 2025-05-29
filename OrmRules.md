# GORM Tag Rules

## 1. ID Field Guidelines

### 1.1 ID Types

ID fields typically come in two types:

1. **string** type IDs:

   - Use for primary entities that may be referenced as foreign keys by other tables
   - Examples: `user`, `group`, `policy`
   - Always set `size:64` to limit string length
   - Use `primaryKey` tag but not `autoIncrement`

2. **int64** type IDs:
   - Use for secondary entities that are unlikely to be referenced elsewhere
   - Examples: `user_email`, `group_sync_records`
   - Use `primaryKey` with `autoIncrement` tags

### 1.2 Foreign Key Fields

- Always use the same type as the primary key they reference
- For string foreign keys, use `size:64` constraint
- Example: If `user.id` is string, then `user_group.user_id` should also be string

## 2. General Rules

- Tags are **case insensitive**, however **camelCase is preferred**
- Multiple tags should be separated by a semicolon `;`
- Characters with special meaning can be escaped with a backslash `\`

## 3. Standard Field Configuration

- Primary keys: Use `primaryKey` tag
- Normal fields: Use `->;<-` for standard read/write fields
- Auto-incrementing fields: Use `autoIncrement` with primary keys
- Read-only fields: Use `->`
- Write-only fields: Use `<-`

## 4. Available Tags

| Tag Name                 | Description                            | Example                                                                                    |
| ------------------------ | -------------------------------------- | ------------------------------------------------------------------------------------------ |
| `column`                 | Specifies db column name               | `"column:user_name"`                                                                       |
| `type`                   | Column data type                       | `"type:text"`, `"type:varchar(255)"`                                                       |
| `serializer`             | Specifies serializer                   | `"serializer:json"`, `"serializer:gob"`                                                    |
| `size`                   | Defines column's size/length           | `"size:256"`                                                                               |
| `primaryKey`             | Defines a column as primary key        | `"primaryKey"`                                                                             |
| `unique`                 | Defines a column as unique key         | `"unique"`                                                                                 |
| `default`                | Defines column's default value         | `"default:'user'"`                                                                         |
| `precision`              | Specifies column's precision           | `"precision:2"`                                                                            |
| `scale`                  | Specifies column's scale               | `"scale:2"`                                                                                |
| `not null`               | Specifies column as NOT NULL           | `"not null"`                                                                               |
| `autoIncrement`          | Specifies column as auto-incrementing  | `"autoIncrement"`                                                                          |
| `autoIncrementIncrement` | Auto step increment                    | `"autoIncrementIncrement:5"`                                                               |
| `embedded`               | Specifies embedded fields              | `"embedded"`                                                                               |
| `embeddedPrefix`         | Prefix for embedded field column names | `"embeddedPrefix:user_"`                                                                   |
| `autoCreateTime`         | Track creation time                    | `"autoCreateTime"`, `"autoCreateTime:nano"`                                                |
| `autoUpdateTime`         | Track update time                      | `"autoUpdateTime"`, `"autoUpdateTime:milli"`                                               |
| `index`                  | Creates an index                       | `"index"`, `"index:idx_name"`                                                              |
| `uniqueIndex`            | Creates a unique index                 | `"uniqueIndex"`, `"uniqueIndex:idx_name"`                                                  |
| `check`                  | Create check constraints               | `"check:age > 13"`                                                                         |
| `<-`                     | Write permission                       | `"<-:create"` (create only), `"<-:update"` (update only), `"<-:false"` (no write)          |
| `->`                     | Read permission                        | `"->:false"` (no read)                                                                     |
| `-`                      | Ignore field                           | `"-"` (no read/write), `"-:migration"` (no migration), `"-:all"` (no read/write/migration) |
| `comment`                | Add comment during migration           | `"comment:'User ID'"`                                                                      |

## 5. String Field Size Guidelines

- **ID fields (primary or foreign keys)**: `size:64`
- **Normal string fields**: Use appropriate size constraints like `size:255` for names, emails, etc.
- **Large text content**: Use `type:text` instead of size
- **URL fields**: Use `type:text` for URL fields to accommodate potentially long URLs

## 6. Read/Write Permission Tags

- `->;<-`: Both read and write permission (default)
- `->`: Read-only permission
- `<-`: Write-only permission
- `<-:create`: Write permission only on create
- `<-:update`: Write permission only on update
- `<-:false`: No write permission
- `->:false`: No read permission
- `-`: No read or write permission

## 7. Common Patterns

### 7.1 String Primary Key

```proto
string id = 1 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "primaryKey;->;<-:create;size:64"
}];
```

### 7.2 Integer Primary Key

```proto
int64 id = 1 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "primaryKey;->;<-:create;autoIncrement"
}];
```

### 7.3 Handle

```proto
string handle = 2 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-;unique;size:255"
}];
```

### 7.4 Foreign Key Reference

```proto
string user_id = 2 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-;not null;size:64"
}];
```

### 7.5 Auto Timestamps

```proto
int64 create_ts = 8 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-:create;autoCreateTime"
}];

int64 update_ts = 9 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-;autoUpdateTime"
}];
```

### 7.6 Required Field (Not Null)

```proto
string name = 2 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-;not null;size:255"
}];
```

### 7.7 Unique Field

```proto
string email = 3 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-;unique;size:255"
}];
```

### 7.8 Text Field

```proto
string description = 5 [(protoc_gen_gorm.options.v1.field_options) = {
  tag: "->;<-;type:text"
}];
```

## 8. Best Practices

1. Use `string` type (with `size:64`) for IDs of primary entities that may be referenced
2. Use `int64` type with `autoIncrement` for IDs of secondary entities
3. Match foreign key types with their referenced primary key types
4. Add appropriate `size` constraints to all string fields
5. Mark `id` fields with `primaryKey`
6. Use `autoCreateTime` and `autoUpdateTime` for `create_ts` and `update_ts` fields
7. Use `not null` for required fields
8. Be specific about read/write permissions with `->` and `<-`
9. Use `type:text` for fields that may contain large amounts of text
10. Add `unique` constraint to fields that must be unique
11. Use `default` to provide default values for fields when appropriate
