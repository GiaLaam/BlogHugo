---
title: "WebSocket trong JavaScript - Giao Tiếp Real-time"
date: 2024-12-05
tags: ["JavaScript", "WebSocket", "Real-time"]
---

## WebSocket là gì?

WebSocket cung cấp kênh giao tiếp hai chiều (full-duplex) giữa client và server thông qua một kết nối TCP. Khác với HTTP, WebSocket cho phép server chủ động gửi dữ liệu đến client.

## Tạo kết nối WebSocket

```javascript
// Kết nối đến server
const socket = new WebSocket('ws://localhost:8080');

// Sự kiện khi kết nối thành công
socket.onopen = function(event) {
  console.log('Đã kết nối!');
  socket.send('Xin chào Server!');
};

// Nhận tin nhắn từ server
socket.onmessage = function(event) {
  console.log('Nhận được:', event.data);
};

// Xử lý lỗi
socket.onerror = function(error) {
  console.error('Lỗi WebSocket:', error);
};

// Khi đóng kết nối
socket.onclose = function(event) {
  console.log('Đã đóng kết nối');
};
```

## Gửi dữ liệu

```javascript
// Gửi text
socket.send('Hello World!');

// Gửi JSON
const data = { type: 'message', content: 'Xin chào' };
socket.send(JSON.stringify(data));

// Gửi binary data
const buffer = new ArrayBuffer(8);
socket.send(buffer);
```

## Ví dụ: Chat Application

```javascript
class ChatClient {
  constructor(url) {
    this.socket = new WebSocket(url);
    this.setupListeners();
  }
  
  setupListeners() {
    this.socket.onopen = () => {
      console.log('Đã vào phòng chat');
    };
    
    this.socket.onmessage = (event) => {
      const message = JSON.parse(event.data);
      this.displayMessage(message);
    };
  }
  
  sendMessage(text) {
    const message = {
      type: 'chat',
      text: text,
      time: new Date().toISOString()
    };
    this.socket.send(JSON.stringify(message));
  }
  
  displayMessage(message) {
    const chatBox = document.getElementById('chat');
    chatBox.innerHTML += `<p>${message.text}</p>`;
  }
}

// Sử dụng
const chat = new ChatClient('ws://localhost:8080/chat');
chat.sendMessage('Xin chào mọi người!');
```

## Kết luận

WebSocket là giải pháp tuyệt vời cho các ứng dụng cần giao tiếp real-time như chat, game online, notifications, live updates.
