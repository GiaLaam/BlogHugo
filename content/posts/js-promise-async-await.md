---
title: "Promise và Async/Await trong JavaScript"
date: 2024-12-02
tags: ["JavaScript", "Promise", "Async"]
---

## Promise là gì?

Promise đại diện cho một giá trị có thể có ngay bây giờ, trong tương lai, hoặc không bao giờ có. Nó giúp xử lý các tác vụ bất đồng bộ một cách dễ dàng hơn.

## Tạo Promise

```javascript
const myPromise = new Promise((resolve, reject) => {
  // Giả lập tác vụ bất đồng bộ
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve('Thành công!');
    } else {
      reject('Có lỗi xảy ra!');
    }
  }, 2000);
});

// Sử dụng Promise
myPromise
  .then(result => console.log(result))
  .catch(error => console.error(error));
```

## Promise States

1. **Pending**: Đang chờ
2. **Fulfilled**: Thành công
3. **Rejected**: Thất bại

## Async/Await

Async/await là cách viết code bất đồng bộ trông giống như code đồng bộ:

```javascript
async function fetchUserData() {
  try {
    const response = await fetch('/api/user');
    const user = await response.json();
    console.log('User:', user);
    return user;
  } catch (error) {
    console.error('Lỗi:', error);
  }
}
```

## Xử lý nhiều Promise

### Promise.all - Chờ tất cả hoàn thành

```javascript
async function fetchAllData() {
  const [users, posts, comments] = await Promise.all([
    fetch('/api/users').then(r => r.json()),
    fetch('/api/posts').then(r => r.json()),
    fetch('/api/comments').then(r => r.json()),
  ]);
  
  console.log({ users, posts, comments });
}
```

### Promise.race - Lấy kết quả đầu tiên

```javascript
const fastest = await Promise.race([
  fetch('/api/server1'),
  fetch('/api/server2'),
]);
```

## Ví dụ thực tế

```javascript
async function login(username, password) {
  try {
    // Gọi API đăng nhập
    const response = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username, password }),
    });
    
    if (!response.ok) {
      throw new Error('Sai tài khoản hoặc mật khẩu');
    }
    
    const data = await response.json();
    localStorage.setItem('token', data.token);
    return data;
  } catch (error) {
    console.error('Đăng nhập thất bại:', error.message);
    throw error;
  }
}
```

## Kết luận

Promise và Async/Await là nền tảng quan trọng trong JavaScript hiện đại, giúp xử lý code bất đồng bộ một cách sạch sẽ và dễ bảo trì.
