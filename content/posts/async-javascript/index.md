---
title: "Asynchronous JavaScript - Callbacks, Promises và Async/Await"
date: 2024-02-10T16:00:00+07:00
draft: false
description: "Hiểu về lập trình bất đồng bộ trong JavaScript với Callbacks, Promises và Async/Await, xử lý Event Loop"
tags: ["JavaScript", "Async", "Promises", "Async/Await", "Event Loop"]
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
🚀 Khám phá bí mật đằng sau **JavaScript Async Programming** - từ Callbacks cổ điển đến Async/Await hiện đại, master Event Loop và viết code non-blocking như một pro!
{{< /lead >}}

## Giới thiệu Asynchronous Programming

{{< alert "circle-info" >}}
**Tại sao cần học Async?** JavaScript là single-threaded nhưng có thể xử lý hàng ngàn operations đồng thời! Bí mật nằm ở **Event Loop** và **Asynchronous Programming**.
{{< /alert >}}

JavaScript là **single-threaded** (đơn luồng) nhưng có thể xử lý các tác vụ bất đồng bộ (asynchronous) nhờ vào:
- **Call Stack**: Nơi thực thi code đồng bộ
- **Web APIs**: setTimeout, fetch, DOM events
- **Callback Queue**: Hàng đợi callbacks
- **Event Loop**: Kiểm tra và chuyển callbacks từ queue vào stack

## Tại sao cần Async?

```javascript
// Synchronous - blocking code
console.log("Start");

// Giả sử API call mất 3 giây
const data = fetchDataFromAPI(); // BLOCKING!
console.log(data);

console.log("End"); // Phải chờ 3 giây mới chạy

// Asynchronous - non-blocking
console.log("Start");

fetchDataFromAPI((data) => {
    console.log(data); // Chạy sau khi có data
});

console.log("End"); // Chạy ngay, không chờ
```

## 1. Callbacks

{{< badge >}}
📌 Foundation
{{< /badge >}}

Callback là function được truyền như argument và thực thi sau khi một tác vụ hoàn thành.

### Ví dụ cơ bản

```javascript
// setTimeout - async function
setTimeout(() => {
    console.log("Chạy sau 2 giây");
}, 2000);

console.log("Chạy ngay lập tức");

// Output:
// "Chạy ngay lập tức"
// (sau 2 giây) "Chạy sau 2 giây"
```

### Callback Hell (Pyramid of Doom)

```javascript
// Giả lập API calls
function getUser(userId, callback) {
    setTimeout(() => {
        console.log("Got user");
        callback({id: userId, name: "Tín"});
    }, 1000);
}

function getPosts(userId, callback) {
    setTimeout(() => {
        console.log("Got posts");
        callback([
            {id: 1, title: "Post 1"},
            {id: 2, title: "Post 2"}
        ]);
    }, 1000);
}

function getComments(postId, callback) {
    setTimeout(() => {
        console.log("Got comments");
        callback([
            {id: 1, text: "Comment 1"},
            {id: 2, text: "Comment 2"}
        ]);
    }, 1000);
}

// Callback Hell - khó đọc, khó maintain
getUser(1, (user) => {
    console.log("User:", user);
    
    getPosts(user.id, (posts) => {
        console.log("Posts:", posts);
        
        getComments(posts[0].id, (comments) => {
            console.log("Comments:", comments);
            
            // Càng lồng càng sâu...
        });
    });
});
```

### Vấn đề với Callbacks

{{< alert "triangle-exclamation" >}}
**⚠️ Callback Hell Warning!** Lồng quá nhiều callbacks sẽ tạo ra "Pyramid of Doom" - code khó đọc, khó debug, và khó maintain!
{{< /alert >}}

**Các vấn đề chính:**
1. {{< badge >}}Callback Hell{{< /badge >}} Code khó đọc, khó maintain
2. {{< badge >}}Error Handling{{< /badge >}} Khó xử lý lỗi
3. {{< badge >}}Inversion of Control{{< /badge >}} Mất quyền kiểm soát

## 2. Promises

{{< badge >}}
🔥 Modern Solution
{{< /badge >}}

