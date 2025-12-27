---
title: "Xử lý DOM và Events trong JavaScript"
date: 2024-11-28
tags: ["JavaScript", "DOM", "Events"]
---

## DOM là gì?

DOM (Document Object Model) là giao diện lập trình cho phép JavaScript tương tác và thay đổi nội dung, cấu trúc và style của trang web.

## Truy cập DOM Elements

```javascript
// Theo ID
const header = document.getElementById('header');

// Theo class
const buttons = document.getElementsByClassName('btn');

// Theo tag
const paragraphs = document.getElementsByTagName('p');

// Query Selector - phổ biến nhất
const element = document.querySelector('.container');
const allCards = document.querySelectorAll('.card');
```

## Thay đổi nội dung

```javascript
// Thay đổi text
element.textContent = 'Nội dung mới';

// Thay đổi HTML
element.innerHTML = '<strong>Text đậm</strong>';

// Thay đổi style
element.style.color = 'red';
element.style.backgroundColor = '#f0f0f0';

// Thêm/xóa class
element.classList.add('active');
element.classList.remove('hidden');
element.classList.toggle('selected');
```

## Xử lý Events

```javascript
// Cách 1: addEventListener (khuyến khích)
const button = document.querySelector('#myButton');

button.addEventListener('click', function(event) {
  console.log('Button được click!');
  console.log('Event:', event);
});

// Cách 2: Arrow function
button.addEventListener('click', (e) => {
  e.preventDefault(); // Ngăn hành vi mặc định
  console.log('Clicked!');
});
```

## Các Event phổ biến

```javascript
// Mouse events
element.addEventListener('click', handler);
element.addEventListener('dblclick', handler);
element.addEventListener('mouseenter', handler);
element.addEventListener('mouseleave', handler);

// Keyboard events
document.addEventListener('keydown', (e) => {
  console.log('Phím:', e.key);
  if (e.key === 'Enter') {
    // Xử lý khi nhấn Enter
  }
});

// Form events
form.addEventListener('submit', (e) => {
  e.preventDefault();
  const formData = new FormData(form);
  // Xử lý dữ liệu form
});

input.addEventListener('input', (e) => {
  console.log('Giá trị:', e.target.value);
});
```

## Tạo và thêm Elements

```javascript
// Tạo element mới
const newDiv = document.createElement('div');
newDiv.textContent = 'Element mới';
newDiv.classList.add('card');

// Thêm vào DOM
document.body.appendChild(newDiv);

// Thêm trước một element khác
parent.insertBefore(newDiv, referenceElement);

// Xóa element
element.remove();
```

## Ví dụ: Todo List

```javascript
const todoInput = document.querySelector('#todoInput');
const todoList = document.querySelector('#todoList');
const addBtn = document.querySelector('#addBtn');

addBtn.addEventListener('click', () => {
  const text = todoInput.value.trim();
  if (text) {
    const li = document.createElement('li');
    li.textContent = text;
    li.addEventListener('click', () => li.remove());
    todoList.appendChild(li);
    todoInput.value = '';
  }
});
```

## Kết luận

Hiểu rõ DOM và Events là kỹ năng cốt lõi để xây dựng các ứng dụng web tương tác. Kết hợp với CSS, bạn có thể tạo ra những trải nghiệm người dùng tuyệt vời.
