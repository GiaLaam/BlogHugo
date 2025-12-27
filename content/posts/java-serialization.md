---
title: "Serialization trong Java - Gửi Object qua Mạng"
date: 2024-12-10
tags: ["Java", "Serialization", "Network"]
---

## Serialization là gì?

Serialization là quá trình chuyển đổi một object thành chuỗi bytes để có thể lưu trữ hoặc truyền qua mạng. Deserialization là quá trình ngược lại.

## Tạo class có thể Serialize

```java
import java.io.Serializable;

public class User implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private String name;
    private int age;
    private transient String password; // Không serialize
    
    public User(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }
    
    // Getters và Setters
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

## Gửi Object qua Socket

### Server nhận Object

```java
import java.net.*;
import java.io.*;

public class ObjectServer {
    public static void main(String[] args) throws Exception {
        ServerSocket server = new ServerSocket(8888);
        System.out.println("Đang chờ client...");
        
        Socket client = server.accept();
        
        ObjectInputStream ois = new ObjectInputStream(
            client.getInputStream()
        );
        
        User user = (User) ois.readObject();
        System.out.println("Nhận được user: " + user.getName() 
            + ", tuổi: " + user.getAge());
        
        ois.close();
        client.close();
        server.close();
    }
}
```

### Client gửi Object

```java
public class ObjectClient {
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("localhost", 8888);
        
        ObjectOutputStream oos = new ObjectOutputStream(
            socket.getOutputStream()
        );
        
        User user = new User("Gia Lâm", 20, "secret123");
        oos.writeObject(user);
        
        System.out.println("Đã gửi user!");
        
        oos.close();
        socket.close();
    }
}
```

## Lưu ý bảo mật

- Không deserialize dữ liệu từ nguồn không tin cậy
- Sử dụng `serialVersionUID` để kiểm soát phiên bản
- Dùng `transient` cho các field nhạy cảm

## Kết luận

Serialization giúp việc truyền dữ liệu object phức tạp qua mạng trở nên đơn giản. Tuy nhiên cần chú ý đến bảo mật khi sử dụng.
