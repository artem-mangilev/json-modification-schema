# json-modification-schema

Цели для создания схемы:

1. Простая сериализация/десериализация
2. Читабельность
3. Модификация вместо создания

## Переименование

### Данные

```json
{
  "array": [1]
}
```

### Модификатор

```json
{
  "modifications": [
    {
      "selector": "$.arr",
      "method": "rename",
      "arguments": ["newName"]
    }
  ]
}
```

### Результат

```json
{
  "newName": [1]
}
```

### Удаление полей

### Данные

```json
{
  "firstName": "Artem",
  "lastName": "Mangilev"
}
```

### Модификатор

```json
{
  "modifications": [
    {
      "selector": "$.lastName",
      "method": "delete"
    }
  ]
}
```

### Результат

```json
{
  "firstName": "Artem"
}
```