Promise là object đại diện cho kết quả (hoặc lỗi) của một async operation trong tương lai.

{{< alert "lightbulb" >}}
**💡 Promise Insight:** Promise là "lời hứa" về một giá trị trong tương lai - có thể thành công (fulfilled) hoặc thất bại (rejected)!
{{< /alert >}}

### Promise States

{{< mermaid >}}
stateDiagram-v2
    [*] --> Pending: Create Promise
    Pending --> Fulfilled: resolve()
    Pending --> Rejected: reject()
    Fulfilled --> [*]: .then()
    Rejected --> [*]: .catch()
{{< /mermaid >}}

- {{< badge >}}Pending{{< /badge >}} Initial state, đang chờ
- {{< badge >}}Fulfilled{{< /badge >}} Operation completed successfully
- {{< badge >}}Rejected{{< /badge >}} Operation failed

### Tạo Promise

```javascript
// Create Promise
const myPromise = new Promise((resolve, reject) => {
    // Async operation
    setTimeout(() => {
        const success = true;
        
        if (success) {
            resolve("Success! Data here"); // Fulfilled
        } else {
            reject("Error! Something wrong"); // Rejected
        }
    }, 1000);
});

// Use Promise
myPromise
    .then(data => {
        console.log(data); // "Success! Data here"
    })
    .catch(error => {
        console.error(error);
    });
```

### Promise Chain

```javascript
// Giả lập API functions trả về Promises
function getUser(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Got user");
            resolve({id: userId, name: "Tín"});
        }, 1000);
    });
}

function getPosts(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Got posts");
            resolve([
                {id: 1, title: "Post 1", userId},
                {id: 2, title: "Post 2", userId}
            ]);
        }, 1000);
    });
}

function getComments(postId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Got comments");
            resolve([
                {id: 1, text: "Comment 1", postId},
                {id: 2, text: "Comment 2", postId}
            ]);
        }, 1000);
    });
}

// Promise Chain - clean & readable
getUser(1)
    .then(user => {
        console.log("User:", user);
        return getPosts(user.id);
    })
    .then(posts => {
        console.log("Posts:", posts);
        return getComments(posts[0].id);
    })
    .then(comments => {
        console.log("Comments:", comments);
    })
    .catch(error => {
        console.error("Error:", error);
    })
    .finally(() => {
        console.log("All done!");
    });
```

### Promise Methods

```javascript
// Promise.all - chờ tất cả promises complete
Promise.all([
    Promise.resolve(1),
    Promise.resolve(2),
    Promise.resolve(3)
]).then(results => {
    console.log(results); // [1, 2, 3]
});

// Nếu một promise reject, all reject
Promise.all([
    Promise.resolve(1),
    Promise.reject("Error!"),
    Promise.resolve(3)
]).catch(error => {
    console.error(error); // "Error!"
});

// Promise.allSettled - chờ tất cả, không quan tâm success/fail
Promise.allSettled([
    Promise.resolve(1),
    Promise.reject("Error!"),
    Promise.resolve(3)
]).then(results => {
    console.log(results);
    // [
    //   {status: "fulfilled", value: 1},
    //   {status: "rejected", reason: "Error!"},
    //   {status: "fulfilled", value: 3}
    // ]
});

// Promise.race - trả về promise complete đầu tiên
Promise.race([
    new Promise(resolve => setTimeout(() => resolve("slow"), 1000)),
    new Promise(resolve => setTimeout(() => resolve("fast"), 100))
]).then(result => {
    console.log(result); // "fast"
});

// Promise.any - trả về promise fulfilled đầu tiên
Promise.any([
    Promise.reject("Error 1"),
    Promise.resolve("Success!"),
    Promise.reject("Error 2")
]).then(result => {
    console.log(result); // "Success!"
});
```

## 3. Async/Await

{{< badge >}}
⚡ Best Practice
{{< /badge >}}

Async/Await là syntactic sugar trên Promises, giúp code async trông như sync code.

{{< alert "star" >}}
**⭐ Modern JavaScript:** Async/Await là cách **RECOMMENDED** để xử lý async operations - clean, readable, và easy to debug!
{{< /alert >}}

