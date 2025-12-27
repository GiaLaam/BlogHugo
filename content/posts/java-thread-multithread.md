---
title: "Lập Trình Đa Luồng (Multithreading) trong Java"
date: 2024-12-18
tags: ["Java", "Thread", "Multithreading"]
---

## Thread là gì?

Thread (luồng) là đơn vị thực thi nhỏ nhất trong một chương trình. Multithreading cho phép chạy nhiều tác vụ đồng thời, tăng hiệu suất ứng dụng.

## Tạo Thread trong Java

### Cách 1: Kế thừa lớp Thread

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Thread: " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // Bắt đầu thread
    }
}
```

### Cách 2: Implement interface Runnable

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable đang chạy!");
    }
    
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
    }
}
```

## Đồng bộ hóa (Synchronization)

Khi nhiều thread truy cập cùng một tài nguyên, cần đồng bộ hóa để tránh xung đột:

```java
public class Counter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public int getCount() {
        return count;
    }
}
```

## Ứng dụng trong lập trình mạng

Multithreading rất quan trọng trong server để xử lý nhiều client đồng thời:

```java
public class MultiThreadServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8080);
        
        while (true) {
            Socket clientSocket = serverSocket.accept();
            // Tạo thread mới cho mỗi client
            new Thread(() -> handleClient(clientSocket)).start();
        }
    }
    
    static void handleClient(Socket socket) {
        // Xử lý client
    }
}
```

## Kết luận

Multithreading là kỹ năng thiết yếu cho lập trình viên Java, đặc biệt khi xây dựng các ứng dụng server có hiệu suất cao.
