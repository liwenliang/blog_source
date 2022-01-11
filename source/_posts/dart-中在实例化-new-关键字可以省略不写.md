---
title: dart 中在实例化 new 关键字可以省略不写
tags:
  - 知识点备忘
  - Flutter
originContent: ''
categories:
  - 移动开发
toc: false
date: 2022-01-03 08:59:14
---

dart 中在实例化 new 关键字可以省略不写

```dart
class Person {
  String name;
  int age;
  String sex;
  Person(this.name, this.age, this.sex);
  info() {
    print("${this.name}");
    print("${this.age}");
    print("${this.sex}");
  }
}

<!-- 这里没有省略关键字new -->
main() {
  var p = new Person('张三', 20, '女');
  p.info();
}

<!-- 这里省略了关键字new -->
main() {
  var p = Person('张三', 20, '女');
  p.info();
}
```
