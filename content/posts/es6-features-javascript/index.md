---
title: "ES6/ES2015 - Những tính năng hiện đại của JavaScript"
date: 2024-02-05T14:00:00+07:00
draft: false
description: "Khám phá các tính năng mới trong ES6: Arrow Functions, Destructuring, Spread/Rest, Classes, Promises và nhiều hơn nữa"
tags: ["JavaScript", "ES6", "Modern JavaScript", "ES2015"]
categories: ["JavaScript"]
author: "Trung Tín"
showAuthor: true
showDate: true
showDateUpdated: true
showReadingTime: true
showTableOfContents: true
showPagination: true
showTaxonomies: true
showWordCount: true
---

{{< lead >}}
🚀 Bước vào thế giới **Modern JavaScript** với ES6/ES2015 - cuộc cách mạng làm thay đổi cách chúng ta code JavaScript forever!
{{< /lead >}}

## Giới thiệu ES6

{{< alert "star" >}}
**⭐ Game Changer:** ES6 (2015) là bản update **LỚN NHẤT** của JavaScript, mang đến hàng chục tính năng mới giúp code ngắn gọn, sạch đẹp và mạnh mẽ gấp 10 lần!
{{< /alert >}}

ES6 (ECMAScript 2015) là bản cập nhật lớn nhất của JavaScript, mang đến nhiều tính năng mới giúp code ngắn gọn, rõ ràng và mạnh mẽ hơn.

## 1. Let và Const

### Vấn đề với var

```javascript
// var: function scope, có hoisting
if (true) {
    var x = 10;
}
console.log(x); // 10 - truy cập được từ bên ngoài block!

// Hoisting
console.log(y); // undefined (không lỗi)
var y = 5;
```

### Let: Block Scope

```javascript
// let: block scope
if (true) {
    let x = 10;
}
console.log(x); // ReferenceError - không truy cập được!

// Không có hoisting
console.log(y); // ReferenceError
let y = 5;

// Can be reassigned
let count = 1;
count = 2; // OK
```

### Const: Constant

```javascript
// const: không thể reassign
const PI = 3.14159;
PI = 3.14; // TypeError!

// Nhưng object/array vẫn mutable
const person = {name: "Tín"};
person.name = "Nam"; // OK - modify property
person.age = 20;     // OK - add property

const numbers = [1, 2, 3];
numbers.push(4);     // OK - modify array
numbers = [5, 6];    // TypeError - không reassign!

// Best practice
const CONFIG = {
    API_URL: "https://api.example.com",
    TIMEOUT: 5000
};
```

## 2. Arrow Functions

```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function - cú pháp ngắn gọn
const add = (a, b) => a + b;

// Nhiều statements cần {}
const multiply = (a, b) => {
    const result = a * b;
    return result;
};

// Single parameter không cần ()
const square = x => x * x;

// No parameters cần ()
const greet = () => console.log("Hello!");

// Returning object cần ()
const makePerson = (name, age) => ({name, age});

console.log(makePerson("Tín", 20)); // {name: "Tín", age: 20}
```

### Lexical `this`

```javascript
// Traditional function - this phụ thuộc context
function Counter() {
    this.count = 0;
    
    setInterval(function() {
        this.count++; // this không trỏ đến Counter!
        console.log(this.count); // NaN
    }, 1000);
}

// Arrow function - this bind với outer scope
function Counter() {
    this.count = 0;
    
    setInterval(() => {
        this.count++; // this trỏ đến Counter
        console.log(this.count); // 1, 2, 3, ...
    }, 1000);
}

const counter = new Counter();
```

## 3. Template Literals

```javascript
// Old way - string concatenation
let name = "Trung Tín";
let age = 20;
let message = "Tôi là " + name + ", " + age + " tuổi";

// Template literals - clean & readable
let message = `Tôi là ${name}, ${age} tuổi`;

// Multi-line strings
let html = `
    <div class="user">
        <h2>${name}</h2>
        <p>Age: ${age}</p>
    </div>
`;

// Expressions inside ${}
let a = 10, b = 20;
console.log(`Tổng: ${a + b}`);           // "Tổng: 30"
console.log(`Double: ${a * 2}`);         // "Double: 20"
console.log(`Is adult? ${age >= 18}`);   // "Is adult? true"

// Function calls
function getGreeting() {
    return "Xin chào";
}
console.log(`${getGreeting()}, ${name}!`); // "Xin chào, Trung Tín!"
```