### Cú pháp cơ bản

```javascript
// Async function luôn return Promise
async function fetchData() {
    return "Data";
}

fetchData().then(data => console.log(data)); // "Data"

// Await chỉ dùng trong async function
async function getData() {
    const data = await fetchData(); // Chờ Promise resolve
    console.log(data);
}

getData();
```

### Refactor với Async/Await

```javascript
// Với Promises
function getUserData() {
    getUser(1)
        .then(user => {
            console.log("User:", user);
            return getPosts(user.id);
        })
        .then(posts => {
            console.log("Posts:", posts);
            return getComments(posts[0].id);
        })
        .then(comments => {
            console.log("Comments:", comments);
        })
        .catch(error => {
            console.error("Error:", error);
        });
}

// Với Async/Await - clean & readable
async function getUserData() {
    try {
        const user = await getUser(1);
        console.log("User:", user);
        
        const posts = await getPosts(user.id);
        console.log("Posts:", posts);
        
        const comments = await getComments(posts[0].id);
        console.log("Comments:", comments);
        
    } catch (error) {
        console.error("Error:", error);
    }
}

getUserData();
```

### Error Handling

```javascript
// Try-catch
async function fetchUserData(userId) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
        
    } catch (error) {
        console.error("Failed to fetch user:", error);
        throw error; // Re-throw if needed
    }
}

// Multiple try-catch
async function processData() {
    try {
        const user = await fetchUserData(1);
        console.log("User:", user);
    } catch (error) {
        console.error("User error:", error);
    }
    
    try {
        const posts = await fetchPosts();
        console.log("Posts:", posts);
    } catch (error) {
        console.error("Posts error:", error);
    }
}
```

### Parallel Execution

```javascript
// Sequential - slow (tổng 3 giây)
async function sequential() {
    const user = await getUser(1);      // 1 giây
    const posts = await getPosts(1);    // 1 giây
    const comments = await getComments(1); // 1 giây
    
    return {user, posts, comments};
}

// Parallel - fast (1 giây - chạy đồng thời)
async function parallel() {
    const [user, posts, comments] = await Promise.all([
        getUser(1),
        getPosts(1),
        getComments(1)
    ]);
    
    return {user, posts, comments};
}

// Run immediately, await later
async function parallelOptimized() {
    const userPromise = getUser(1);     // Start
    const postsPromise = getPosts(1);    // Start
    const commentsPromise = getComments(1); // Start
    
    const user = await userPromise;      // Wait
    const posts = await postsPromise;    // Wait
    const comments = await commentsPromise; // Wait
    
    return {user, posts, comments};
}
```

## Ví dụ thực tế: Fetch API

```javascript
// Fetch data from API
async function fetchGitHubUser(username) {
    try {
        console.log(`Fetching data for ${username}...`);
        
        const response = await fetch(`https://api.github.com/users/${username}`);
        
        if (!response.ok) {
            throw new Error(`User not found! Status: ${response.status}`);
        }
        
        const user = await response.json();
        
        console.log("User Info:");
        console.log(`- Name: ${user.name}`);
        console.log(`- Bio: ${user.bio}`);
        console.log(`- Public Repos: ${user.public_repos}`);
        console.log(`- Followers: ${user.followers}`);
        
        return user;
        
    } catch (error) {
        console.error("Error fetching user:", error.message);
        throw error;
    }
}

// Fetch multiple users
async function fetchMultipleUsers(usernames) {
    try {
        const promises = usernames.map(username => 
            fetch(`https://api.github.com/users/${username}`)
                .then(res => res.json())
        );
        
        const users = await Promise.all(promises);
        
        users.forEach(user => {
            console.log(`${user.name}: ${user.public_repos} repos`);
        });
        
        return users;
        
    } catch (error) {
        console.error("Error fetching users:", error);
    }
}

