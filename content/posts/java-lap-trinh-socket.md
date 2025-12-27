---
title: "Lập Trình Socket trong Java - Cơ Bản"
date: 2024-12-20
tags: ["Java", "Network", "Socket"]
---

## Giới thiệu về Socket trong Java

Socket là một điểm cuối (endpoint) trong giao tiếp hai chiều giữa hai chương trình chạy trên mạng. Java cung cấp các lớp `Socket` và `ServerSocket` trong package `java.net` để hỗ trợ lập trình mạng.

## Mô hình Client-Server

Trong mô hình này:
- **Server**: Lắng nghe kết nối từ client
- **Client**: Khởi tạo kết nối đến server

## Ví dụ Server đơn giản

```java
import java.net.*;
import java.io.*;

public class SimpleServer {
    public static void main(String[] args) {
        try {
            // Tạo ServerSocket lắng nghe port 8080
            ServerSocket serverSocket = new ServerSocket(8080);
            System.out.println("Server đang chờ kết nối...");
            
            // Chấp nhận kết nối từ client
            Socket clientSocket = serverSocket.accept();
            System.out.println("Client đã kết nối!");
            
            // Đọc dữ liệu từ client
            BufferedReader in = new BufferedReader(
                new InputStreamReader(clientSocket.getInputStream())
            );
            String message = in.readLine();
            System.out.println("Nhận được: " + message);
            
            // Đóng kết nối
            clientSocket.close();
            serverSocket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Ví dụ Client đơn giản

```java
import java.net.*;
import java.io.*;

public class SimpleClient {
    public static void main(String[] args) {
        try {
            // Kết nối đến server
            Socket socket = new Socket("localhost", 8080);
            
            // Gửi dữ liệu đến server
            PrintWriter out = new PrintWriter(
                socket.getOutputStream(), true
            );
            out.println("Xin chào từ Client!");
            
            // Đóng kết nối
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Kết luận

Lập trình Socket là nền tảng quan trọng trong việc xây dựng các ứng dụng mạng. Hiểu rõ cách thức hoạt động của Socket giúp bạn phát triển các ứng dụng chat, game online, hoặc các hệ thống phân tán.