## 4. Destructuring

### Array Destructuring

```javascript
// Old way
let arr = [1, 2, 3];
let a = arr[0];
let b = arr[1];
let c = arr[2];

// Destructuring - elegant!
let [a, b, c] = [1, 2, 3];
console.log(a, b, c); // 1 2 3

// Skip elements
let [first, , third] = [1, 2, 3];
console.log(first, third); // 1 3

// Rest operator
let [head, ...tail] = [1, 2, 3, 4, 5];
console.log(head); // 1
console.log(tail); // [2, 3, 4, 5]

// Default values
let [x = 5, y = 10] = [20];
console.log(x, y); // 20 10

// Swap variables
let a = 1, b = 2;
[a, b] = [b, a];
console.log(a, b); // 2 1
```

### Object Destructuring

```javascript
// Old way
let person = {name: "Tín", age: 20, city: "Hà Nội"};
let name = person.name;
let age = person.age;

// Destructuring
let {name, age} = person;
console.log(name, age); // "Tín" 20

// Rename variables
let {name: fullName, age: years} = person;
console.log(fullName, years); // "Tín" 20

// Default values
let {name, age, country = "Vietnam"} = person;
console.log(country); // "Vietnam"

// Nested destructuring
let user = {
    id: 1,
    name: "Tín",
    address: {
        city: "Hà Nội",
        district: "Cầu Giấy"
    }
};

let {name, address: {city, district}} = user;
console.log(city, district); // "Hà Nội" "Cầu Giấy"

// Function parameters
function greet({name, age}) {
    console.log(`Xin chào ${name}, ${age} tuổi`);
}

greet({name: "Tín", age: 20}); // "Xin chào Tín, 20 tuổi"
```

## 5. Spread & Rest Operators

### Spread Operator (...)

```javascript
// Array spread
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Copy array
let original = [1, 2, 3];
let copy = [...original];
copy.push(4);
console.log(original); // [1, 2, 3] - không bị ảnh hưởng
console.log(copy);     // [1, 2, 3, 4]

// Object spread
let obj1 = {a: 1, b: 2};
let obj2 = {c: 3, d: 4};
let merged = {...obj1, ...obj2};
console.log(merged); // {a: 1, b: 2, c: 3, d: 4}

// Override properties
let user = {name: "Tín", age: 20};
let updated = {...user, age: 21, city: "HN"};
console.log(updated); // {name: "Tín", age: 21, city: "HN"}

// Function arguments
function sum(a, b, c) {
    return a + b + c;
}
let numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```

### Rest Operator (...)

```javascript
// Rest parameters - collect arguments
function sum(...numbers) {
    return numbers.reduce((total, n) => total + n, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// Combine with regular parameters
function multiply(multiplier, ...numbers) {
    return numbers.map(n => n * multiplier);
}

console.log(multiply(2, 1, 2, 3)); // [2, 4, 6]

// Rest in destructuring
let [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]

let {a, b, ...others} = {a: 1, b: 2, c: 3, d: 4};
console.log(others); // {c: 3, d: 4}
```

## 6. Default Parameters

```javascript
// Old way
function greet(name, greeting) {
    greeting = greeting || "Hello";
    return greeting + ", " + name;
}

// ES6 - cleaner
function greet(name, greeting = "Hello") {
    return `${greeting}, ${name}`;
}

console.log(greet("Tín"));              // "Hello, Tín"
console.log(greet("Tín", "Chào"));      // "Chào, Tín"

// Default với expression
function createUser(name, role = "user", createdAt = Date.now()) {
    return {name, role, createdAt};
}

// Skip default parameter
function test(a, b = 2, c = 3) {
    console.log(a, b, c);
}

test(1, undefined, 5); // 1 2 5 - b use default
```

## 7. Enhanced Object Literals

```javascript
// Property shorthand
let name = "Tín";
let age = 20;

// Old way
let person = {
    name: name,
    age: age
};

// ES6 - shorthand
let person = {name, age};

// Method shorthand
// Old way
let obj = {
    greet: function() {
        return "Hello";
    }
};

// ES6
let obj = {
    greet() {
        return "Hello";
    }
};

// Computed property names
let key = "name";
let person = {
    [key]: "Tín",
    ["age"]: 20,
    [`get${key}`]() {
        return this[key];
    }
};

console.log(person.name);     // "Tín"
console.log(person.getname()); // "Tín"
```

