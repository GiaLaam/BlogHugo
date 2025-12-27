---
title: "Fetch API trong JavaScript - Gọi API đơn giản"
date: 2024-12-08
tags: ["JavaScript", "Fetch", "API"]
---

## Fetch API là gì?

Fetch API là cách hiện đại để thực hiện HTTP requests trong JavaScript, thay thế cho XMLHttpRequest cũ. Nó trả về Promise, dễ sử dụng với async/await.

## GET Request cơ bản

```javascript
// Cách 1: Dùng .then()
fetch('https://api.github.com/users/GiaLaam')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Lỗi:', error));

// Cách 2: Dùng async/await
async function getUser() {
  try {
    const response = await fetch('https://api.github.com/users/GiaLaam');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Lỗi:', error);
  }
}
```

## POST Request

```javascript
async function createUser(userData) {
  const response = await fetch('https://api.example.com/users', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(userData),
  });
  
  if (!response.ok) {
    throw new Error('Lỗi khi tạo user');
  }
  
  return await response.json();
}

// Sử dụng
createUser({ name: 'Gia Lâm', email: 'test@example.com' })
  .then(user => console.log('Đã tạo:', user));
```

## Xử lý lỗi

```javascript
async function fetchWithErrorHandling(url) {
  try {
    const response = await fetch(url);
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    if (error.name === 'TypeError') {
      console.error('Lỗi mạng hoặc CORS');
    } else {
      console.error('Lỗi:', error.message);
    }
    throw error;
  }
}
```

## Gửi Form Data

```javascript
async function uploadFile(file) {
  const formData = new FormData();
  formData.append('file', file);
  
  const response = await fetch('/upload', {
    method: 'POST',
    body: formData,
  });
  
  return await response.json();
}
```

## Kết luận

Fetch API là công cụ mạnh mẽ và đơn giản để làm việc với HTTP trong JavaScript. Kết hợp với async/await giúp code dễ đọc và bảo trì hơn.
