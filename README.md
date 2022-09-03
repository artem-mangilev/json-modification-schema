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

## Фильтрация

### Данные

```json
{
  "cars": [
    { "brand": "tesla", "model": "model s" },
    { "brand": "tesla", "model": "model x" },
    { "brand": "BMW", "model": "x5" },
  ]
}
```

### Модификатор

```json
{
  "modifications": [
    {
      "selector": "$.cars",
      "method": "filter",
      "arguments": [
        {
          "method": "equals",
          "arguments": [
            {
              "method": "select",
              "arguments": ["$item.brand"]
            },
            "tesla"
          ]
        }
      ]
    }
  ]
}
```

### Результат
```json
{
  "cars": [
    { "brand": "tesla", "model": "model s" },
    { "brand": "tesla", "model": "model x" }
  ]
}
```

## Интерполяция

### Данные

```json
{
  "numbers": [1, 2, 3, 4]
}
```

### Модификатор

```json
{
  "modifications" [
    {
      "selector": "$.numbers",
      "method": "template",
      "arguments": [
        {
          "one": "{{ one }}",
          "two": "{{ two }}",
          "three": "{{ three }}"
        },
        {
          "one": {
            "method": "select",
            "arguments": ["$.numbers[0]"]
          },
          "two": {
            "method": "select",
            "arguments": ["$.numbers[1]"]
          },
          "three": {
            "method": "select",
            "arguments": ["$.numbers[2]"]
          }
        }
      ]
    }
  ]
}
```

### Результат

```json
{
  "numbers": {
    "one": 1,
    "two": 2,
    "three": 3 
  }
}
```
