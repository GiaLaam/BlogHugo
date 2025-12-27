---
title: "So sánh TCP và UDP trong Lập Trình Mạng Java"
date: 2024-12-12
tags: ["Java", "TCP", "UDP", "Network"]
---

## TCP vs UDP

| Đặc điểm | TCP | UDP |
|----------|-----|-----|
| Kết nối | Có kết nối | Không kết nối |
| Độ tin cậy | Đảm bảo dữ liệu | Không đảm bảo |
| Thứ tự | Đúng thứ tự | Có thể sai thứ tự |
| Tốc độ | Chậm hơn | Nhanh hơn |

## Lập trình TCP với Socket

### TCP Server

```java
import java.net.*;
import java.io.*;

public class TCPServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(9000);
        System.out.println("TCP Server đang chạy...");
        
        Socket client = server.accept();
        
        BufferedReader in = new BufferedReader(
            new InputStreamReader(client.getInputStream())
        );
        PrintWriter out = new PrintWriter(
            client.getOutputStream(), true
        );
        
        String message = in.readLine();
        System.out.println("Nhận: " + message);
        out.println("Server đã nhận: " + message);
        
        client.close();
        server.close();
    }
}
```

### TCP Client

```java
public class TCPClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 9000);
        
        PrintWriter out = new PrintWriter(
            socket.getOutputStream(), true
        );
        BufferedReader in = new BufferedReader(
            new InputStreamReader(socket.getInputStream())
        );
        
        out.println("Hello TCP!");
        System.out.println("Server trả lời: " + in.readLine());
        
        socket.close();
    }
}
```

## Lập trình UDP với DatagramSocket

### UDP Server

```java
import java.net.*;

public class UDPServer {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(9001);
        byte[] buffer = new byte[1024];
        
        System.out.println("UDP Server đang chạy...");
        
        DatagramPacket packet = new DatagramPacket(
            buffer, buffer.length
        );
        socket.receive(packet);
        
        String message = new String(
            packet.getData(), 0, packet.getLength()
        );
        System.out.println("Nhận: " + message);
        
        socket.close();
    }
}
```

### UDP Client

```java
public class UDPClient {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket();
        
        String message = "Hello UDP!";
        byte[] buffer = message.getBytes();
        
        InetAddress address = InetAddress.getByName("localhost");
        DatagramPacket packet = new DatagramPacket(
            buffer, buffer.length, address, 9001
        );
        
        socket.send(packet);
        System.out.println("Đã gửi: " + message);
        
        socket.close();
    }
}
```

## Khi nào dùng TCP, khi nào dùng UDP?

- **TCP**: Chat, email, web, file transfer
- **UDP**: Streaming video, game online, VoIP
