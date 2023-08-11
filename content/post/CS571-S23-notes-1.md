---
title: "CS571-S23 学习笔记(一) : JSON"
date: 2023-08-09T00:00:01+08:00
description: "CS571 S23 旨在向计算机科学本科生介绍用户体验 (UX) 开发的精彩世界，这是我在这门课的学习笔记。本节关键词：JSON"
categories: [前端学习]
tags: [JavaScript, CS571, 前端, JSON]
draft: false
---

## 引言

> CS571 S23 旨在向计算机科学本科生介绍用户体验 (UX) 开发的精彩世界，这是我在这门课的学习笔记。本节关键词：JSON

<!--more-->

## JSON

JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，常用于数据的传输和存储。它采用键值对的方式表示数据，并且具有简洁、易于理解和生成的特点。以下是 JSON 格式的一些基本规则和示例：

1. **数据类型**：JSON 支持以下数据类型：

   - 字符串（用双引号括起来）："Hello World"
   - 数字：42
   - 布尔值：true 或 false
   - 数组（用方括号括起来）：[1, 2, 3]
   - 对象（用花括号括起来）：{"name": "John", "age": 30}

2. **键值对**：JSON 中的对象由一组键值对组成，键和值之间使用冒号分隔，每个键值对之间使用逗号分隔，对象用花括号括起来。键必须是字符串，值可以是任意数据类型。示例：

```json
{
  "name": "John",
  "age": 30,
  "isStudent": false
}
```

3. **数组**：JSON 中的数组由一组值组成，值之间使用逗号分隔，数组用方括号括起来。示例：

```json
[1, 2, 3, 4, 5]
```

4. **嵌套**：JSON 支持嵌套，可以在对象中包含对象或数组，也可以在数组中包含对象或其他数组。示例：

```json
{
  "name": "John",
  "age": 30,
  "hobbies": ["reading", "traveling"],
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```

注意，JSON 中的键和字符串必须使用双引号括起来，而不是单引号。

JSON 是一种通用的数据格式，在多种编程语言中都有对应的解析和生成 JSON 的库和函数。在 JavaScript 中，可以使用 `JSON.parse()` 将 JSON 字符串解析为 JavaScript 对象，使用 `JSON.stringify()` 将 JavaScript 对象转换为 JSON 字符串。

```javascript
const jsonString = '{"name": "John", "age": 30, "isStudent": false}';
const jsonObject = JSON.parse(jsonString);
console.log(jsonObject.name); // 输出: "John"

const person = { name: "John", age: 30, isStudent: false };
const jsonString2 = JSON.stringify(person);
console.log(jsonString2); // 输出: '{"name":"John","age":30,"isStudent":false}'
```

以上是 JSON 格式的基本规则和用法，它提供了一种方便可读的数据交换格式，广泛应用于前后端数据传输和存储。
