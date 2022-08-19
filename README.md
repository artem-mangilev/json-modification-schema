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

## Удаление полей

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

## Создание вложенности

### Данные

```json
{
  "data": "hello world"
}
```

### Модификатор

```json
{
  "modifications": [
    {
      "selector": "$.data",
      "method": "rename",
      "arguments": ["newData"]
    },
    {
      "selector": "$",
      "method": "wrapWithObject",
      "arguments": ["wrapper"]
    }
  ]
}
```

### Результат

```json
{
  "wrapper": {
    "newData": "hello world"
  }
}
```

## Условие

### Данные

```json
{
  "firstPerson": "Andrew",
  "secondPerson": "Andrew"
}
```

### Модификатор

```json
{
  "modifications": [
    {
      "method": "if",
      "arguments": [
        {
          "method": "equals",
          "arguments": [
            {
              "method": "select",
              "arguments": ["$.firstPerson"]
            },
            {
              "method": "select",
              "arguments": ["$.secondPerson"]
            }, 
          ]
        },
        {
          "selector": "$"
          "method": "addProp",
          "arguments": ["isSamePerson", true]
        },
        {
          "selector": "$"
          "method": "addProp",
          "arguments": ["isSamePerson", false]
        },
      ]
    }
  ]
}
```

### Результат

```json
{
  "firstPerson": "Andrew",
  "secondPerson": "Andrew",
  "isSamePerson": true
}
```