## 8. Classes

```javascript
// ES6 Class syntax
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Method
    introduce() {
        return `Tôi là ${this.name}, ${this.age} tuổi`;
    }
    
    // Getter
    get info() {
        return `${this.name} - ${this.age}`;
    }
    
    // Setter
    set birthYear(year) {
        this.age = new Date().getFullYear() - year;
    }
    
    // Static method
    static create(name, age) {
        return new Person(name, age);
    }
}

// Create instance
let person = new Person("Tín", 20);
console.log(person.introduce());

// Use getter/setter
console.log(person.info);
person.birthYear = 2004;

// Static method
let person2 = Person.create("Nam", 22);

// Inheritance
class Student extends Person {
    constructor(name, age, major) {
        super(name, age); // Call parent constructor
        this.major = major;
    }
    
    // Override method
    introduce() {
        return `${super.introduce()}, học ${this.major}`;
    }
    
    // New method
    study() {
        return `${this.name} đang học ${this.major}`;
    }
}

let student = new Student("Tín", 20, "Computer Science");
console.log(student.introduce());
console.log(student.study());
```

## 9. Promises

```javascript
// Create Promise
const fetchData = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const data = {name: "Tín", age: 20};
            
            if (data) {
                resolve(data); // Success
            } else {
                reject("Error: No data"); // Error
            }
        }, 1000);
    });
};

// Use Promise
fetchData()
    .then(data => {
        console.log("Success:", data);
        return data.name;
    })
    .then(name => {
        console.log("Name:", name);
    })
    .catch(error => {
        console.error("Error:", error);
    })
    .finally(() => {
        console.log("Done!");
    });

// Multiple Promises
Promise.all([
    Promise.resolve(1),
    Promise.resolve(2),
    Promise.resolve(3)
]).then(results => {
    console.log(results); // [1, 2, 3]
});

Promise.race([
    new Promise(resolve => setTimeout(() => resolve("fast"), 100)),
    new Promise(resolve => setTimeout(() => resolve("slow"), 1000))
]).then(result => {
    console.log(result); // "fast" - first to complete
});
```

## 10. Modules

```javascript
// math.js - Export
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

// Default export
export default function subtract(a, b) {
    return a - b;
}

// app.js - Import
import subtract from './math.js';           // Default import
import {PI, add, multiply} from './math.js'; // Named imports
import * as math from './math.js';          // Import all

console.log(PI);              // 3.14159
console.log(add(5, 3));       // 8
console.log(subtract(10, 4)); // 6
console.log(math.multiply(2, 3)); // 6
```

## Kết luận

{{< timeline >}}

{{< timelineItem icon="code" header="Variables & Functions" badge="Foundation" >}}
{{< badge >}}let/const{{< /badge >}} {{< badge >}}Arrow Functions{{< /badge >}} {{< badge >}}Template Literals{{< /badge >}} - Viết code ngắn gọn và an toàn hơn!
{{< /timelineItem >}}

{{< timelineItem icon="cube" header="Data Manipulation" badge="Power Tools" >}}
{{< badge >}}Destructuring{{< /badge >}} {{< badge >}}Spread/Rest{{< /badge >}} {{< badge >}}Default Params{{< /badge >}} - Xử lý data như một pro!
{{< /timelineItem >}}

{{< timelineItem icon="sitemap" header="OOP & Async" badge="Advanced" >}}
{{< badge >}}Classes{{< /badge >}} {{< badge >}}Promises{{< /badge >}} {{< badge >}}Modules{{< /badge >}} - Build scalable applications!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ Modern JavaScript Standard:** ES6 là **MUST-KNOW** cho mọi JavaScript developer. Tất cả frameworks hiện đại (React, Vue, Angular) đều sử dụng ES6+!
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://github.com/lukehoban/es6features" target="_blank" >}}
📚 ES6 Features Overview
{{< /button >}}

{{< button href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide" target="_blank" >}}
📖 MDN JavaScript Guide
{{< /button >}}

{{< button href="https://javascript.info/" target="_blank" >}}
🎓 JavaScript.info
{{< /button >}}

{{< button href="https://exploringjs.com/es6/" target="_blank" >}}
📕 Exploring ES6
{{< /button >}}

