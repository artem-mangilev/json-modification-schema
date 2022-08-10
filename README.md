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
      "method": "rename('newName')"
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

## Условие

### Данные

```json
{
  "firstName": "Artem",
}
```

### Модификатор

```json
{
  "modifications": [
    {
      "selector": "$.firstName",
      "method": "if",
      "true": [
        {
          "selector": "$",
          "method": "addProp('lastName', 'Mangilev')"
        }
      ],
      "false": [
        {
          "selector": "$.firstName",
          "method": "delete"
        }
      ]
    }
  ]
}
```

### Результат

```json
{
  "firstName": "Artem",
  "lastName": "Mangilev"
}
```