// Usage
fetchGitHubUser("torvalds");
fetchMultipleUsers(["torvalds", "gvanrossum", "BrendanEich"]);
```

## Event Loop - Cách JavaScript xử lý Async

{{< alert "circle-info" >}}
**🔄 Event Loop Magic:** Đây là "trái tim" của JavaScript async - hiểu Event Loop = hiểu cách JS hoạt động!
{{< /alert >}}

{{< mermaid >}}
graph TD
    A[Call Stack] --> B{Stack Empty?}
    B -->|Yes| C[Check Microtask Queue]
    C --> D{Has Microtasks?}
    D -->|Yes| E[Execute Microtasks]
    E --> A
    D -->|No| F[Check Macrotask Queue]
    F --> G{Has Macrotasks?}
    G -->|Yes| H[Execute One Macrotask]
    H --> A
    G -->|No| B
    
    style A fill:#ff6b6b
    style C fill:#4ecdc4
    style F fill:#45b7d1
{{< /mermaid >}}

```javascript
console.log("1. Start");

setTimeout(() => {
    console.log("2. Timeout callback");
}, 0);

Promise.resolve()
    .then(() => {
        console.log("3. Promise callback");
    });

console.log("4. End");

// Output:
// 1. Start
// 4. End
// 3. Promise callback
// 2. Timeout callback

// Giải thích:
// - Synchronous code chạy trước (1, 4)
// - Microtasks (Promises) chạy trước Macrotasks (setTimeout)
// - Nên: 3 chạy trước 2
```

## Best Practices

### 1. Always handle errors

```javascript
// ❌ Bad - không handle error
async function badFetch() {
    const data = await fetch(url);
    return data.json();
}

// ✅ Good - handle error
async function goodFetch() {
    try {
        const data = await fetch(url);
        return await data.json();
    } catch (error) {
        console.error("Fetch failed:", error);
        throw error;
    }
}
```

### 2. Avoid nesting

```javascript
// ❌ Bad - nested async
async function bad() {
    const user = await getUser();
    
    if (user) {
        const posts = await getPosts(user.id);
        
        if (posts.length > 0) {
            const comments = await getComments(posts[0].id);
            return comments;
        }
    }
}

// ✅ Good - early return
async function good() {
    const user = await getUser();
    if (!user) return null;
    
    const posts = await getPosts(user.id);
    if (posts.length === 0) return null;
    
    return await getComments(posts[0].id);
}
```

### 3. Use Promise.all for parallel

```javascript
// ❌ Bad - sequential
async function bad() {
    const user = await getUser();
    const posts = await getPosts();
    const comments = await getComments();
    return {user, posts, comments};
}

// ✅ Good - parallel
async function good() {
    const [user, posts, comments] = await Promise.all([
        getUser(),
        getPosts(),
        getComments()
    ]);
    return {user, posts, comments};
}
```

## Kết luận

{{< timeline >}}

{{< timelineItem icon="code" header="Level 1: Callbacks" badge="Foundation" subheader="2009-2015" >}}
Cách cơ bản để handle async, nhưng dễ rơi vào **Callback Hell**. Phù hợp cho operations đơn giản.
{{< /timelineItem >}}

{{< timelineItem icon="link" header="Level 2: Promises" badge="Modern" subheader="2015-2017" >}}
Giải quyết callback hell với **chainable syntax**. Code clean hơn, dễ maintain hơn, có error handling tốt.
{{< /timelineItem >}}

{{< timelineItem icon="star" header="Level 3: Async/Await" badge="Best Practice" subheader="2017-Now" >}}
**RECOMMENDED!** Syntax clean nhất, code như synchronous, dễ đọc và debug. Đây là standard của modern JavaScript!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ Best Practice:** Luôn sử dụng **Async/Await** cho mọi async operations. Chỉ quay lại Promises khi cần Promise.all/race!
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous" target="_blank" >}}
📚 MDN - Async JavaScript
{{< /button >}}

{{< button href="https://javascript.info/async" target="_blank" >}}
📖 JavaScript.info - Promises
{{< /button >}}

{{< button href="http://latentflip.com/loupe/" target="_blank" >}}
🎥 Event Loop Visualization
{{< /button >}}

{{< button href="https://github.com/getify/You-Dont-Know-JS" target="_blank" >}}
📕 You Don't Know JS
{{< /button >}}

