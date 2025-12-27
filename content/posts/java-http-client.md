---
title: "Sử dụng HttpClient trong Java 11+"
date: 2024-12-15
tags: ["Java", "HTTP", "API"]
---

## Giới thiệu HttpClient

Từ Java 11, `HttpClient` được giới thiệu như một API hiện đại để thực hiện các HTTP request, thay thế cho `HttpURLConnection` cũ.

## Tạo HttpClient

```java
import java.net.http.HttpClient;
import java.time.Duration;

HttpClient client = HttpClient.newBuilder()
    .version(HttpClient.Version.HTTP_2)
    .connectTimeout(Duration.ofSeconds(10))
    .build();
```

## GET Request

```java
import java.net.URI;
import java.net.http.*;

public class GetExample {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.github.com/users/GiaLaam"))
            .header("Accept", "application/json")
            .GET()
            .build();
        
        HttpResponse<String> response = client.send(
            request, 
            HttpResponse.BodyHandlers.ofString()
        );
        
        System.out.println("Status: " + response.statusCode());
        System.out.println("Body: " + response.body());
    }
}
```

## POST Request

```java
public class PostExample {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        
        String json = "{\"name\":\"Gia Lam\",\"email\":\"test@example.com\"}";
        
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/users"))
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(json))
            .build();
        
        HttpResponse<String> response = client.send(
            request,
            HttpResponse.BodyHandlers.ofString()
        );
        
        System.out.println("Response: " + response.body());
    }
}
```

## Async Request

```java
public class AsyncExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.github.com"))
            .build();
        
        client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
            .thenApply(HttpResponse::body)
            .thenAccept(System.out::println)
            .join();
    }
}
```

## Kết luận

`HttpClient` trong Java 11+ cung cấp API hiện đại, hỗ trợ HTTP/2 và async, rất phù hợp cho việc xây dựng các ứng dụng gọi REST API.
